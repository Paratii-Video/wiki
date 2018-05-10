# A Paratii Video Protocol

This document contains some ideas for a generic protocol for communication and negotiation between nodes in a p2p video distribution network.

[Basically, the idea is to take [my ideas for the advertisement market]( https://github.com/Paratii-Video/wiki/wiki/A-market-for-Sybils)  and  [Felipes idea of using the 0x mechanisms](https://medium.com/paratii/what-if-0x-could-be-used-for-decentralised-real-time-bidding-advertising-9fcb733d8e84) a bit further by generalizing the idea and use it for all client-activated requests in the Paratii framework. I'll also try to be a bit more precise about how to handle claims that cannot be resolved onchain (such as "proof-of-view")]

[yeah, work in progress, parts are unreadable, etc]


## 0x

Roughly, the idea of 0x can be described as follows.

**Orders** are messages that are statements of intent, like "I want to sell 3 bananas for 1 dollar". Orders are submitted and signed by a  **maker**.  Orders are then aggregated by **relayers**, which collect orders and try to find counterparties -- **takers** -- that fulfill the orders.
To fulfill the order, the taker submits the order to a smart contract.
A precondition for fulfilling the order is that both maker and taker have giving this contract can transfer the tokens referred to in the order.
The smart contract checks the validity of the data - in particular, the signature of the maker and the necessary allowances - and executes  the order.

## Generalizing 0x

The basic idea  that we can generalize the mechanisms of the 0x protocol and use it for many types of "trades" that occur in the video pipeline - for example, requests for services such transcoding or storage, and even for more complex exchanges, such as that of trading attention to an advertisement in exchange for a compensation.

The general schema is this:

* an **order** is an array  with fields like:

  - orderType : the type of order that is (ZEIP4)
  - makerToken
  - makertokenAmount: the (max) amount of tokens that the sender is willing to pay
  - taker: (ZEIP??) can be different from the sender [perhaps not needed if we use orderType]
  - takerToken
  - takerTokenAmont
  - data: a generic structure that can contain further data related to the orderType
  - a signature of the maker that signs the message


* orders are sent to (and collected by) **relayers** that try to match those orders

* In the case of on-chain trading of tokens (as in 0x), after an order is matched and signed, all information is there for the order to be settled. Requests for services cannot be settled immediately, of course, while requests for trading more subjective value, such as attention, present other kinds of complications. We will discuss what this means for particular cases below.

## Order types

Each order type comes with:
* a specification of the data structure of the order
* a specification of the smart contract used to fullfill the order (its address is tht of the `taker` field)

### `VIDEO_SUBMIT` submit a video

Request to add a video to a listing:

    {
      orderType: `VIDEO_SUBMIT`,
      data:  {
        hash: '0x...', // (ipfs) hash of the video submitted
        author: 0x.. , // address of the author
        hashData: 0x..., // IPFSHash with further data on the video
        listId: 0x..., // an identifier of the list where the video is to be submitted
      },
      makerTokenAmount: 1234 // the amount the maker is willing to stake on this video (can be 0)
      takerTokenAmount: null //  the taker must at least fill the bid for `stake - makerTokenAmount`
    }

Listings may be of different kinds.
We'll describe the case where the listing is a TCR.

The takers role here is to provide the (additional) capital needed for staking in the TCR, and to submit the TCR request.

The smart contract will fulfill this order by (a) creating an onchain record for the video in the `Videos.sol` contract and (b) submit the identifier to the TCR.

There are different scenarios where this can be useful:
- video makers can set `makerTokenAmount` to 0, i.e. submit videos completely for free (having "professionals" handle the staking part)
- video makers can submit videos with a stake in tokens; and stakeholders in the TCR can pay transaction fees (this is useful for platforms that want to reduce friction to pay the transaction fees for their users)

[there is a complication here in that the TCR will not allow a "split deposit", so the contract should handle that as well]

Note that all these actions can also be executed directly by the maker himself, bypassing the relayer and the relayer's contract.
For the `VIDEO_SUBMIT` orderType, the relayer serves as a way to provide new ways of paying for transaction costs and staking, a uniform interface in the Paratii framework, but does not provide an essential service.

There is a complementary `VIDEO_CHALLENGE` action that functions as a request to "crowdfund" a challenge, and a `VIDEO_EXIT` which is a request to remove the video from the listing entirely.


## `TRANSCODING_REQUEST`

Request that the video be transcoded.

    {
      orderType: `TRANSCODING_REQUEST`,
      data: {
        hash: '0x...', // ipfs hash of the file to be transcoded [may make this an url]
        ... information about max price, quality, timing, etc etc
      },
      makerTokenAmount: 1234 // (maximal) amount offered by the maker for transcoding this
    }

Relayers match the transcoding request with a transcoder or transcoding machinery. We can here think of interesting market interactions and price discovery, where relayers will match the order type with a transcoder that asks less than `makerTokenAmount`, and is allowed to keep the difference between actual cost and makerTokenAmount.

##  `PIN_REQUEST` plz pin this file. Will come with times, prices, etc.

This is very similar to the transcoding request, except that the `data` field will contain requested conditions for the pinning of the file (time, guarantees, etc)

## `AD_REQUEST` we have a view and would like to insert an Ad before

[This is changed a lot from felipe's medium article because it is @#$#$@ handwaiving on all the 43@#$$#@ details :heart: ]

This case is more complex than the previous ones, because it involves a two-step process. First the viewer requests an advertisement (which is the first order), then, after watching it, the viewer submits a "proof of view" with evidence that she has actually watched the ad.

Or, in more abstract terms: the maker her is offering her attention to the advertisement provided, and to settle the order must provide a "proof of attention" which can, of course, only provided at a later date.

So **step 1** is: match viewer and advertiser:

    {
      orderType: `AD_REQUEST`,
      data: {
        // anything that may convince the advertiser that the risk to provide this ad in return for takerTokenAmount is worth taking
        why : '....',  
        what: '...',  // a description of what kind of ad the maker is willing to watch, conditions on the reputation of the advertiser, stuff like that
        contactInfo: '..', // i.e. the ipfs or whisper enode address of the maker
      },
      takerTokenAmount: .. , // the amount the maker wants to be paid for watching the advertisement
      makerTokenAmount: 0,
    }

A relayer finds a taker - i.e. an advertiser, for this order.

**step 2: negotiate about conditions**

The taker then negotiates directly (using contactInfo) with the maker about the conditions of the view (i.e. what type of advertisement, conditions for "proof of view")

The negotiation process is concluded with in particular, in a URL or address of the advertisement itself.
Optinally, the advertiser and maker mutually sign a message with details about the agreement.

**step 3: settling the order**

After the maker has watched the video, the order is ready to be settled. For this, the maker creates a signed "proof of view" (which collects all evidence that she has actually seen the advertisement, but only as far as she is willing to share this info. The proof of view may also inclucde the agreement mentioned before), and communicates that to the taker.
If there is no disagreement, the taker simply signs the original order and submits it to the smart contract.

In case of disagreement, an abitrage procedure is started.s
This means that the viewer submits the original order together with the "proof of view" to an Arbiter.
[and we enter into a whole new rabbit hole, but that is "out of scope" here :-)]


## `LIKE`

In the case of a `LIKE` orderType, the viewer is offering to contribute to the curation of a list. This information may be worth something to aggregators or list creators, or other analists. Because the viewer is offering a piece of information, we cannot include the info itself into the order. Instead, the viewer will offer a *commitment* to a like or dislike, which she will reveal once the offer is accepted.

    {
      orderType: 'LIKE',
      data: {
        'proof': ... // "proof" of the like -i.e. sustaining evidence that the like is real
        commitment:   ..// hash of a yes/no answer + salt
        contactInfo: ... // contactinfo of the maker
      }
      makerTokenAmount: 0,
      takerTokenAmount: ., // the amount the maker would like to be paid for this "curation act"
    }

Here we can do the usual commit-reveal scheme with a twist: taker accepts the order and signs it. Maker accepts the taker-signed order, reveals the commit, and submits the whole shebang to a smart contract that will check the signaturs, etc.
