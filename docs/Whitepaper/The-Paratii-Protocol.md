# Paratii Protocol
The Paratii Protocol governs all forms of peer to peer communication within the Paratii ecosystem. Buyers will purchase video segments via micropayments over IPFS using `libp2p`. Publishers will upload video content to IPFS and register its hash on a smart contract. Viewers will purchase the video through the smart contract and download the video over IPFS. The viewer is expected to send receipts to the nodes storing and serving the video in question, who will in turn exchange these receipts for micropayments on the blockchain. In this way, the viewers pay publishers for hosting content, or in turn incentivize IPFS nodes to serve it. The Parati protocol authenticates transactions at each step of this system.

# Uploading and Registering Videos
Users will be able to upload videos to IPFS with their Paratii Uploader Tool. Their local video will be sent through a transcoder, and uploaded to IFPS in several common formats. The hash of the video in each format, as stored on IPFS, will be registered within the `VideoRegistry` contract as part of the following metadata.

```
{
  title: <title>,
  owner: <owner wallet address>,
  duration: <duration in seconds>,
  id: <video id>,
  formats: {
    <format 1>: {
      id: <id>,
      price: <PTI>,
      size: <size in bytes>
    },
    ...
  }
}
```

All metadata within records on the `VideoRegistry` can be changed by the `owner`. The `price` is the price in PTI that the publisher charges to manage the smart contracts. The `fee` is the fee offered to IPFS nodes who help download the video to the viewer. The `id` field is a unique identifier for the video. This can be a hash of a JSON document consisting of all other metadata when the video was first registered.

## Uploading at the Portal
Users will be prompted to upload their video at a web portal, which will respond wtih a screen requesting an estimated price in PTI and ETH for the transcoding job. The user will fill out a form with the metadata used to register the video on the Ethereum blockchain within the `VideoRegistry.sol` contract and send the requested amount of PIT or ETH to an escrow service. On the backend, the portal will initiate an IPFS node, which uploads the raw video file to IPFS, and recieves a hash `rawHash`. 

The portal then submits a transcoding job to `TranscodingJobs.sol` referenced by `rawHash`, together with the amount of ETH or PTI uploaded to the escrow service. The uploader will be given the opportunity to cancel the request before the job is accepted. Meanwhile, transcoding agents will scans and accepts jobs submitted to `TranscodingJobs.sol`. The transcoder will keep the money should an error occur due to an invalid file uploaded by the uploader. The funds will remain in the escrow should the transcoder fail to transcode the video at the settled price, which the user can reclaim at the uploading portal.

Upon successfully transcoding the video, tbe transcoding agent will register a resulting `ipfsHash` within the `Videos.sol` contract and update the `TranscodingJobs.sol` contract to reflect the job's completion. At this point, the transcoding agent will receive the funds locked in the escrow.

Paratii will host a centralized transcoding service during its first iteration, and move on to a decentralized one in the future.

# Buying a Video
When a viewer purchases a video, they will send the following data to the publisher.

```
{
  id: <id>,
  format: <format>,
  walletSignature: <walletSignature>,
  IPFSSignature: <IPFSSignature>
}
```

`walletSignature` signs a JSON document containing all other fields. That is,

 ```
{
  id: <id>,
  format: <format>,
  IPFSSignature: <IPFSSignature>
}
```

`IPFSSignature` similarly signs a JSON document containing all other fields. That is,

 ```
{
  id: <id>,
  format: <format>,
  walletSignature: <walletSignature>
}
```

The publisher will submit this information to Paratii's micropayment contract, which will extract exactly enough PTI from the buyer's wallet to pay the price of the video in the requested format as regiseted in the `VideoRegistry`. The contract will use the `id` and `format` fields to determine the `owner` from the `VideoRegistry`. 

```
nonce = HASH(
  {
    id: <id>,
    format: <format>,
    walletSignature: <walletSignature>,
    IPFSSignature: <IPFSSignature>
  }
)
```

When the viewer's Paratii Player queries IPFS for the video, the player will receive the requested data from the nodes that store them on IPFS. The player will reply to any node that sends it data with the following packet.

```
{
  nonce: <nonce>, 
  owner: <video owner's IFPS public key>,
  bytes: <number of bytes received>,
  hash: <hash of received data>,
  key: <sender's public key>,
  buyerSignature: <buyer'IPFS public key signature>
}
```

`buyerSignature` signs a JSON document containing all other fields. That is,

```
{
  nonce: <nonce>,
  owner: <video owner's IFPS public key>,
  bytes: <number of bytes received>,
  hash: <hash of received data>,
  key: <sender's public key>
}
```

