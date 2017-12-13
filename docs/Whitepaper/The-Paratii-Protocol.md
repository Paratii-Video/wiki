# Paratii Protocol
The Paratii Protocol governs all forms of peer to peer communication within the Paratii ecosystem. The majority of communication will be facilitated offchain via IPFS pubsub. All users will subscribe to the topic given by the public key of their Paratii wallet. All messages published to this topic should be signed by the public key of the sender's Paratii wallet and encryptied with that same public key. Only data necessary to establish an initial offchain connection will be stored on the Ethereum blockchain via Paratii's smart contracts.

# Uploading Videos
Users will be able to upload videos to IPFS with a simple uploader tool. The interface will allow the user to select a video from their local hard drive, which will be streamed through Livepeer for transcoding. The output streams (in various common formats) will be gathered and segmented, each of which will be individually uploaded to IPFS. Finally, a key will be generated, unique to the uploader's Paratii wallet private key and the hash of the entire transcoded stream, which will be used to encrypt a list of the hashes of each segment of video in sequential order.

```
{
  format 1: Encrypt(
  {[
    hash of segment 1,
    hash of segment 2,
    ...
  ]}
  ),
)
  format 2: Encrypt(
  {[
    hash of segment 1,
    hash of segment 2,
    ...
  ]}
  ),
...
}
```

This json data will be uploaded to IPFS and its hash will be registered on the blockchain with the `VideoRegistry` contract with the following metadata:

```
{
  ipfs hash: <hash>,
  owner: <uploader wallet address>,
  title: <title>,
  duration: <duration in seconds>,
  segment: {length: <seconds>, size: <size in kbytes>}
  formats: [<available formats>]
}
```

The keys used to encrypt each list of segments will be stored on the uploader's local computer in the following form:

```
{
  ipfs hash: <hash>,
  uplaod time: <timestamp>
  formats: {
    format 1: <key to decrypt hash of segment list in format 1>,
    format 2: <key to decrypt hash of segment list in format 2>,
    ...
}
```

# The Exchange
Before streaming can begin, viewers will be allowed to bid on content they would like to watch, and producers will be able to bid on viewers to whom they wish to sell their content.

## Public Data
The following data is necessary to bootstrap a connection and authenticate senders and recievers:
- Public key of owner
- ipfs hash of video data
- Available foramts of video
- Length of videos (in seconds)
- Chunksize (in seconds) of video chunks stored on IFPS

This will all be stored on the Ethereum blockchain within Paratii's smart contracts.

## Bidding on Content
Video content will be registered by its IPFS hash on the Ethereum blockchain. The `VideoRegistry` contract will associate this hash with the video's owner's IPFS public key and the length of the video. When a user wants to bid on a video, they will subscribe to a topic given by the seller's public key as registered on the blockchain and publish a bid to that topic with the following paylod:

```
{
  bid: [PTI/second],
  format: [video format]
  signature: [signature]
}
```

Where `signature` contains a signature of the bid with the public key of the bidder's wallet. If the owner accepts the bid, they will acknowledge the bid by publishing a message with the bidder's public key as the topic with the following paylaod.

```
{
  bid: [accepted bid]
  address: [ipfs hash]
  signature: [signature]
}
```

The bidder may submit this payload along with the funds necessary to stream the entire video to a Paratii payment channel. The bidder will stream the video in multiples of its registered chunksize by publishing signed micropayments to the seller's public key.

### Bidding On Attention
Producers may bid on a viewer's attention using a similar protocol to how viewers may bid on content. They place a bid with the usual payload:

```
{
  bid: [PTI/second],
  chunksize: [seconds]
  length: [seconds]
  signature: [signature]
}
```
If the viewr accepts the bid, they will acknowledge the bid by publishing a message with the bidder's public key as the topic with the following paylaod.

```
{
  bid: [accepted bid]
  signature: [signature]
}
```

The bidder may submit this payload along with the funds necessary to stream the entire video to a Paratii payment channel. The bidder will publish micropayments to the viewer's public key channel along with each chunk. The viwer, in turn, replies with the hash of each chunk they receive as a crude proof of view. 

## Security and Privacy
All messages published to a public key topic must be encrypted with that same public key. This will ensure that no one can spy on bids made for different videos, and provides plausible deniability for bidding on specific videos published by a particular user.

## Prematurely closing microchannels
Note that anyone can close a microchannel at any time, thereby terminating a stream. This is both for the safety of the viewer, and the security of the producer.  
