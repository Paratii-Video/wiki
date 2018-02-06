# TCRs for filtering
Paratii will employ *token curated registries* as a means of collective curation. The Paratii TCR is a smart contract with internal mechanisms for accepting or rejecting participants that keeps a secure record of videos taken as *reputable* by token holders. Its native token is the PTI, and it is used as a means of staking. The TCR lives under the assumption that its participants' interests are aligned as for including reputable videos and excluding non-reputable videos, which happens through a propose-challenge mechanism. 

The Foundation states initially two kinds of content it’ll pursue to curb, and thus asks for the help of token holders (further guidelines to be laid out in a separate document):

- Copyright infringing content.
- Offensive content (as defined by local applicable laws).

If content is excluded from the Paratii TCR, it can’t be included in other listings, as we’ll see below, and thus are “wiped away” from users (although their original blocks may remain and hashes may "work" on IPFS indeterminately).

In general lines, token holders can help curate the list by 1) submitting videos to the registry, 2) challenging applications, 3) voting in face-offs. Anyone is able to read/access the listings even without having any tokens or relationship to the amounts in stake.

Users are free to submit a video to the registry, along with a deposit stake of PTI. Proposals can be challenged or not, by those willing to 1) keep the list quality high, 2) try to earn a share of the applicant stake, 3) both. Challenges can be made to any new applicant, and also to already listed videos, by any token holder willing to match a MIN_DEPOSIT stake of at least equal value to the amount in stake for that video, at any time. If an application is not challenged, the stake becomes effectively bonded in the candidate’s listing, and can be undone at any time. If challenged, the stake is matched by challenger(s), and a voting round starts, with one vote per token granted for all token holders. After a threshold is crossed, the outcome is determined. 

In the case of a rejected applicant or delisted video, 50% of the forfeited stake is distributed to the challenger and majority voters in the challenge (for example, in a 25% / 25% proportion), and the other half goes to all token holders, weighted by their stakes on the list (if a token holder has zero stake in the given list, no rewards are assigned). The minority voters neither gain nor lose, receiving back their stake, subtracted of a potentially small penalty. The applicant or listee under challenge is *slashed*, besides being removed from the list.

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

##### _MIN_DEPOSIT_
The amount, in PTI tokens, a candidate must lock as a deposit whenever submitting a video, and which must remain "bonded" for the duration of the listing thereafter.

##### _APPLY_STAGE_LEN_
The duration, in blocks or other defined period (e.g. epoch), during which an application can be challenged. On the original implementation, listing occurs only after the end of this period. The proposal here is to list applicants immediately, and extend the _APPLY_STAGE_LEN_ indefinitely (perpetuating the "challenge window"). Alternatively, we can incorporate cost functions that increase the matching stake required with time once a certain period has passsed and no challenge was made.

##### _COMMIT_PERIOD_LEN_
The duration of the "face-off" phase, in blocks or other defined period (e.g. epoch). Period during which token holders can commit votes for a certain challenge.

##### _REVEAL_PERIOD_LEN_
The duration, in blocks or other defined period (e.g. epoch), during which token holders participating on a face-off can reveal committed votes for a challenge they're taking part of.

##### _CHALLENGER_PCT_
The percentage of the forfeited deposit, in case of a rejected application, after a challenge, which is awarded to the original challenger(s) that triggered the voting round, winning party in the voting round as a dispensation compensating for their capital risk.

##### _VOTERS_PCT_
The percentage of the forfeited deposit, in case of a rejected application, after a challenge, which is awarded to the winning party in the voting round. The amount "left" for token holders according to their stakes on the list is thus determined by 100% - (_CHALLENGER_PCT_ - _VOTERS_PCT_).

##### _CHALLENGER_PENALTY_PCT_
The percentage of the matching stake, by a challenger, that's slashed in case the majority goes against him or the VOTE_QUORUM is not met. In case this penalty occurs, the amount sums up to the staked deposit associated to the listing, making it more costly to challenge, next time.