`key` contains the public key of the IFPS node that sent the chunk. This micropayment effectively verifies that the buyer recieved `bytes` bytes of data pertaining to the purchase referenced by `nonce` from the owner of the IPFS node with public key `key. To reclaim their payment, the sender will forward their payment to the `owner` referenced in the payment with a new signature. That is, the owner will receive receipts in the following form.

```
{
  nonce: <nonce>, 
  owner: <video owner's IFPS public key>,
  bytes: <number of bytes received>,
  hash: <hash of received data>,
  key: <sender's public key>,
  buyerSignature: <buyer IPFS public key signature>,
  senderSignature: <sender IPFS public key signature>
}
```

`senderSignature` signs a JSON document containing all other fields. That is,

```
{
  nonce: <nonce>, 
  owner: <video owner's IFPS public key>,
  bytes: <number of bytes received>,
  hash: <hash of received data>,
  key: <sender's public key>,
  buyerSignature: <buyer IPFS public key signature>,
  senderSignature: <sender IPFS public key signature>
}
```

The owner may then submit the payment to the payment channel, which will assign the appropriate payouts to the owner and sender. In practice, several IPFS nodes may be responsible for serving different parts of the video, and each will recieve payments according to the proportion of the video they contributed.

Note that within this system, it is always the publisher's job to spend ETH communicating with smart contracts. If the Paratii system succeeds, publishers will sell PTI to buyers through exchanges, thereby circulating earned PTI in exchange for the ETH necessary to run the system.

# Advertising
Advertising, that is buying attention, works in an analagous way to buying video content. Publishers may decide to list advertisements at a *negative* price. This is to say the viewer will earn money from a sponser to watch the video content. The cash flow must reverse such that the viewer is paid by the publisher rather than vice veresa. A publisher can advertise a negative price and a negative fee to be paid to the content provider seeding blocks. This of course raises the question of whether any of these parties is expected to be the advertiser and if not, how an advertising agent is expected to pay the viewer for their time wotching a video. Paratii will aim to mirror the existing, centralized model of advertising in which the advertiser is separate from the publisher and the data seeder. However, the advertiser must execute their own analysis to determine whether a viewer is real and watching their ads. This can be outsourced to a third party, but since there currently exists no universal solution to "proof of view" to date, any would-be advertiser would need to develop their own solution to this problem. They would likey have to outsource this effort to third party data miners that track which addresses have bought which videos, and determined who is likey to actually watch their video.

The advertiser will publish their ad as a video with a *negative* price for the video, and a positive one for the fee paid to whoever sucessfully serves the video content. When a negative price is registered on the `VideoRegistry`, the advertiser is expected to mantain enough funds in the wallet registered in the `VideoRegistry` to incentivize content providers and viewers. The process will oherwise look similar to buying a video. The user will send an initial packet to the publisher as though they are purchasing a video.

```
{
  id: <id>,
  format: <format>,
  walletSignature: <walletSignature>,
  IPFSSignature: <IPFSSignature>
}
```

This will be submitted to the Paratii micropayment smart contract, which will open a payment channel as usual. However, since the price of the video is negative, the funds necessary to pay the video will be deducted from the `owner`'s address if they have sufficient funds to pay someone to view the entire video at the rate advertised on the `VideoRegistry`. The rest of the protocol proceeds as usual, where the advertiser is assumed to publish their own content, and therefore will rightly be responsible for paying any ETH necessary to communicate with smart contracts.

Care must be taken to generate a proper proof of view. Since data is already content addressed by its hash on IPFS, a dishonest viewer could simply submit receipts of the chunks of video without actually watching them. Where the viewer previously sent payments that referenced the hash of a segment and the IFPS node that sent it, the viewer will now send back a *receipt* in a similar format.
  
```
{
  nonce: <nonce>, 
  owner: <video owner's IFPS public key>,
  bytes: <number of bytes received>,
  hash: <hash of received data + nonce>,
  key: <sender's public key>,
  viewerSignature: <viewer's IPFS public key signature>
}
```

The only difference between a receipt and a payment is that rather than sending back the hash of the data segment, the viewer must append the `nonce` to the data segment and hash *that* as proof they viewed the video. Note the viewer can only be paid for watching an advertisement once in this scheme. This is because once they have the raw video data, a viewer can always just *pretend* to have rewatched the video with hash based challenges. That is, if submitting a hash is a proof of view, there is no feasible way to distinguish viewing twice from viewing once, caching the data, and submitting a new hash that satisfies a new rule. For example, appending a timestamp might naively solve the problem, but the viewer could just as easily cache their data and send back hashes with new timestamps. A viewer may continue to earn money from the advertiser by serving the data on IFPS to new viewers.

It is the advertiser's duty to track who has already watched which parts of their ads. They would be advised to organize their records by `nonce` and storing a list of `hash of segment + nonce` segments for which they paid a viewer with the given `nonce`.
