# Paratii Protocol
The Paratii Protocol governs all forms of peer to peer communication within the Paratii ecosystem. The majority of communication will be facilitated offchain via IPFS pubsub. Users will generate one or more **communication keypairs** with their Paratii client (Uploader Tool or Paratii Player). For each communication keypair, the public key is optinally registered on the `UserRegistry` contract in association with the Paratii wallet address registered to the user, and optional metadata the user may set to complete their profile. Public keys may be added or revoked at any time on the `UserRegistry` contract. For each public key registered to a Paratii wallet address, the client in possession of the wallet's private key will receive notifications over `libp2p` via a local ipfs node registered with the communication keypair.

# Uploading Videos
Users will be able to upload videos to IPFS with their Paratii uploader Tool. The interface will allow the user to select a video from their local hard drive and will prompt the user to either select a preexisting communication keypair or generate a new one. If the user opts to generate a new keypair, they will be given the option of registering the public key on the Paratii `UserRegistry` if their client's wallet contains sufficient funds to do so.

Next, their local video will be streamed through Livepeer for transcoding. The output streams (in various common formats) will be gathered and segmented. Each segment of which will be individually uploaded to IPFS. Finally, a key will be generated, unique to one of the uploader's Paratii wallet private key and the hash of the entire transcoded stream, which will be used to encrypt a list of the hashes of each segment of video in sequential order. This encrypted *table of contents* will be uploaded to IFPS, and its hash added to a *directory* with the following format:

```
{
  format 1: Hash(Encrypt(
  {[
    Hash(segment 1),
    Hash(segment 2),
    ...
  ]}
  ))),
)
  format 2: Hash(Encrypt(
  {[
    Hash(segment 1),
    Hash(segment 2),
    ...
  ]}
  )),
...
}
```

This directory will be uploaded to IPFS and its hash will be registered on the blockchain with the `VideoRegistry` contract with the following metadata:

```
{
  ipfs hash: <hash>,
  owner: <communication public key>,
  title: <title>,
  duration: <duration in seconds>,
  segment: {length: <seconds>, size: <size in kbytes>},
  formats: [<available formats>]
}
```

The keys used to encrypt each list of segments will be stored on the uploader's local computer in the following form:

```
{
  ipfs hash: <directory hash>,
  upload time: <timestamp>
  formats: {
    format 1: <key to decrypt hash of segment list in format 1>,
    format 2: <key to decrypt hash of segment list in format 2>,
    ...
}
```

Finally, the client will register and maintain an IPFS node with the communication key pair.

# The Exchange
Before streaming can begin, viewers will be allowed to bid on content they would like to watch, and producers will be able to bid on viewers to whom they wish to sell their content.

## Bidding on Content
Users will send bids to a video's owner via *js-libp2p*, which will send messages in the form of JSON documents to the `connection public key` registered in the `VideoRegistry`. Where `communication public key` is the `owner` registered on the `VideoRegistry`.

The payload itself takes the following form:

```
{
  nonce: <nonce>,
  bid: <PTI/second>,
  expiration: <expiration date of bid>,
  hash: <directory hash>,
  format: <format>,
  signature: <signature>,
  wallet signature: <wallet signature>
}
```

Where `signature` contains a signature of JSON object containing the remaining data in the packet and is signed with the bidder's `communication public key`. The `nonce` is used to prevent fraud in which the recipient reuses messages. An example would be if two bids are sent and the recipient attempts to cash out on both at once. Whenever a sequence of messages sucessfully leads to establishing a micropayment channel, the nonce changes. The `signature` and `wallet signature` sign with the `communication private key` and `wallet private key` of the sender respectively. The `wallet signature` signs the following JSON document withthe private key of the Paratii wallet.

```
{
  nonce: <nonce>,
  bid: <PTI/second>,
  expiration: <expiration date of bid>,
  hash: <directory hash>,
  format: <format>,
  signature: <signature>
}
```

Similarly, `signature` signs the following JSON document.


```
{
  nonce: <nonce>,
  bid: <PTI/second>,
  expiration: <expiration date of bid>,
  hash: <directory hash>,
  format: <format>,
  wallet signature: <wallet signature>
}
```

This allows the recipient to verify the sender approves of money exchanged to and from the wallet in question, and the owner of the waller approves the sender to negotiate bids on behalf of it. Since all other data is signed too, this approval is only valid for exactly one bid.

If the owner accepts the bid, they will send the bid to the Paratii micropayment channel, which will verify its validity. The following conditiuons must be met for a state channel to activate:

