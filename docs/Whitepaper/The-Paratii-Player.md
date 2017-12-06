# The Paratii Player
**TBD: a short intro about how it's a standard, modular client for bundling access to all the underlying services of the protocol.**

## Key Hierarchy
The embeddable nature of the Paratii player and its underlying p2p dynamics present some requirements, from the end-user point of view:

- It must be able to execute micropayments, handling PTIs and ETH itself.
- It should not explicitly ask for permission at every transaction to be performed.
- The user's private key must not be stored in plain sight, nor touch any form of storage if not in password-protected form.

Although possible, it would be both unwise and a general security risk, for users, to relinquish all control of PTI and ETH to the Paratii player. It's reasonable, however, for a subset of these tokens to be kept in a locally accesible wallet, while the bulk of one's funds is kept safe on a separate contract account, controllable only by its `owner`. For the sake of clarity, these will be called respectively `smallWallet` and `bigWallet`.  

Users have a master key they can use to unlock their in-player wallet. This **spending** key can sign, validate, and invalidate **posting** keys used to cast "votes" on videos when it comes to ratings, flagging and moderation.

**TBD: a diagram explaning relationships among the keys mentioned (+IPFS keys + ETH keys? can we use bip32 for the child key?).**

## Protecting User Funds
The `smallWallet` is a javascript lightwallet that stores encrypted private keys in the browser for interacting with Ethereum contracts even without running a local node. It can be unlocked with a password and the keystore (held in localDB), then connected via a hooked web3 provider to a remote node which relays transactions. Once unlocked, it's controllable by both its `owner` and `ParatiiAvatar.sol` **(right @jellegerbrandy?)**, but can only interact with a number of whitelisted contract addresses. This wallet's purpose is twofold: to pay for videos on `VideoStore.sol`; and to open/close channels enforced through an onchain `brokerContract`, for sending and receiving micropayments related to fileshare accounting, under thresholds delimiting the size of outgoing payments and the maximum frequency with which they can happen. An additional safety measure is to trigger the transfer of excess funds to the address' respective `bigWallet`, whenever a threshold of tokens under custody is crossed. 

The `bigWallet` is an EOA that, besides the abovementioned funds, directly receives desimbursements from `ParatiiAvatar.sol` in respect to the address' mapped share on the redistribution pool payout. It's controllable only by its `owner` and explicitly requires the password to execute transactions.

**TBD: polish this rough scheme:**

![Browser Diagram](https://i.imgur.com/DOzTc9E.jpg)

## Non-registered users & exceptional situations
**TBD: 
- Anonymous navigation.
- Change password.
- Different devices.
- Players on multiple tabs (service workers to the rescue!).**

## Extensibility
**TBD: Plug-in oriented architecture.**

## General Use Flow
**TBD**

## Example Use Cases

### Crowdfunding
**TBD**

### News
**TBD**

### Distance Education
**TBD**
