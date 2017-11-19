# High Level Architecture Overview
## Introduction
At its core, the Paratii system facilitates decentralized content distribution and ad exchange through the following core components:
- The Paratii Player
- Decentralized video storage
- Decentralized metadata storage
- Recommendation engine(s)
- The PTI Token

A series of smart contracts, deployed on the Ethereum blockchain, glue these systems together, and will largely be maintained by the [Paratii Foundation](../Paratii-Foundation.md). The Paratii Player consists of a video player that allows viewers to download, rate, and otherwise interact with videos on the Paratii System. The videos will be stored according to their hashes within the IFPS content addressable peer to peer file system. Metadata about users and videos, such as what videos a user owns, buys, likes/dislikes, and flags, will be continuously updated through peer to peer communication using the [y.js](http://y-js.org/) framework with snapshots periodically synced to an IPFS backend as seen in [Peerpad](https://peerpad.net/). 

Within any video distribution system with ads, the following incentives must align:
1. Viewers must be able to watch content
2. Advertisers must be able to compete for users attention

Within a traditional ad exchange, advertisers bid on a user's attention when watching a video. A producer may track a user's history and provide it to the advertiser to determine how much the user's attention is worth. The advertiser with the highest bid gets their ad shown to the user. This system has a number of problems:

- Everyone must trust the third party hosting the exchange
- Producers must reliably track viewer data
- Advertisers may need to pay the third party to join an exchange

Within Paratii's decentralized content distribution framework, user data is recorded on a distributed database that anyone can access. This facilitates competition analyzing user data and predicting who is most likely to actually click an ad.

Within the Paratii system, users can pay for videos with PTI. The transaction takes place through a series of micropayments through an offchain state channel as they download chunks of the video from IPFS. This download is public on the blockchain. Advertisers can bid in realtime to have their ad segments injected into the download stream. Advertisers bid for their videos to be downloaded with PTI. When the user views ad segments, they recieve some of advertiser's PTI the used to bid on the ad space. The content provider receives some other proportion of the bid, and a proportion of the ad revenue goes to a redistribution fund that will help facilitate new users joining the network. Advertisers will be given an opportunity to bid on a producer's video when a user begins to watch the user's video through the [Paratii player](../Paratii-Player.md). They may outsource the valuation advertising on a particular view to a third party [recommendation engines](../Recommendation-Engine.md) which will help determine the value of a particular user watching a particular video, to each ad an advertiser has available using the user's current history as recorded within Paratii's public [metadatadata storage](../The-Paratii-Protocol/Metadata-Storage.md).

## Smart Contracts
The Paratii Player will rely on a system of smart contracts to connect users to the Ethereum blockchain, and to the video content stored on IPFS. The Paratii Architecture can primrily be broken down into *administrative* contracts that primarily deal with the core infrastructure, and a series of *action* contracts that intract with users, videos, and metadata.

The following contracts define Paratii's aministrative contracts:
- `ParatiiRegistry`: Tracks addresses of contracts as they are updated
- `ParatiiAvatar`: Spends ETH and/or PTI on behalf of the user, collects redistribution funds
- `ParatiiToken`: ERC20 PTI token

These contracts are expected to remain unchanged. By contrast, the action contracts may be updated, and are expected to grow in number as Paratii develops new user/video interactions. These contracts interact with a series of action contracts that perform the following actions:
1. Trigger one or more `web.js` hooks through an `Event`
2. Log the interaction within Paratii's [metadata database](../Paratii-Protocol/Metadata-Storage.md)

Paratii currently implements the following action contracts:
1. `UserRegistry`: Tracks:
    - User metadata
    - Video ownership
    - Likes/Dislikes
2. `VideoRegistry`: Tracks:
    - Video Metadata
    - IPFS hash
3. `VideoStore`: Tracks:
    - Ownership of videos
    - Price of videos

In addition, Paratii currently uses a small utility contract, `SendEther` which sends ETH from the application and logs an event that is read by the client to log transaction history.
