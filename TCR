# TCRs with Dynamic Staking

We add a new possibility to the "basic" TCR contract: the possibility to add stakes or challenges.  It works like this:

* Each item `i` in the registry has `staked_i` tokens that are staked in favor of the item remaining in the list, 
and `challenge_i` tokens that are staked in favor of removing the item.
* To add an item to the list, the submitter need to stake at least `MIN_STAKE` tokens to the `stake`
* [to simplify, we set the challenge period to 0: an item is by default _in_ the list, and can be challenged for removal]
* While the item is in the registry but not being voted on, other participants can add tokens to either `staked_i` and `challenge_i`. 
(This is the "faites vos jeux" period or the "staking stage")
* As soon as `challenge_i >= MIN_STAKE`, the staking stage ends, and the voting stage is opened. (* this is probably wrong)
* Voting works just as in the canonical TCR: there is a fixed voting period, any token holder can vote
* After the voting period ends, the votes are tallied. The tokens of the loosing parties are divided among the voters and the winners.
If the item is rejected, the `staked_i` tokens are divided betwen the token holders from `challenge_i` as well as all token holders that cast a vote.


This description is still grossly underspecified, but it allows from some more sophisticated signaling by stakes and challengers. 
Note that the more tokens are staked towards an item, the lower the payout. 
That means that rational token holders are expected to stake more towards items that are perceived to be "low risk", and less
it implies that stakes (and "counterstakers") can signal their
belief in the probability that an item will remain in the list or be removed. 
To be more precise, given a subjective probability of `P_i` that 
the item will be voted to reamin in the list, the expected payout for each staked token is [* disregarding opportunity costs!]:

      P_i * challenge_i / staked_i
      
 Rational stakes will add to `staked_i` if this value is `> 0`, and to `challange_i` if this value is `< 0`. 
 If we oversimplify and assume that `P_i` is the same for all token holders, there is a natural equilibrium where
 `P_i * challenge_i/staked_i = 0`, or `challenge_i/staked_i = P_i`.
 
 One problem with the "there is a fixed stake that can be challenged by matching that stake" approach of TCRs is that, 
 more attractive to challenge the "easy" iterms, i.e. those with a high probability of either winning or loosing the challenge. 
 Items that are somehow "ambiguous" may never be put to the vote simply because challenger will go for the low-hanging fruit. 
In the proposed schema, the "easier" votes will attract more investements, and the winnings will have to be shared among more people, and the payout will be lower.


## Prediction Markets

One problem with large lists is "bandwidth".
It is simply infeasible that a large number of people will vote on a large number of items. 


With the proposed scheme, the stake-challenge mechanism becomes effectively a prediction market w.r.t. outcome of the vote.
This is similar to Matan Field's "holacracy", which is an approach where you do not have all ppl vote on all items 
(which would be ifeasible if there are many items to vote on), but instead have a prediction market predict the outcome of the vote,
and *use that prediction to decide instead of the actual vote*. 
Actual voting only take place occasionally (*randomly?), to keep the predictino markets in good behavior.

Or, in other words, the decision on keeping an item in the registry is only occasionally taken by a plenary vote among all token holders.
In most cases, "professional" speculators decide whether to keep items in the registry, by betting on the outcome of the vote.






 
        
 
