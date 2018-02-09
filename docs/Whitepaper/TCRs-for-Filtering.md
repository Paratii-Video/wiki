# TCRs for filtering
Paratii will employ *token curated registries* as a means of collective curation. The Paratii TCR is a smart contract with internal mechanisms for accepting or rejecting participants that keeps a secure record of videos taken as *reputable* by token holders. Its native token is the PTI, and it is used as a means of staking. The TCR lives under the assumption that its participants' interests are aligned as for including reputable videos and excluding non-reputable videos, which happens through a propose-challenge mechanism. 

The Foundation states initially two kinds of content it’ll pursue to curb, and thus asks for the help of token holders (further guidelines to be laid out in a separate document):

- Copyright infringing content.
- Offensive content (as defined by local applicable laws).

If content is excluded from the Paratii TCR, it can’t be included in other listings, as we’ll see below, and stops being "whitelisted" for operating with the system's contracts, thus being “wiped away” from users (although their original blocks may remain and hashes may "work" on IPFS indeterminately).

In general lines, token holders can help curate the list by 

- submitting videos to the registry, 
- challenging applications, or
- voting in face-offs. 

Anyone is able to read/access the listings even without having any tokens or relationship to the amounts in stake.

Users are free to submit a video to the registry, along with a deposit stake of PTI. Proposals can be challenged or not, by those willing to 1) keep the list quality high, 2) try to earn a share of the applicant stake, 3) both. Challenges can be made to any new applicant, and also to already listed videos, by any token holder willing to match a MIN_DEPOSIT stake of at least equal value to the amount in stake for that video, at any time. If an application is not challenged, the stake becomes effectively bonded in the candidate’s listing, and can be undone at any time. If challenged, the stake is matched by challenger(s), and a voting round starts, with one vote per token granted for all token holders. After a threshold is crossed, the outcome is determined. 

In the case of a rejected applicant or delisted video, 50% of the forfeited stake is distributed to the challenger and majority voters in the challenge (for example, in a 25% / 25% proportion), and the other half goes is divided among all stake holders, whether they voted or not. The minority voters neither gain nor lose, receiving back their stake, subtracted of a potentially small penalty. The applicant or listee under challenge is *slashed*, besides being removed from the list.

In terms of UX, we can "subvert" each of the steps in the propose-challenge mechanism into actions video consumers are more or less used to:

![TCR Flow](https://i.imgur.com/wppd9EM.png) 

- Uploading a video requires staking;
- Flagging means challenging;
- After a challenge (or enough weighted stake on challenges in order to activate a face-off voting round), the video starts showing a "V *vs* X" instead of a flag;
- Face-off happens through this "V *vs* X" and, when enough stake pends to one side;
- Outcome is determined, stakes are redistributed, and participants are notified.
- (The "reveal" part of PLCR voting can be integrated into an interface for "collecting rewards", if required).

Curators are incentivized to actively participate in the list, since every proposal for change may lead to a reward. Token holders want demand for their token to be high, in order to push its price upwards. Under the speculative game, though, you have incentives for them to be never be disinterested in the contents of the list they’re responsible for curating: to keep demand for the token high, they must maintain the *quality* of listings high, so consumers remain interested in its content, making video proponents desirous of being listed there. Token holders have a financial benefit for curating the list well, and this benefit increases proportionally to how well they curate and generate interest.

As [Mike Goldin, from AdChain](https://medium.com/@ilovebagels/token-curated-registries-1-0-61a232f8dac7), puts it, “token holders have a tactical incentive to challenge and reject every candidate to their registry in the interest of increasing their holdings, but this is at odds with their strategic interest of increasing the value of their holdings. An empty list is of no interest to consumers, so candidates would not bother applying to it. Candidates drive fundamental demand for a registry’s intrinsic token, and so by behaving tactically rather than strategically, token holders go against their own interests and incur a potentially severe financial loss.” [Another straightforward strategy]((https://medium.com/@DimitriDeJonghe/curated-governance-with-stake-machines-8ae290a709b4)) is to comply with the majority of the community by estimating or predicting the outcome of a challenge.

# Key Parameters

##### `MIN_DEPOSIT`
The amount, in PTI tokens, a candidate must lock as a deposit whenever submitting a video, and which must remain "bonded" for the duration of the listing thereafter.

##### `APPLY_STAGE_LEN`
The duration, in blocks or other defined period (e.g. epoch), during which an application can be challenged. On the original implementation, listing occurs only after the end of this period. The proposal here is to list applicants immediately, and extend the `APPLY_STAGE_LEN` indefinitely (perpetuating the "challenge window"). Alternatively, we can incorporate cost functions that increase the matching stake required with time once a certain period has passsed and no challenge was made.

##### `COMMIT_PERIOD_LEN`
The duration of the "face-off" phase, in blocks or other defined period (e.g. epoch). Period during which token holders can commit votes for a certain challenge.

##### `REVEAL_PERIOD_LEN`
The duration, in blocks or other defined period (e.g. epoch), during which token holders participating on a face-off can reveal committed votes for a challenge they're taking part of.

##### `CHALLENGER_PCT`
The percentage of the forfeited deposit, in case of a rejected application, after a challenge, which is awarded to the original challenger(s) that triggered the voting round, winning party in the voting round as a dispensation compensating for their capital risk.

##### `VOTERS_PCT`
The percentage of the forfeited deposit, in case of a rejected application, after a challenge, which is awarded to the winning party in the voting round. The amount "left" for token holders according to their stakes on the list is thus determined by 100% - (`CHALLENGER_PCT` - `VOTERS_PCT`).

##### `CHALLENGER_PENALTY_PCT`
The percentage of the matching stake, by a challenger, that's slashed in case the majority goes against him or the `VOTE_QUORUM` is not met. In case this penalty occurs, the amount sums up to the staked deposit associated to the listing, making it more costly to challenge, next time.

##### `VOTING_PENALTY_PCT`
The percentage of the tokens staked into face-off voting, by engaged token holders, that's slashed in case they end up in the minority side. Initially, this will be set out to zero.

##### `VOTE_QUORUM`
The percentage of tokens out of the total tokens commited and revealed in favor of _rejecting_ a challenged candidate that's set as mininum requirement for this candidate to lose listee status. The `VOTE_QUORUM` does not count non-voting tokens, and unrevealed tokens are considered non-voting. 

The reference implementation we borrow most of these parameters from is in https://github.com/skmgoldin/tcr.


# Treasured Playlists, or child-TCRs

Although the ability to organise content in playlists should be frictionless for any network user, the mechanisms behind TCRs can be extended by users willing to create skin-in-the-game playlists, or to try "outperforming
the curation output" of the main, generic list.

Treasured Playlists are a category of content listings subject to the same propose-challenge mechanisms of the parent-TCR, but with a difference: when one is created (and a corresponding TCR contract, deployed), it becomes able to mint its own native staking token. 

Users willing to start a Treasured Playlist (which can range from an organisation trying to secure a crowdsourced list of videos; a common user speculating on a specific list he's made; a promoter willing to bootstrap a group of creators) can initiate the following actions: (1) deploy a [new smart token and market maker contract](https://github.com/bancorprotocol/contracts); (2) stake an amount of PTI into it; (3) it spins off a child-TCR contract with the native token set to be the newly created one; (4) define the distribution of the new token; (5) sponsor applications into the playlist. 

The main parameters its creator defines are:

##### `LIQUIDITY_POOL_SIZE`
Contains the locked reserve currency (PTI), and, initially, consists by default of the child-TCR's creator `MIN_DEPOSIT` staked to kickstart the contract. It increases or decreases dynamically according to the demand for the child token. 

##### `OWNER_PCT`
The amount of tokens, in percentage of the `tokenSupply`, to be distributed, or pre-minted, to the list's creator.

##### `RESERVE_RATIO`
The relationship between token price, token supply and `LIQUIDITY_POOL_SIZE`.

`RESERVE_RATIO = LIQUIDITY_POOL_SIZE / (currentBuyPrice * tokenSupply)`

Different from the PTI, these token are made nontransferrable, but buyable or redeemable only against the market making contract that initially holds the “staked amount". The market maker establishes a price for buying the token and a “redeeming value” for which it can be “sold” back for PTI - both curves can be the same; they can have a fixed distance; or can have a distance that grows exponentially as more tokens are minted.

<img src="https://i.imgur.com/yq0miXB.png" width="400">

*[Simon de la Rouviere's representation](https://medium.com/@simondlr/tokens-2-0-curved-token-bonding-in-curation-markets-1764a2e0bee5) of a bonding curve with increasingly distant ceiling and floor prices, used by a market maker.*

At any moment, a token can be redeemed against the market maker for the reserve currency (PTI) according to the `currentRedeemPrice` curve, and the market maker will burn it, reducing the `totalSupply`.

Such price fluctuations present a computational challenge: setting the `currentBuyPrice` for selling 5 newly minted tokens isn't trivial once the price is supposed to change every time a new token is minted (or even fractions of tokens). The goal of computing the sum of all infinitely small changes in price can be achieved by calculating the area under the bonding curve in question (`currentBuyPrice` or `currentRedeemPrice`), using integrals.

For example, to find the PTI (`LIQUIDITY_POOL_SIZE`) held by a quadratic bonding curve contract, we calculate the integral of the function defining the current price:

`LIQUIDITY_POOL_SIZE = 1/3 * tokenSupply³`

Which represents the area under our curve between 0 and the current `tokenSupply`. 

<img src="https://i.imgur.com/xaIzpTb.jpg" width="800">

*[Slava Balasanov's calculations for setting prices through bonding curves](https://hackernoon.com/how-to-make-bonding-curves-for-continuous-token-models-3784653f8b17).*

Then we can derive the PRICE that should be charged for N new tokens:

`PRICE = 1/3 * (tokenSupply + N)³ — LIQUIDITY_POOL_SIZE`

Using this method allows for computing bonding curves based on any power function y=x^p. Taking a simple y=x² function, `LIQUIDITY_POOL_SIZE = 1/3 * tokenSupply³`, so the reserve ratio formula can be written as:

`RESERVE_RATIO = 1/3 * tokenSupply³ / (tokenSupply² * tokenSupply) = 1 / 3`

Thus we can look at our quadratic curve as if it defines a token with `RESERVE_RATIO` of 33.33% (1/3). With Bancor's formula, one can choose any arbitrary curve with reserve ratio between 0 and 100%. A reserve ratio of ½ yields y=x as a curve, and a reserve ratio of ⅔ means a y=sqrt(x) curve.

Child-TCRs can only be made up of content that’s necessarily on the parent list, although they are supposed to have their own focal points. It is a premise that these focal points "fit within" that of the parent list (e.g. a playlist with the best video lessons on how to play cool songs in the harmonica; which has only non-copyright-infringing content).

As [Simon de la Rouviere puts it](https://medium.com/@simondlr/continuous-token-curated-registries-the-infinity-of-lists-69024c9eb70d), "anyone can create a list, set criteria for inclusion, and play the crypto-economic game to include or exclude actors. The value of the registry will be determined by how useful it is and whether it acts as a good schelling (focal) point. This can also create a long tail of categorisation that wasn’t necessarily feasible or possible before".

Lists with high demand of people wanting to submit to it (because they’re well curated and have their audience) earn revenue for its token holders, since its token raises in value programatically as the number of token holders grow (and vice versa).

Challenges against playlists can also be made, according to the same rationale beforementioned. In the case, the stake to be matched would be that initially deposited to deploy the child-TCR token and playlist. Eventually, when multiple playlists with overlapping objectives coexist, there could even be a way to merge lists and combine stakes to increase the difficulty of challenging a child-TCR and to widen participation on it. 

### Front-running attacks
Bancor, and bonding curves in general, are famously known for susceptibility to front running: when an agent sees a buy order coming in and "frontruns" it with another order, with more gas, to get ahead of the original one. Once the original order is executed, it means the attacker already sold at a guaranteed profit the tokens just bought. A straightforward solution is to define a limit on gas price exchanges can consume, and encourage agents to always use the maximum allowed price when placing orders.