##### _VOTING_PENALTY_PCT_
The percentage of the tokens staked into face-off voting, by engaged token holders, that's slashed in case they end up in the minority side. Initially, this will be set out to zero.

##### _VOTE_QUORUM_
The percentage of tokens out of the total tokens commited and revealed in favor of _rejecting_ a challenged candidate that's set as mininum requirement for this candidate to lose listee status. The _VOTE_QUORUM_ does not count non-voting tokens, and unrevealed tokens are considered non-voting. 

The reference implementation we borrow most of these parameters from is in https://github.com/skmgoldin/tcr.


# Treasured Playlists, or child-TCRs

Although the ability to organise content in playlists should be frictionless for any network user, the mechanisms behind TCRs can be extended by users willing to create skin-in-the-game playlists, or to try "outperforming
the curation output" of the main, generic list.

Treasured Playlists are a category of content listings that are subject to the same propose-challenge mechanisms of the parent-TCR, but with a difference: when one is created (and a corresponding TCR contract, deployed), it becomes able to mint its own native staking token. 

Users willing to start a Treasured Playlist (which can range from an organisation trying to secure a crowdsourced list of videos; a common user speculating on a specific list he's made; a promoter willing to bootstrap a group of creators) can initiate the following actions: (1) deploy a [new smart token and market maker contract](https://github.com/bancorprotocol/contracts); (2) stake an amount of PTI into it; (3) it spins off a child-TCR contract with the native token set to be the newly created one; (4) define the distribution of the new token; (5) sponsor applications into the playlist. 

Different from the PTI, these are made nontransferrable, but buyable or redeemable only against the market making contract
that initially holds the “staked amount". The market maker establishes one minimum “stake price” for “buying” the token, and one “redeeming value” for which it can be “sold” back for PTI. Both grow as the “minted” supply of the token grows, and the distance between them too. 

<img src="https://i.imgur.com/yq0miXB.png" width="400">

*[Simon de la Rouviere's representation](https://medium.com/@simondlr/tokens-2-0-curved-token-bonding-in-curation-markets-1764a2e0bee5) of a bonding curve used by a market maker to determine prices.*

The main parameters its creator defines are:

##### _LIQUIDITY_POOL_SIZE_
Percentage of the supply of the new token (standardly defined as equal to that of the parent-TCR native token) to remain locked into the contract's liquidity pool.

##### _OWNER_PCT_
The amount of tokens, in percentage of the supply, to be distributed, or pre-minted, to the list's creator.

The _LIQUIDITY_POOL_SIZE_ is rather locked in the market maker contract, just like the the _MIN_DEPOSIT_ initially staked in PTI by the child-TCR creator - these are effectively the two sides of the market.

Child-TCRs can only be made up of content that’s necessarily on the parent list, although they are supposed to have their own focal points. It is a premise that these focal points "fit within" that of the parent list (e.g. a playlist with the best video lessons on how to play cool songs in the harmonica; which has only non-copyright-infringing content).

As [Simon de la Rouviere puts it](https://medium.com/@simondlr/continuous-token-curated-registries-the-infinity-of-lists-69024c9eb70d), "anyone can create a list, set criteria for inclusion, and play the crypto-economic game to include or exclude actors. The value of the registry will be determined by how useful it is and whether it acts as a good schelling (focal) point. This can also create a long tail of categorisation that wasn’t necessarily feasible or possible before".

Lists with high demand of people wanting to submit to it (because they’re well curated and have their audience) earn revenue for its token holders, since its token raises in value programatically as the number of token holders grow (and vice versa).

Challenges against playlists can also be made, according to the same rationale beforementioned. In the case, the stake to be matched would be that initially deposited to deploy the child-TCR token and playlist. Eventually, when multiple playlists with overlapping objectives coexist, there could even be a way to merge lists and combine stakes to increase the difficulty of challenging a child-TCR and to widen participation on it. 
