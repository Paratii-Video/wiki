# High Level Architecture Overview
# Introduction
The Paratii system is a completely decentralized and incentivized video distribution system. It is designed to exist as an ecosystem supported entirely through peer to peer communication. Users are able to view, buy, and sell video content on demand. A traditional video exchange operates as a for-profit service that provides videos users *want* to see in exchange for ads users *do not want* to see. The advertising agents pay this middleman for the unwanted attention its viewers. The content provider often supplies user statistics so advertisers can better decide how much an individual's attention is worth in realtime.

Paratii aims to provide a decentralized exchange of tokens for video content. This will allow advertisers and consumers to set their own conditions for providing video content. We hope this will help remove friction and facilitate better prices for viewers and content producers. Paratii assumes videos can exist on a spectrum from advertisements in which advertisers pay for unwilling viewers to watch, and desirable content which viewers will pay to watch. Within this model, viewers and content producers (which may produce advertisements, desirable content, or anything in between), can act as either the bidder or the asker in a decentralized exchange. Viewers will be given the option to either be paid to watch advertisements or spend their tokens to watch desirable content.

When a user wants to be paid to watch content, they will recieve videos that maximize the rate at which they are paid by producers of advertisements to watch videos. Their anonymized viewing and rating history, as available on the ethereum blockchain, will be scrutinized by advertisers. These advertisers will bid on the user's time spent watching particular advertisements that the advertiser produced. The advertiser who bids the highest on the user's time will have their accompanying video sent to the user in small segments. The user will recieve payment in PTI at the rate at which the advertiser bidded as they watch each segment.

Users will, alternatively, be able to request desirable content in exchange for PTI. As bidders, they will be given an option to specify the maximum rate at which they are willing to pay for video content. Content producers will be able to place asks on a user's time spent watching videos in exactly the same manner as advertisers do when users ask to be paid to watch content. Simply by specifying a negative bid, a content producer can seek to maximize their profits by adjusting their prices based on a user's profile. Users will pay for content through micropayements as the receive the content in small segments. This is done in exactly the same way as when users are paid to watch advertisements.

# Micropayments
Whether the viewer is buying or selling their time, they will be charged or paid incrementally with offchain micropayments for watching individual segments of the content they are sent. At any time, the viewer can pause or stop watching a video, at which point the channel will be closed and their funds will either be transferred to (if they are buying content) or received from (if they are selling their time) the content producer.

# User Feedback
At the end of each video, a user is given the opportunity to vote with a "thumbs up" or "thumbs down" response. A positive "thumbs up" response indicates the viewer wishes to receive more of such content, and a negative "thumbs down" response indicates the viewer would want to be paid more (or pay less) for similar content. These transactions allow content producers to build a profile of each user that enables them to predict what types of content each user would want to watch. They can then match these preferences against their own content to determine where to place bids/asks for different users and different videos in their video library.

# Anonymity and Sybil Resistance
Every transaction will be recorded publicly. This includes who watched what videos at what price from who and for how long. This also includes any ratings a user provides on a video. This information is essential for content producers to determine how much a user's time watching their content is worth. Users may create as many accounts as they like and chose whether to store any personally identifying information on any of them. This will be largely irrelavent to content producers because they only see ethereum addresses with a series of tranactions on the blockchain. More user history means a better opportunity for content producers to place more directed content and be more confident in the prices they set. There is no opportunity for a denial of service or sybil attack to disrupt the system because the entire economic basis of the Paratii system is built to accomodate anonymous users with multiple accounts.

# Decentralized Video Storage and Transcoding
Since there are already mainstream efforts to build incentivized filestorage, Paratii currently intends to interface with an emerging solution rather than implement one from scratch. Paratii currently plans to work with [IFPS](https://ipfs.io/) rather than [SWARM](http://swarm-guide.readthedocs.io/en/latest/introduction.html) since IPFS is currently more mature. Paratii may switch or port to SWARM in the future. Uploaded videos will have to be available on demand in common formats on IPFS. To help facilitate this, Paratii will support incentivized transcoding. Borrowing [Livepeer's protoocol](https://github.com/livepeer/wiki/blob/master/WHITEPAPER.md#broadcast--transcoding-job), uploaders will broadcast a job of the form `Job(StreamID, TranscodingOptions, PricePerSegment)`.

# Smart Contracts
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
