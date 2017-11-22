# High Level Architecture Overview
## Introduction
The Paratii system is a completely decentralized and incentivized video distribution system. It is designed to exist as an ecosystem supported entirely through peer to peer communication. Users are able to view, buy, and sell video content on demand. A traditional video exchange operates as a for-profit service that provides videos users *want* to see in exchange for ads users *do not want* to see. The advertising agents pay this middleman for the unwanted attention its viewers. The content provider often supplies user statistics so advertisers can better decide how much an individual's attention is worth in realtime.

Paratii aims to provide a decentralized exchange of tokens for video content. This will allow advertisers and consumers to set their own conditions for providing video content. We hope this will help remove friction and facilitate better prices for viewers and content producers. Paratii assumes videos can exist on a spectrum from advertisements in which advertisers pay for unwilling viewers to watch, and desirable content which viewers will pay to watch. Within this model, viewers and content producers (which may produce advertisements, desirable content, or anything in between), can act as either the bidder or the asker in a decentralized exchange. Viewers will be given the option to either be paid to watch advertisements or spend their tokens to watch desirable content.

When a user wants to be paid to watch content, they will recieve videos that maximize the rate at which they are paid by producers of advertisements to watch videos. Their anonymized viewing and rating history, as available on the ethereum blockchain, will be scrutinized by advertisers. These advertisers will bid on the user's time spent watching particular advertisements that the advertiser produced. The advertiser who bids the highest on the user's time will have their accompanying video sent to the user in small segments. The user will recieve payment in PTI at the rate at which the advertiser bidded as they watch each segment.

Users will, alternatively, be given the option of requesting desirable content in exchange for PTI. As bidders, they will be given an option to specify the maximum rate at which they are willing to pay for video content. Content producers will be able to place asks on a user's time spent watching videos in exactly the same manner as advertisers do when users ask to be paid to watch content. Simply by specifying a negative bid, a content producer can seek to maximize their profits by adjusting their prices based on a user's profile. Users will pay for content through micropayements as the receive the content in small segments. This is done in exactly the same way as when users are paid to watch advertisements.

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
