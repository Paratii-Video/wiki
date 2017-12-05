# The Paratii Player
**TBD: a short intro about how it's a standard, modular client for bundling access to all the underlying services of the protocol.**

## Key Hierarchy
The embeddable nature of the Paratii player and its underlying p2p dynamics present some requirements, from the end-user point of view:
- It must be able to execute micropayments, handling PTIs and ETH itself.
- It should not explicitly ask for permission at every transaction to be performed.
- The user's private key must not be stored in plain sight, nor touch any form of storage if not in password-protected form.



private key. The key is stored on the worker’s memory. It touches browser’s LocalStorage in password-protected form only. No service worker would mean the user has to unlock the key on every new page she visits, as there is no centralised key management between the frames.


It would be both unwise and a general security risk, for users, to relinquish all control of PTI and ETH to the Paratii player. It would be reasonable, however, for a subset of those tokens to be kept in a local wallet, **(WORKING HERE)** into a shared wallet or an account from which they could reclaim their ETH and PTI or reload their account balance with more ETH and PTI, and allowed the Paratii Player to spend tokens and ETH in accordance with the paratii contract. We'll call this `bigWallet` and `smallWallet`, for the sake of clarity.

In this sceario, it would be a good security practice to have a key heirarchy for users to spend their tokens through the paratii player. Users will have a master key they can use to unlock their paratii player wallet. This **spending** key can sign, validate, and invalidate **posting** keys used to cast votes on videos for ratings, flagging and moderation.

## Handling User Info

**TBD: a diagram explaning relationships among the keys mentioned (+IPFS keys + ETH keys).**

**TBD: polish this rough scheme**

![Browser Diagram](https://i.imgur.com/DOzTc9E.jpg)

## Extensibility

## General Use Flow

## Example Use Cases

### Crowdfunding

### News

### Distance Education
