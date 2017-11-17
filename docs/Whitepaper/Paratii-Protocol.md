# Paratii Protocol

## Layers

## DSN-Agnosticity

## IPFS

## Swarm

## Metadata Storage
### Purpose
Storing data on the blockchain is prohibitively expensive. There has been some existing research into storing data in a fully peer to peer manner. Parratii currently uses [IPFS](https://ipfs.io/) to store its videos. IPFS uses a Kademlia-like an algorithm to route requests for hashes of files to a node whose address is closest to the file's hash according to the XOR metric. However, IPFS uses content addressable storage and currently does not work well with mutable data.

#### IPFS Options and  Resources
IPFS could potentially store mutable data with the following systems (in development at the time of this writing):

* [InterPlanetary Naming System](https://github.com/ipfs/faq/issues/16)
* [InterPlanetary Record System](https://github.com/ipfs/specs/tree/master/iprs) 
* [Git style versioning in IPFS datastructures](https://github.com/ipfs/notes/issues/23)

Git style versioning has not received any recent signs of development (in public) despite it first being seen in the original IPFS paper. While there isn't a complete spec on IPRS, the general idea seems to be an overlay of the [IPLD](https://github.com/ipld/specs/tree/master/ipld), which is basically storing an extra layer of JSON metadata within IPFS hashes. This metadata can link to other content. IPNS has been developed most extensively but is the most limited in use. The idea amounts to replacing the file hash with a hash of its owner's public key. The owner is then responsible for forwarding queries to the current value of the hash.

##### IPNS
IPNS is well integrated into the IPFS framework but can be limiting because it requires individual nodes to be responsible for tracking the content of what would otherwise be distributed data. This effectively creates a point of centralization at the nodes responsible for tracking hashes of mutable content.

##### IPRS
IPRS proposes a record system with signatures based on the [IPLD](https://github.com/ipld/specs/tree/master/ipld) system. This appears to be a work in progress, with an incomplete spec and no known implementation in any codebase. However, since it is an overlay of the IPLD, it might not be out of the question to develop and use a simple implementation of the idea. It is hard to tell whether this is in active development or an abandoned appendage, but in 2015, there was work on an [example javascript implementation](https://github.com/libp2p/js-iprs-record).

The IPRS specs as they stand might be useful as a format for metadata on IPFS. 

##### Minimalist IPFS Updates 
Git integration first appeared in the original IPFS whitepaper. There has, however, been limited progress towards a function implementation. However, given IPLD and building on the ideas of IPRS, it might not be out of the question to implement the idea. 

As a minimalist implementation, we might store a file with the following record format:

```
data object {
  'data': <raw mutable data'>,
  'parent': <hash of parent record>,
  'validator': hash of validator,
  'signature': <multisiganture with public keys of IFPS node "owners">,
}
``` 

The `signature` field would involve a signature of all IPFS node public keys approved to make child commits. The keys should sign a hash of a record that includes all fields *except* the signature. The parent should be a hash of the entire parent data object (signature and all) that can be found on the IPFS filesystem.

The `validator` as described by the IPRS specs is a hash of a function that checks the validity of the data object. While this is useful in its own right, the idea can also be adapted to check for *updates* of the existing data object. This function might query each of the public key addresses in the multisignature for updated files of the hash of the current file. This could be done using the IPLD/IPNS system with links. Each node with a public key in the signature stores a link with an address given by a parent record that points to the hash of a child commit. The function could check whether any of these links exist on the public keys. If a child commit is found, either a conflict resolution rule must be established, or the system can use CFDT (conflict-free distributed replicated data-type) as seen in the Peerpad system described below. Peerpad also makes use of the IPFS [pubsub](https://ipfs.io/blog/25-pubsub/) functionality in which updates snapshots upon updates by peers.

Note that `validators` can also be Ethereum smart contract addresses.

#### Third Party Options
In addition to existing and emerging IPFS functionality, there are third-party databases that need to be explored in more depth:

* [Orbit-db](https://github.com/orbitdb/orbit-db) stores arbitrary key value pairs with IPFS.
* [Peerpad](https://github.com/ipfs-shipyard/peerpad/blob/master/docs/ARCHITECTURE.md) Peerpad is a peer to peer text editor that may provide some insight for decentralized storage and consensus of mutable metadata.

#### Orbit-db
Orbit-db stores arbitrary key-value data over IPFS, however, we need to further investigate the degree to which this database is distributed under the hood.

#### Peerpad
Peerpad uses [Y.js](https://github.com/y-js/yjs) to store text files as CRDT and performs a constant update of the document through peer to peer communication. A snapshot is stored in an updated database backend, which in the case of Peerpad is IPFS. This can be very useful for offchain updates of data. In fact, it might be a good idea to employ `web3.js` event hooks to initialize peer to peer communications in a similar manner. The final IPFS address of the new snapshot should, however, be periodically updated on the blockchain.

This process could conceivably be combined in the peer to peer communication when `web3.js` events trigger updates on IPFS. When the hash is updated, a smart contract is contacted to update the hash of the metadata on the blockchain. Within this system, Paratii could use any appropriate (in memory) database and simply store the hash of the database state on IPFS.

## Traffic data storage

## A Market for Sybils

**warning: speculative ideas ahead**

### The problem

Sybil attacks. Attackers have incentives to create "fake" users and views. For example:

 * Creators get rewarded on the basis of the quantity of views, they gain by creating fake views. 
 * The ideas we are bouncing around about bootstrapping and "lending": if we give anonymous users a "microloan" of PTI to spend on bootstrapping the download process. The attack would then create many false accounts, get the microloans, and "fake" the spending
 
This is not a problem that can easy be solved by a single algorithm. Fraud detection is difficult because attackers can adapt and will evolve. 

It is also a problem that is aggravated by the fact that we are making an open source project running on the blockchain: whatever our sybil-detecting mechanism is, it will be known to the attacker, and so more easily attackable. 

We have bounced around different ideas, but they all presupposed that Paratii should find an algorithm/method/system to detect click fraud that is as good as possible. (I say "as good as possible", because the absolute problem is basically unsolveable: it will be impossible to distinguish with 100% accuracy a "real" from a "fake" user. Instead, the base line is a good percentage - say between 60-80% of the counted views are "real")

Here is another idea:

### A market for views

What we do is put _all_ views that are reported by our players on a market. This will be a large and various "pool" of users - from anonymous probably-bot-nets to registered users that have an email and a profile pic. 

For these last type of users, we (could imagine) ask them for more personal info. (This can be on an opt-in basis, we can pay them for giving these data, etc)). So for a small (?) part of users we do have ome demographic info. (Also the profile pic basically already tells us sex and age

The core Paratii system is not concerned about fake users. Instead, the problem of detecting fake users is pushed further down the line, to a market in which "bundlers", who can, **depending on the use case and the purposes** try to filter out bad cases or not. 


 * For example, in the ad market there are bundles of views for sale. This market is open, in the sense that anyone can create bundles and try to sell them. These bundles may be of different types, both in quality as well as specificity: 1000 "registered female users in age group 30-39" will have a differet price then "1000 registered or anonymous users that have IP addresses in a range of XY". It does not matter much for this market if the sybil rate is very high (say 99%), it just means that a bundle of 1000 arbitrary views will be very cheap (because you are basically buying sybils). What matters instead is that the system provides enough information to create high-quality bundles.

 * Similar for the "bootstrapping microloans". A lender here has a a market on which anonymous new users are on offer - but the lender will have some information about this new anonymous user: perhaps a referral link, perhaps an IP address, the video he is watching, the timing of the view... the lender can do a risk analysis on the basis of this profile, and decide to grant or deny the loan, and  if granted, decide on an interest rate corresponding to the estimated risk.

  * (There is one specific case in which our model does presuppose that Paratii itself has a fraud detection mechanism: the "proof of quality" mechanism that decides how much creators get paid from the redistribution pool.)

Basically, it will not be Paratii who will need to find the holy grail of fraud detection. Paratii "Core" will just register any view from any source that behaves like an instance of the paratii playe and follows protocol. Instead, there will different fraud detection mechanisms competing on a transparent open market, in which agents choose/buy the mechanisms that are best for their particular use case.

## Decentralised Ad Exchanges

## Reputation System
### Introduction
We want to be able to reach a decentralized consensus on content quality and the association of tags with it.

For the purposes of Paratii, reputation can be thought of as how representative a user's decisions are of other users of a given system. This can naturally justify why one user's vote should be weighed more than another's. If one user can be thought of as twice as representative of other users, their vote should carry twice as much weight. 

Reputation can be acquired by consistently acting in way that best represents the rest of the community. 

## Background

All content posted in Paratii has the following fields:

 - **Tags**: A series of UTF fields associated with each video entry. Users can choose existing ones or create their own.
 - **Up/Down vote**: An up vote denotes positive feedback from the viewer, while a downvote denotes negative feedback from the viewer.

A _naive_ system would allow users to vote arbitrarily and weigh them all equally. This system remains vulnerable to a sybil attack in which a user creates a swarm of fake identities to vote together. Reputation provides a means of assessing the odds a given vote is representative of the general community, and can be integrated into a reward system.

A vote should require some risk on the part of the voter and potential for reward. If they vote in a manner aligned with the consensus, they get rewarded with reputation. If they don't, they are penalized. The mathematical nature in which users are rewarded or penalized should be determined with care and ideally be representative of some overarching philosophy.

## Constraints Given Attack Vectors

The reputation change of the current and previous voters should be completely determined by the current and previous voter reputations at the time of voting. Optionally, one could incorporate their current reputations as well.

In this section we outline a series of increasingly complex attacks on a decentralized reputation system and describe how to mitigate each of them with correspondingly sophisticated reputation schemes. 

The adversary's objective is to reap the benefits of producing highly valued content without actually producing highly valued content. The most vulnerable and commonly exploited aspect of decentralized networks is the sybil attack, in which the adversary creates and directs a swarm of fake identities to behave like an approving crowd.

To illustrate the most trivial attack on Paratii's video sharing service, let's briefly discuss the notion of someone voting for their own video. In this instance, someone uploads a video, upvotes it, and proceeds to profit off of their increase in reputation. This is obviously disallowed and has little real world effect, but this is the effect we are attempting to thwart in its simplest form. Identities come and go on a decentralized network, and this makes it difficult to monitor interactions, create a consistent notion of reputation, and create incentives that reward people of altruism. 

### First Order Sybil Attack

In the most basic sybil attack, an adversary would create an identity that uploads a video and a swarm of identities that upvote that same video. The content producing identity is rewarded for producing good content while the voting identities gain reputation for voting together. 

The first order sybil attack can be immediately generalized into a network of identities that upload content and upvote each others content.

#### Proposed Nonsolution: Non Free Identities

If Paratii wanted to act as an oracle for bad content, this would be an obvious way to collect necessary funds to justify the time it will spend reviewing content. 

Aside from making Paratii more like YouTube and similar video sharing ancestors, the user could always find some some number of identities to create for which they would gain more profit uploading and upvoting videos than it costs to create more identities. The reason is if we call <img src="http://mathurl.com/22kf4dt.png"> the profit per upvote per video, and <img src="http://mathurl.com/32qch9w.png"> the cost of creating a new identity, the cost of creating <img src="http://mathurl.com/32h6522.png"> identities is <img src="http://mathurl.com/ycfcpuvz.png"> and the profit of having them all upload a video and upvote that video is <img src="http://mathurl.com/ybqxg5d8.png"> (we assume there is already a rule against an identity voting for its own video, which is why it isn't <img src="http://mathurl.com/y9b6pq6s.png">). We can always find an N such that <img src="http://mathurl.com/ya3p9pep.png">, namely <img src="http://mathurl.com/ybmxaxt7.png">.

#### Proposed Solution: Sacrifice Reputation to Vote

As proposed in the Backfeed protocol, sacrificing a nonzero reputation <img src="http://mathurl.com/y9dovst9.png"> to vote would suffice to prevent an attacker from directing a swarm of fake identities voting together since the fake identities would have to acquire reputation in a nontrivial way. This then raises the question of how users acquire reputation. 

We outline two approaches to this.

##### Nonzero Initial Reputation and Reputation from Uploading

Users could, for example, be initially given "the benefit of the doubt" and awarded enough reputation for a small number of votes. In this case, we must be careful that they earn less reputation voting together than they lose casting their initial votes.

If a voter's reputation changes by <img src="http://mathurl.com/y9zb5baf.png"> per vote in perfect accordance with all other votes, gains <img src="http://mathurl.com/6bz4xn2.png"> reputation per upload, gains <img src="http://mathurl.com/lzhndgq.png"> per upvote on their upload, we want to ensure that 

<img src="http://mathurl.com/y7g7h5o3.png">

so the profit gained from the voting fake identities is less than the cost of using them to vote on an upload.

The user can bootstrap reputation by uploading valued content. The first voters are then going to be early adopters who set forth a precedent for what makes for good content by voting it up, and thereby awarding the uploaders reputation that allows them to vote for other videos.

#### _Proposed Solution: Voters Cannot Surpass Uploader Reputation_

This technique can be used in conjunction with any other approaches. In essence, it prevents malicious users from bootstrapping reputation by creating fake identities that only interact with each other by requiring the uploader have more reputation than the voter for the voter to earn reputation by voting on the video. Users may still lose reputation by voting on a video.

Alternatively, one can take any reward function and rescale it to cap at the reputation of the uploader. One could, for example, send the initial calculation of reputation earned through a sigmoid function and multiply by the reputation of the uploader. 

## Haze Attacks

In this situation an attacker does not care about their own reputation, and only seek to target a single user out of malice. They repeatedly downvote or flag the user's videos to decrease their reputation. They may accomplish this with the help of a swarm of fake accounts.

Flagging and jurisdiction will be covered in a separate page.

### Proposed Solution

Users must be limited in how much reputation they can lose from having their videos downvoted. This should happen in the same manner. This can be a percentage of their total reputation should none of their videos have been downvoted, and can asymptotically approach this percentage as more and more videos are downvoted. 

The question is then at what rate it should approach this percentage. If a user has a large following, one would expect a large number of videos downvoted, so the number itself cannot be the correct metric. Indeed, if we look at the percentage of all votes that were downvotes for this user, we can easily use this as a sliding scale that rescales their reputation by a factor. If a user has R reputation, <img src="http://mathurl.com/ycnljlno.png"> staked in downvotes of their videos, and <img src="http://mathurl.com/yc964emc"> staked in upvotes, then their reputation will be decreased by the following amount due to downvotes.

<img src="http://mathurl.com/ydall5ds.png">

for downvotes. <img src="http://mathurl.com/yemlmqa.png"> is an arbitrary number between 0 and 1 that serves as a limit to the percentage of their reputation that can be lost due to penalization for downvotes on their videos. 
## Possible Cost and Reward Functions
In this section we outline possible cost and reward functions for voting. The Backfeed protocol is an obvious choice. Comments from @Jelle are much appreciated. The **_cost function_** is the change in reputation incurred from the voter and all previous voters by the act of voting.

### Backfeed

In the Backfeed protocol, we use a cost function, d given by the following formulas.

For the most recent voter of k votes, the new voter loses some reputation, <img src="http://mathurl.com/yclwybyy.png"> proportional to their current reputation:

<img src="http://mathurl.com/yczelowm.png">

Where <img src="http://mathurl.com/y9nt2jys.png"> is the reputation of the most recent voter, <img src="http://mathurl.com/y72calut.png"> is an arbitrary rescaling, <img src="http://mathurl.com/y7jncv3g.png"> is the sum of the reputation of all users who voted on this tag, <img src="http://mathurl.com/ycknqtp8.png"> is the sum of all the reputation of all the users who voted in the same way as the most recent vote, <img src="http://mathurl.com/yajnqbx.png"> is the sum of all reputation of all voters in the system, and <img src="http://mathurl.com/827tag.png"> is the skewness of the payout distribution for each value of reputation. We can always choose a distribution so <img src="http://mathurl.com/827tag.png"> is 1 or 0, which makes for easier analysis. 

We consider modifying this for <img src="http://mathurl.com/ycv6w55k.png"> to lessen the penalty for late voters.

<img src="http://mathurl.com/yc279car.png">

For previous voters:

<img src="http://mathurl.com/y6u3j52e.png"> if they voted differently from the most recent voter, and otherwise <img src="http://mathurl.com/y92gyqk5.png">, where <img src="http://mathurl.com/y7yzbo3f.png"> is the <img src="http://mathurl.com/y9hkd5uh.png"> user's reputation, and <img src="http://mathurl.com/ycknqtp8.png"> is the reputation staked in the most recent voter's vote (including their vote).

This can be summarized as "the new voter loses a proportion of their reputation, possibly less so if they vote like other people, and all previous voters who voted the same way gain reputation proportional to the contribution of their reputation to the total reputation staked on that vote.

#### Uploader Reputation Ceiling

To prevent a swarm of fake identities from bootstrapping their reputation from scratch, we can either not grant them any increase in reputation if their reputation is already greater than that of the uploader, or continuously rescale their increase in reputation such that it infinitesimally approaches that of the uploader. In either case, we would map the cost and reward functions through a nonlinear function. 

In the case of a hard cutoff, we can leave the cost function unaltered when it will not boost the voter's reputation higher than the uploaders, and otherwise set the voter's reputation equal to the uploader's reputation.

#### Rescaling Reputation Boost From Voting

As long as we're cutting off the reputation of the voter to never surpass that of the uploader, we can rescale the reputation gain from voting on each video so if the voter gains no reputation from any other sources, their reputation will asymptotically approach that of the uploader. The reputation gained can be rescaled by a factor equal the difference between the voter's reputation and the uploader's reputation, divided by uploader's reputation. This fraction will between 0 and 1 and always readjust any cost function so a voter's reputation that starts below an uploader's reputation can never increase higher that uploader's reputation because of their vote.

### _Page Rank and Smart Contract Dispersal_

In this section we propose a different approach in which reputation is represented by tokens transferred between users upon upvoting.

Google's Page Rank was originally designed as a form of decentralized measure of reputation for peer review publications. It became the no famous search engine by applying the same technique to rank web pages based on how often the reference other webpages. This can be adapted to an arbitrary decentralized reputation system. 

Say we have a contract that all users sign that distributes tokens that represent units of reputation to all users. These can, perhaps, be PTI tokens. At set time intervals, the PTI redistribute themselves from each user to the users who uploaded the videos they've liked. If the user has liked more videos than uploaded videos others have liked, they lose their reputation at this cycle. But the moment the upload a video that others like, reputation flows into their account. 

Since reputation is conserved in this system, no set of fake accounts can bootstrap reputation by voting for each other. Any reputation they cumulatively had is redistributed amongst their fake identities but never created nor destroyed. 

~~Reputation could be safely awarded at the creation of an account in an arbitrary manner too because they can't gain reputation by voting. Reputation can be awarded too for uploading. As long as interactions conserve reputation, this system works.~~ We cannot just award reputation for creating an account since this leads to a sybil attack in which someone creates a series of fake accounts to acquire reputation distributed over many fake accounts. They could then even combine the reputation into one large pool. Users would have to upload a video to gain some reputation for voting from the votes of other users.

Reputation tokens can be embedded as smart contracts that are written to automatically transfer wallets at a predetermined time. It might be wise to have a dual token awarded in return so users gain something from upvoting videos. These tokens will flow in the direction opposite to reputation tokens. That is, they will flow from addresses of uploaders who have been upvoted into the addresses of user who upvoted their videos.

This is a stable system and should be considered further since it is naturally decentralized and should lend itself to a smart contract code. 

### Large Reputation Holders

Ideally, reputation should be earned fairly due to a sound self consistent system. However, we consider a taxation system in which users with a large proportion of the network's reputation, might be required to make a mandatory tax donation back to the network. Care should be taken to avoid gaming this system. The user should not know when they would be taxed, so this should probably happen in a probabilistic manner but with a deterministic formula--so everyone knows what they signed up for, but they'll have an increasingly high probability of being taxed as they grow more powerful. This prevents powerful users from riding close to a taxation threshold but never passing it. 

## Credit System
### Introduction
The average use case intended for the Paratii player is to have it embedded in webpages and applications, which brings up the matter of anonymous users - those who haven't and might not ever want to register as a new user in Paratii. Here we outline possible means to allow accounts (common users or publishers) to accrue debt, and others to gauge their creditworthiness. The objective is to let users "aid" in serving content to their anonymous counterparts, which may sometimes "drain" their account by not signing up.

### Purpose
A typical newcomer will want to watch a video before they sign up or make some kind of account. However, the content provider sharing the video stream will need to exchange tokens for a video stream. We may want to allow users to accrue token debt so they can serve anonymous users to encourage new users to sign up.

### Credit Worthiness
Ultimately, we need users to have enough invested in their accounts that they probably wouldn't want to "defect" and lose their accounts. Thus we could have some notion of credit in which users are allowed to borrow tokens from each other or the PTI redistribution pool and have some metrics that could be used to determine their creditworthiness.

We first establish possible metrics for creditworthiness. These factors include:

- Age of account (T)
- Total PTI stored in account (X)
- Total current debt (D)
- Reputation (R)
- Total amount of loaned money returned (H)

We would almost certainly want to keep a running history of all of these metrics throughout the existence of the user's account so a potential lender can inspect statistics pertaining to the user's account. A potential lender will first attempt to mitigate risk, then try to maximize returns on investment (ROI).

Note that with these quantities, we can derive rates by dividing by T. There are some other potentially useful metrics:

- Net worth: W = X - D
- Projected net worth after time T': W' = W(1 + T' / T)

We can then use a set a means of allowing debt to the PTI redistribution pool or allow users to make their own conclusions and introduce peer to peer lending. These are of course not mutually exclusive options, and would likely complement each other well.

### Sample Lending Strategy for the Redistribution Pool
Here we illustrate a sample lending strategy that could be used by the PTI distribution pool. Since the redistribution pool isn't primarily trying to make a profit, it could serve as a starter lending agent for newcomers. We just want to help new users participate in the system without devaluing it by defaulting on small loans.

Since the redistribution pool will likely be for new users without an established credit history, we can loan according to account age. We might quantify our objective to maximize the total amount paid back and charge an interest rate based on the expected probability of defaulting such that the expected return is zero.

For a simple model, we want to make a loan to an account of age T with no history and a net worth of W PTI stored in the account. To help simplify calculations, we can first operate under the assumption that doubling the amount of PTI in an account is just as likely to mitigate risk as doubling the age of the account. In this case, we can combine T and W with the following quantity:

![Predictive quantity, P](http://latex2png.com/output//latex_ede79174bf7100330bc63777dd02030c.png)

Here we define the predictive parameter, P, as the sum of each net worth W, multiplied by the time the user had that net worth before it changed due to a new transaction. For example, if a user had a net worth of 7 PTI for 4 minutes, then earned a PTI token and had a net worth of 8 PTI for 5 minutes, you'd have P = 7x4 + 8x5 = 68 PTI minutes. The unit of time isn't important for this metric as long as it's used consistently. In the above equation, we assume N transactions took place. This product effectively provides our measure of investment the user has made in the Paratii system in terms of time and money.

We can make another simplifying assumption and say the probability of defecting, Q, will decrease exponentially with the predictor P.

![Rate of default](http://latex2png.com/output//latex_e0b3a76f8dbf4fb45153d74176b2c53d.png)

It is important to note that in practice, one may have more than one predictive parameters, but we are trying to work with a simple model to start.

Now, as we make loans, we'd update the values of *a* and *b* to fit the curve and determine how likely users actually are to default on their loans. This can be done using a [standard least squares approach](http://mathworld.wolfram.com/LeastSquaresFittingExponential.html)

Let's say we want our expected returns to be zero on average (since the Paratii avatar is supposed to just supply liquidity). Given a probability of defaulting, Q, and interest rate I, we'd obtain the following expected return.

![Return on investment](http://latex2png.com/output//latex_312836d596015211bdb6b00b66c75df3.png)

Solving for I, we obtain

![Interest Rate](http://latex2png.com/output//latex_c744d354ea6de0554d7226aaa0c46354.png)

So we now have a formula for the interest rate we can charge given a calculated probability of defaulting Q. Q is in turn calculated according to our exponential distribution with parameters *a* and *b* which are continuously updated to match the data collected as users actually pay off their loans or default.

The final question is how to mitigate risk. As the system stands, a new user could ask for all the Paratii tokens in the redistribution pool, get them with an enormous interest rate, and just spend them on something and close the account.

We can start by limiting the number of tokens taken out in the loan to the user's current net worth divided by the interest rate, W/I. Within this philosophy, the worst case for the debtor is they lose their net worth (or are forcing them to spend it), and the worst that can happen to the lender is lose the same amount in a loan. We could similarly offer the loan a period of time up to the age, T, of the account. The user will always have at least as much time invested the account as the lender spent waiting on a return.

### Sample Lending Strategy: Peer to Peer Maximize Returns
We can take a similar approach to the Paratii Avatar lending system to maximize profits for a lender with a peer to peer lending system. Instead of setting the expected return on investment equal to zero, we set its derivative equal to zero.

![Maximize Returns](http://latex2png.com/output//latex_955aab730b93e86048fb052cfc8e3782.png)

Then we solve for the interest rate as a function of the odds of defecting.

![Interest Rate](http://latex2png.com/output//latex_8c96bb550df624bc1fa53b2d4f6862fa.png)

The constant C is arbitrary can be adjusted to maximize the number of users that take out loans in the first place.

#### Sybil Resistance
A nice property of the predictor P is that it is very Sybil resistant. If a user creates a swarm of fake identities, there is no way for them to increase P by taking out loans to each other and paying off those loans since debt history is not used.

##### Fake identities that interact with each other
An easy way for fake identities to defraud a reputation system is for them to interact with each other in such a way that they mimic a history of being rewarded for good behavior by other users. In this section, we prove this impossible given the proposed credit system. 

There is similarly no way for them redistribute their wealth in such a way that P for one identity is greater than the sum of the P values for all the fake identities before the transaction.

Proof:

Assume two identities have predictors P1 and P2 at a certain point in time. We consider two scenarios. In Scenario A, the identities do not interact with each other. In Scenario B, identity 1 gives identity 2 N tokens. 

In Scenario A, the two identities' respective predictors at time t later will be P1+tX1, and P2+tX2, where X1 and X2 are the current account balances of the two identities. In Scenario B, we have P1+t(X1-N) and P2+t(X2+N). Note that the sum P1 + P2 remains the same in both scenarios. Since there is no way to merge resources in such a way that the P value of one identity comes out to be greater than the sum of the P value of all available fake identities, the user will have gained (and lost) nothing from dividing their time and resources into managing multiple identities.

##### Fake identities ask for credit first
Another attack on a credit system might include a swarm of fake users who ask for credit and combine their loaned money for more money than the user could have gotten with only one identity. Since time is not transferable, the user cannot gain more time with the loan by using fake identities. Similarly, if the user splits his or her time earning tokens for several accounts rather than focusing on one, they could either combine the tokens earned for the fake accounts to earn a higher credit limit or get the highest loan possible on each account. Either way, a user that spent 1/4 their time managing four accounts to gain 1/4 the tokens they could have earned with one can either use each fake account to receive for 1/4 the loaned money they could have received for one account or pool the tokens and receive at best exactly what they would have gotten had they stuck with on account from the start.

It is worth noting that if there is a way to earn tokens passively, this system can be defrauded since the user would be better off creating several accounts to earn tokens passively in parallel than using just one account. However, we should generally assume that part of what gives tokens their value is that they require some effort to obtain. The point here is that it does not matter how they are distributed. 

### Less Easy Attacks and Possible Mitigations
Possible types of attack:

Sybil thief Attacker creates lots of new accounts, takes out the loans/gifts, transfers the money to his own account, and walks away without every repyaing the debts. Paratii will ahve lost the money, the attacker will have earned it.

Sybil vandalism. Attacker creates lots of new accounts, takes out loans/gifts. Goes away a without every repaying. Paratii will have lost the money (that is locked in the new accounts)

Here are some ideas:

Instead of lending new users the ETH to transact, we may pay for their transaction costs instead. This will obviously remove the incentive for the sybil attack, but is still subject to vandalism. It will also be difficult to implement without relying on some centralized mechanism (and will radically change how our architecture works). Note that state channels have no transaction costs (except for initializing and settlement) and may provide the necessary flexibility here.

the PTI/filecoin needed to bootstrap the p2p node. So, to download from the network, the ndoe needs some filecoin. It can get that from seeding, but obviously the new node does not have content yet. Instead of ledning new users PTI, we can give them the bits, i.e. the content. I.e. we have "sponsored nodes" or "sponsored content" that is available without payment (to new nodes), new nodes download this content from the sponsored nodes and then can start exchanign bits for bits within the whole new network.

Do lend some money to new users, but this money will be very much restricted. For example, let's say that every new users gets 1 PTI in a special CreditWallet contract. This contract limits what the user can do with the PTI - for example, it can only spend that PTI to certain whitelisted contracts. Moreover, the CreditWallet will remain under control of the creditor (i.e. typically Paratii), so if the debtor disappears, the creditor can recuperate any money that has not been spent yet.

## Publisher Supernodes

## Pluggable Components

## Flagging and Moderation
When an account reaches a certain threshold, invitations are triggered to random contributors who’ve demonstrated interest in bounty hunting inside the ecosystem and hold high reputation. Bounties are set proportionally to that account’s total PTI balance. Invitations can be complemented with information that might speed up the process, communicated privately (precise geolocation and scanned documents are two examples); judgements are binary; and a multiple-round consensus seeking mechanism disincentivizes validators to collude, and yields rewards under distinct scenarios. Accounts with higher balances provide higher bounties, and therefore tend to be validated faster. Creators have control over how much sensitive data they want to risk versus how fast and easily they want their accounts to be verified.

### Purpose
This serves as a system designed to determine when, how, and under what circumstances a video can be flagged. We also outline the jurisdiction process and rewards and penalties associated with good and bad voting.

### Background
Users who can upload videos to the Paratii system may do so for malicious reasons. Examples of bad content include:

- Copyright infringed content
- Illegal content
- Hateful content

*Illegal content may have to be modified to include only content illegal in certain countries since what is illegal in one country is not always illegal in another. 

In a basic flagging system, users are allowed to flag a video for violation of terms of an agreement assumed when posting the video. Normally, any identity on the system can vote in this manner. When some threshold of flags has accumulated for the video, the video is sent to a set of moderators who decide whether the video warrants removal. 

Moderation is completed in a similar manner to flagging. After N moderators have voted the same way, the decision is made and no further moderators are offered a vote. 

However, as with reputation, users can employ sybil attacks to exploit this system for malicious intent. We will use potential attacks to outline constraints on the system.  

### Sybil Haze Attack
Users can team up to haze a single user by always flagging their videos. This generally is a known problem on with popular video sharing platforms like YouTube. We can mitigate the possibility that a group of users is a rogue swarm of sybil identities in two ways.

#### Separate Flaggers from Evaluators
We can limit possible motivation for abuse by requiring a separation between flaggers and evaluators. We can start with a requirement that anyone who flags a video is ineligible for moderating it. 

This will eliminate some obvious conflict of interests, but it will not account for the possibility that fake accounts collaborate into two groups: Those that flag and those that moderate. This boils abuse protection down to two problems: controlling who flags and who moderates. 

We can further restrict the moderator group to users that haven't viewed any of the same videos that the flaggers have viewed. This can help mitigate the possibility of social ties between flaggers and moderators. In the even that there are no users that have not viewed any of the same videos that the flaggers have viewed, moderators can be randomly drawn according to preference to those that have viewed the least number of videos in common with any of the flaggers. Alternatively, moderators could be drawn probabilistically with preference to users who have viewed the least number of videos in common with the union of all the videos seen by all the users that flagged the video in question. 

#### Random Moderator Selection by Reputation
Moderators should be randomly selected to evaluate each flagged video. Moderators with higher reputation should be selected more often than those with a lower reputation. It is natural then to devise a system in which moderators are selected with a probability given by the proportion of the total reputation in the Paratii system that the moderator holds.

We can also impose an artificial lower cutoff on the reputation for someone to be selected as a moderator. This will prevent large numbers of fake identities with low reputation from potentially dominating the moderator pool (different ones would always be selected, but if there's enough of them, they could cumulatively have a reputation on the order of the best "good" users). 

Moderators could be given the option of voting or choosing to defer their vote. If they vote the same way as the other participants, they can be rewarded with reputation. If they don't, they will lose some reputation. If they defer, they should also lose some reputation, but less so than if they voted out of accordance with the other moderators. 

If moderators with more reputation are proportionately asked to moderate more and lose some reputation if they don't participate, this acts in some sense as a natural "tax" system in which users with disproportionately high reputation necessarily need to continue to exercise their reputation for good or lose it until they are endowed with a suitable level of responsibility.

#### Mathematical Analysis of Moderator Incentives
Moderators should be given a reward X for voting with the majority, and a penalty Y for voting against the majority. They can further be hit with a penalty Z for deferring their vote. 

Let's assume the next moderator scheduled to vote assigns a probability P that someone would vote to have the video deleted, and the system requires N votes of the same nature to end the voting sequence.

The moderator computes a probability <img src="http://quicklatex.com/cache3/5b/ql_3d7568f49d7d2fb8cd1d89cee853925b_l3.png"> that the video will be deleted given they vote to delete the video, <img src="http://quicklatex.com/cache3/99/ql_a38bd6a42c0f554889af7a670827fe99_l3.png"> that the video will be deleted given the moderator voted to save the video, <img src="http://quicklatex.com/cache3/4d/ql_80a905a9e51bf3f710daaba2ac079d4d_l3.png"> that the video will be saved given the moderator voted to delete the video, <img src="http://quicklatex.com/cache3/7e/ql_7df0de9bdf776511239c9d44ad03c97e_l3.png"> that the video will be saved given the moderator voted to save the video, <img src="http://quicklatex.com/cache3/20/ql_0155d4cceedc6c5e5cc3d527b6bdaa20_l3.png"> that the video will be saved if they defer, and <img src="http://quicklatex.com/cache3/82/ql_864894881addcf8ddcf036a92c019282_l3.png"> that the video will be deleted if they defer. 

The moderator is interested in the probability that they will vote with the majority. Assuming the probability that someone will vote to delete the video is <img src="http://quicklatex.com/cache3/b7/ql_5d7e25f271bb62b9ae1446313e9b5bb7_l3.png">, the moderator would choose between voting to delete the video and passing. The expected outcome of voting to delete the video is:

\
<img src="http://quicklatex.com/cache3/42/ql_12d749b58fc362ba905c464e7aacd542_l3.png">

For the moderator to defer, <img src="http://quicklatex.com/cache3/fd/ql_345bc1468a95b192515fe1c9da6d16fd_l3.png">.

\
Note that <img src="http://quicklatex.com/cache3/7e/ql_71198987b7c412da69e2b3a79baf8e7e_l3.png">.

\
<img src="http://quicklatex.com/cache3/1f/ql_b7cfeb56618bb739af57cdb728e65c1f_l3.png">

\
Solving for <img src="http://quicklatex.com/cache3/5b/ql_3d7568f49d7d2fb8cd1d89cee853925b_l3.png"> would suffice.


#### Stake Reputation for Bad Flagging
Now that we have a system for assuring no conflict of interest between moderators and voters, we need to punish accounts that flag unnecessarily (effectively DDoSing the system). We can do this by first having a stake of reputation in their vote. This stake should be some proportion of their own reputation to be sure extremely reputable voters are affected. The potential gain can also be proportional to their reputation, but this is not strictly necessary. In this sense, someone can gain or lose reputation *exponentially* by flagging videos. This is an appropriate scale of loss and reward since someone should not go flagging videos often, but it's a strong statement when they do.

##### Conserved Reputation
If Paratii uses a "conserved reputation" system, then the flaggers' staked reputation can be transferred to the uploader if the moderators decide to keep the video, and transferred from the uploader to the flaggers if the moderators decide to remove the video. 

If the moderator defers voting, their reputation penalty can be evenly distributed amongst all the moderators that do vote (a new moderator should be selected in place of the old one to maintain a constant number of moderators for each judgment). In this system, moderators have a nonzero probability of receiving a reward from participating in the system should they be taking a task someone else didn't want. If numerous moderators refuse to partake in voting, then the sum of their reputation penalties will be redistributed amongst the moderators that finally do decide to vote.

#### Problems with Sample Biases
There could potentially be constraints on the potential for a moderator to be selected. For instance, one could potentially prevent bias by only selecting users who have never seen the video in question to moderate it. This could, however, produce new a bias in the sample pool if the flagged videos become high profile. It is likely best to keep a random sample of potential users that are certainly *not* bots.

#### Minimum Reputation, Badges, and Further Development
Much like in stack overflow's system, we can assign users "badges" based on their reputation awarded during the course of their participation in the Paratii system. For starting, a single that represents an eligibility for flagging and "moderator duty" should be sufficient for sifting out bots and requiring users who carry weight incur a larger proportion of responsibility for keeping the system maintainable. 

### Multiple Levels of Arbitration
Like Aragon network, Paratii could implement multiple levels of arbitration into their flagging system. A video marked for deletion after moderation could be placed in a limbo state in which the uploader has 24 hours to file an appeal using some additional reputation (or PTI) as stake. It does not matter how much is put at stake, just that they are wagering more than before and there is a finite number of times they can appeal the decision. These conditions respectively eliminate attacks in which a user appeals and hopes for a statistical fluke because they have nothing to loose and eliminates an attack in which they appeal indefinitely with no consequences.

### Multiple Reasons for Flagging
Users could have the option of flagging videos for more than one reason. This could include:

* Pirated content
* Misleading description
* Illegal content
* Hateful content

When a certain number of flags accumulates for a particular reason, moderators would be drawn and asked to confirm the video violates that specific term.

#### Important Aspects and Summary

##### Essential
* Flaggers should stake a proportion of their reputation in their flag.
* Moderators should be selected with a probability given by the proportion of the total reputation they own amongst the pool of all users.
* Moderators should have a minimum threshold of reputation to ensure they are not bots.
* Flaggers should have a minimum threshold of reputation to ensure they are not bots.


##### Optional
* Moderators are penalized from not participating when selected
* Staked reputation of flaggers goes to uploader if the moderators vote against removing the video.
* Multiple levels of arbitration

## Bounty Hunting
