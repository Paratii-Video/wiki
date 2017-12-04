# Architecture Overview
Roughly, what's needed to share a video over an internet connection is to get the source file to be stored, multiplexed and distributed - usually with the aid of a client that wraps the output of the process and presents it in a human-digestible form, e.g. a media player.

The current paradigm relegates the abovementioned range of services to middlemen, under tacit agreements that offer "free" access to content distribution in exchange for the right to privately monetise user data and tax creators' earnings. The `paratii-protocol` leverages networks which are permissionless by design to subvert this paradigm, aiming to establish an open market for video and audience that:

- Gives back all value to its contributors, without the accrue of profits for entities external to the system.
- Bundles access to all base-layer protocols and distributed services needed to serve video over the web without intermediaries, presenting the lowest friction possible to creators, publishers and end-users, and allowing each to contribute up to its own will.
- Is hardwired to move towards fully distributed computation, hosting, and control.

At its core, Paratii is composed of a modular system that relies on:

- Smart contracts running on the EVM, for logic processing, account handling and settlement of payments.
- The InterPlanetary File System (IPFS) for distributed file storage, currently through the `js-ipfs` implementation.

and is under way to incorporate:

- The Livepeer *2nd-layer infrastructure* protocol, for incentivised transcoding of on-demand streamed videos.
- InterPlanetary Linked Data (IPLD) for formatting metadata under cross-industry standards.

## Relevant Levels of Data

- message: encompass wantlists, block references, chunk hashes, payload, signed / non-signed **(to be spec'd)**.
- chunk: the data unit of storage at the heart of content-addressing, determiniscally pointed to by a unique hash. Relevant to storage operations, besides delivery/request and accounting.  
- address: unique identifier of an agent in the network.
- video: an IPFS multihash, relevant to high-level APIs.

## Message format

Nodes running the `paratii-protocol` are connected over libp2p and suited to do a basic set of operations:

**(WIP: @aeftimia / @ya7ya : we got to properly spec this!)**

- Encode a message as in IPFS protobuf, transforming it into a series of key-value pairs and concatenating them into a bytestream.
- Exchange and verify a message (`get`/`put` PTI/ETH address; checks if it's a valid account).
- Deal with payload **(arbitrary arguments to be spec'd)**, which can be a multihash in the case of a video (chunks a file into 1mb chunks, hashes them, seeks for the closest transcoding node, sends a jobRequest and asks for the chunks to be `get`, the transcoding node uploads its job to IPFS and gives back its multihash along links for thumbs/screenshots).
- Signs messages (not default in IPFS) and runs a slightly modified `bitswap` that allows for custom `chunkSize` in order to optimise performance for browser-to-browser communication.

## Smart Contracts
The system can be broken down into *administrative* contracts that primarily deal with the core infrastructure, and a series of *action* contracts that interact with users, videos, and metadata.

The following define Paratii's *administrative* contracts:

### ParatiiRegistry 
The [ParatiiRegistry.sol](https://github.com/Paratii-Video/paratii-contracts/blob/master/contracts/paratii/ParatiiRegistry.sol) contract is a simple key-value store on the blockchain that holds Paratii general settings. In particular, this is the place where the addresses of the deployed Paratii contracts are stored.

For example, the following call will get the address of the `ParatiiToken` contract:

    let paratiiRegistery = paratii.contracts.ParatiiRegistry
    paratiiRegistry.getAddress('ParatiiToken')`
    
The `ParatiiRegistry` is an `Ownable` contract, and contains simple setters and getters for several solidity types:

    ParatiiRegistry.setAddress('UserRegistry', '0x12345') // can only be called by the owner of the contract instance
    ParatiiRegistry.setUint('a-useful-constant', 99999)
    ParatiiRegistry.getUint('a-useful-constant') // will return 99999
    
### ParatiiToken

[ParatiiToken.sol](https://github.com/Paratii-Video/paratii-lib/blob/master/contracts/paratii/ParatiiToken.sol) is an ERC20 token contract and contains the balances of all Paratii account holders.

    paratiToken.balanceOf('0x123') // will return the balance of the given address
    
### ParatiiAvatar

[ParatiiAvatar.sol](https://github.com/Paratii-Video/paratii-lib/blob/master/contracts/paratii/ParatiiAvatar.sol) is a contract that can send and receive ETH and other ERC20 tokens, designed to collect and desimburse redistribution funds. It is controlled by a number of whitelisted addresses.

    erc20Token = 0x12345667
    paratiiAvatar.transferFrom(erc20Token, fromAddress, toAddress, amount) // will fail because you are not whitelisted
    paratiiAvatar.addToWhitelist(0x22222222)
    paratiiAvatar.transferFrom(erc20Token, fromAddress, toAddress, amount, { from: 0x22222222}) // sender is 0x2222, whihc is in the whitelist
    paratiiAvatar.transfer(toAddress, amount) // transfer some ether to toAddress
    
These contracts are expected to remain unchanged. By contrast, the *action* contracts may be updated, and are expected to grow in number as Paratii develops new features. *Administrative* contracts interact with a series of *action* contracts in a simple pattern:
1. Trigger one or more `web.js` hooks through an `Event`.
2. Log the interaction within Paratii's [metadata database](../Paratii-Protocol/Metadata-Storage.md).

These *action* contracts currently are:

### UserRegistry

A registry with information about users: user metadata, upvote/downvote history, ownership of videos.

### VideoRegistry

[VideoRegistry.sol](https://github.com/Paratii-Video/paratii-contracts/blob/master/contracts/paratii/VideoRegistry.sol) contains information about videos: their IPFS hash, its owner, and the price. This contract only stores essential information: additional metadata (duration, license, descriptions, etc etc) can be stored in IPFS.

### VideoStore

The `VideoStore` is a contracts that tracks ownership and prices of videos (be it negative or positive). Buying (i.e. pay-per-view), for example, will register a purchase, and split the money being sent between the `owner` of the video and the [VideoStore.sol](https://github.com/Paratii-Video/paratii-contracts/blob/master/contracts/paratii/VideoStore.sol). To do this, the user must initiate two transactions:

- the client calls `ParatiiToken.approve(ParatiiAvatar.address, price_of_video)` to allows the paratiiAvatar to transfer the price_of_video. (For small transactions, this can be done transparently)
- the client calls `VideoStore.buyVideo(videoId)`, triggering a number of steps:
 - the price of the video will be transfered to the paratiiAvatar
 - an event will be logged that the video is unlocked for this user
 - records the purchase of a video on within the `UserRegistry` allowing the new owner to like/dislike the video
 - a part of the price will be transfered (immediately?) to the owner, other goes tot he redistrubtion pool. (In the first iteration, we can give all money to thee owner)

In addition, Paratii currently uses a simple wrapper contract, [SendEther.sol](https://github.com/Paratii-Video/paratii-contracts/blob/master/contracts/paratii/SendEther.sol), that can be used to send Ether, and will log an event that can be read by clients and logged to transaction history.

The [paratii.js](https://github.com/Paratii-Video/paratii-lib/blob/master/lib/paratii.js) library is the preferred way for clients to interact with the deployed contracts, offering access to them, wallet functionality, and convenience functions that provide more useful error handling than direct calls to the blockchain. 


