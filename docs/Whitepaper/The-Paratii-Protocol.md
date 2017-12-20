# Paratii Protocol
The Paratii Protocol governs all forms of peer to peer communication within the Paratii ecosystem. Buyers will purchase video segments via micropayments over IPFS using `libp2p`. Publishers will upload video content to IPFS and register its hash on a smart contract. Viewers will purchase the video through the smart contract and stream the video over IPFS. The viewer is expected to send receipts to the nodes storing and serving the video in question, who will in turn exchange these receipts for micropayments on the blockchain. In this way, the viewers pay publishers for hosting content, or in turn incentivize IPFS nodes to serve it. The Parati protocol authenticates transactions at each step of this system.

# Uploading and Registering Videos
Users will be able to upload videos to IPFS with their Paratii Uploader Tool. Their local video will be streamed through Livepeer for transcoding, and uploaded to IFPS in several common formats. The hash of the video in each format, as stored on IPFS, will be registered within the `VideoRegistry` contract as part of the following metadata.

```
{
  title: <title>,
  owner: <owner wallet address>,
  duration: <duration in seconds>,
  id: <video id>,
  formats: {
    <format 1>: {
      hash: <ipfs hash>,
      price: <PTI>,
      fee: <PTI/byte>,
      ttl: <ttl of micropayment channel>,
      size: <size in bytes>
    },
    ...
  }
}
```

All metadata within records on the `VideoRegistry` can be changed by the `owner`. The `price` is the price in PTI that the publisher charges to manage the smart contracts. The `fee` is the fee offered to IPFS nodes who help stream the video to the viewer. Finally, `ttl` determines the maximum lifetime of the payment channel that binds the buyer, publisher, and any parties that stream the video. The `id` field is a unique identifier for the video. This can be a hash of a JSON document consisting of all other metadata when the video was first registered.

# Buying a Video
When a viewer purchases a video, they will send the following data to the publisher.

```
{
  id: <id>,
  format: <format>,
  wallet_signature: <wallet_signature>,
  IPFS_signature: <IPFS_signature>
}
```

`wallet_signature` signs a JSON document containing all other fields. That is,

 ```
{
  id: <id>,
  format: <format>,
  IPFS_signature: <IPFS_signature>
}
```

`IPFS_signature` similarly signs a JSON document containing all other fields. That is,

 ```
{
  id: <id>,
  format: <format>,
  wallet_signature: <wallet_signature>
}
```

The publisher will submit this information to Paratii's micropayment contract, which will extract from the buyer's wallet exactly enough PTI to pay the price of the video in the requested format as regiseted in the `VideoRegistry`. The contract will use the `id` and `format` fields to determine the `ttl` and the `owner` from the `VideoRegistry`. The newly opened payment channel is referenced by a `nonce` given by the hash of the JSON document encoding the parameters sent to the publisher. That is,

```
nonce = HASH(
  {
    id: <id>,
    format: <format>,
    wallet_signature: <wallet_signature>,
    IPFS_signature: <IPFS_signature>
  }
)
```

When the viewer's Paratii Player queries IPFS for the video, the player will receive chunks of data from the nodes that store them on IPFS. The player will reply to any node that sends it data with the following packet.

```
{
  nonce: <nonce>, 
  owner: <video owner's IFPS public key>,
  bytes: <number of bytes received>,
  hash: <hash of received data>,
  key: <sender's public key>,
  buyer_signature: <buyer'IPFS public key signature>
}
```

`buyer_signature` signs a JSON document containing all other fields. That is,

```
{
  nonce: <nonce>,
  owner: <video owner's IFPS public key>,
  bytes: <number of bytes received>,
  hash: <hash of received data>,
  key: <sender's public key>
}
```

`key` contains the public key of the IFPS node that sent the chunk. This micropayment effectively verifies that the buyer recieved `bytes` bytes of data pertaining to the purchase referenced by `nonce` from the owner of the IPFS node with public key `key. To reclaim their payment, the sender will forward their micropayment to the `owner` referenced in the micropayment with a new signature that signs the entire micropayment. That is, the owner will receive receipts in the following form.

```
{
  nonce: <nonce>, 
  owner: <video owner's IFPS public key>,
  bytes: <number of bytes received>,
  hash: <hash of received data>,
  key: <sender's public key>,
  buyer_signatur: <buyer IPFS public key signature>,
  sender_signature: <sender IPFS public key signature>
}
```

`sender_signature` signs a JSON document containing all other fields. That is,

```
{
  nonce: <nonce>, 
  owner: <video owner's IFPS public key>,
  bytes: <number of bytes received>,
  hash: <hash of received data>,
  key: <sender's public key>,
  buyer_signatur: <buyer IPFS public key signature>,
  sender_signature: <sender IPFS public key signature>
}
```

The owner may then submit the payment to the micropayment channel, which will assign the appropriate payouts to the owner and sender.