- The bidder has the funds necessary to pay for the entire video.
- The nonce submitted matches the nonce associated with the wallet address in the `UserRegistry` or the wallet address is not registered on the `UserRegistry` (effectively providing anonymous viewing).
- The signatures from the submitted message are valid.

In the case of a valid bid request, the wallet's `nonce` is changed to the hash of the entire JSON document encoding the successful bid message. Within the Paratii contracts, the `nonce` is used to reference any open state channels, and its most recent value represents the last open state channel associated with the wallet address bidding. Hence `nonce`s act as a private hash chain which verify messages are sent in reference to the current state of the wallet.

The funds necessary to pay for the entire video in question are withdrawn from the bidder's account into the state channel. The bidder will then proceed to send checks to the owner as signed messages. The payments must be sent in integer multiples of the quentity `bid x segment` for a corrensponding integer number of `segments` decrypted and sent back from the owner. At any time, the seller can close the state channel by cashing the latest micropayment or the bidder can stop sending micropayments, in which case the remaining funds will be sent back to the bidder's account after `expiration`. Micropayments will be made in the form of a signed message of the form:

```
{
  nonce: <nonce>,
  balance: <segment count>,
  signature: <signature>,
  wallet signature: <wallet signature>
}
```

`signature` signs a JSON document containing all fields excluding itself (as per usual). This time, however, `wallet signature` signs a JSON document containing the following entries.

```
{
  nonce: <nonce>,
  balance: <segment count>,
}
```

This will come in handly later, but for now we can verify the extra `signature` entry is superfluous because the signature is already tied to a `nonce` validated on the blockchain that tied the owner of the wallet's private key back to the messager. We are effectively proving the wallet approved a payment, and the sender is consistent. The `balance` tracks the total number of segments sold to the bidder and will be converted to its PTI value when submited to the payment channel. In this message, the signature When this number is increased in a new message, the owner is expected to reply with the remaining segments with the following format.


```
{
  nonce: <nonce>,
  segments: {
    N: <data>,
    N + 1: <data>,
    ...
  }
  signature: <signature>
}
```

## Bidding On Attention
Advertisers may bid on a viewer's attention using a similar protocol to how viewers bid on content. They may have found the communication public key through several means including:

- combing the `UserRegistry` for registered communication public keys
- subscribing to a data mining agent
- prior communication of selling videos

The payload will take the following form.

```
{
  nonce: <nonce>,
  bid: <PTI/second>,
  expiration: <expiration date of bid>,
  hash: <directory hash>,
  format: <format>,
  signature: <signature>,
  wallet signature: <wallet signature>
}
```

Note this is exactly the same format as before. The only difference is that the owner of the video is now the bidder. The rest of the protocol is actually a mirror image of the *bidding for content* scenario, and will be breifly summarized for clarity.

A bid is placed, this time for the viewer's attention. Since the viewer does not own the video in question, they deduce that they are expected to receive and not provide the video. They submit the bid to the Paratii micropayment contract, which verifies the bidder's wallet has the necessary funds to pay for the video (now being sent rather than received), and the nonce is valid. The commonality here is that *it is only important that the sending wallet's funds are sufficient to pay for the entire video at the bidded rate and the nonce listed for the wallet is valid*. The nonce is incremented as usual, and a state channel listed under the old nonce is opened.

Streaming the video looks different here. In this case, the viewer sends requests for segments in the following format.

```
{
  nonce: <nonce>,
  segment: <N + p>,
  receipts: {
    N:     <hash of segment N>,
    N - 1: <hash of segment N - 1>,
    ...,
    N - q: <hash of segment N - q>
  }
  signature: <signature>,
}
```

In the above payload, the viewer is requesting `p` segments after segment `N`, and is displaying receipts of the previous `q` segments (presumably from the previous batch sent by the advertiser). The advertiser replies with the following packet. `signature`the hash of the JSON document containing all other fields in the sent packet.

```
{
  nonce: <nonce>,
  segments: {
    N + 1: <data>,
    N + 2: <data>,
    ...,
    N + q: <data>
  },
  balance: <segment count>,
  signature: <signature>,
  wallet signature: <wallet signature>
}
```

Again `signature` signs a JSON document containing all fields excluding itself, and again, `wallet signature` signs a JSON document containing the following entries.

```
{
  nonce: <nonce>,
  balance: <segment count>,
}
```

As before, we have verified that the wallet approved an exchange, the identity of the sender remains consistent and as before, and both reference a the nonce that binds the two to an agreement on a state channel.

Note that packets can be dropped or corrupted, but so long as the majority of the segments received remain in tact, the viewer will be able to reclaim the majority of their payments.
