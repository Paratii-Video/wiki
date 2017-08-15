# Paratii

**Blueprint for a trustless video-on-demand distribution system**

Diego di Caro diego@paratii.video, Jelle Gerbrandy jelle@paratii.video, Paulo Perez paulo@paratii.video, Felipe Sant’Ana felipe@paratii.video

## Abstract

The world of online video is undergoing a radical transformation that blurs the distinction between service providers, value providers and end users. In the present document, we outline a video-on-demand distribution system that leverages recent developments in the fields of peer-to-peer networking and cryptoeconomics to move beyond the model of centralized platforms and place full revenue control on the hands of content producers.

## Contents

* I. Introduction	
  * I.a. A novel opportunity in video distribution	
    * Value proposition 1: in-browser peer-to-peer filesharing	
    * Value proposition 2: microeconomic fairness to all key network participants	
  * I.b. The vision of Paratii	
* II. The _paratii_ token	
* III. Crowdsourcing the video distribution stack	
  * III.a. Decentralised Storage Networks	
  * III.b. Reputation and Governance	
  * III.c. _Proof of Quality_	
  * III.d. Content Policy & Rights	
  * III.e. Multiplexing	
* IV. _Proof of View_	
* V. Selling Advertisements	
* VI. The economics of Paratii	
  * VI.a. Issuance Model	
  * VI.b. Distributing _paratii_ - who earns what	
  * VI.c. A communication market	
* VII. Implementation	
  * VII.a. Road Map	
  * VII.b. Paratii Foundation and capital allocation	
  * VII.c. Risks & Challenges	
* VIII. Conclusion	
* IX. References

## I. Introduction
Digital video advertising moves close to U$20 billion in annual expenditure [1], and still replicates power structures of analog models. This document outlines a system designed to be a decentralised alternative to legacy platforms, freeing the value they withhold from content creators and their audiences, through three main principles:
* Distribution of earnings via a rich set of economic interactions among the individuals that sustain the ecosystem, including a flexible and creator-centric monetization scheme that allows the coexistence of AVOD, TVOD and SVOD [2] business models, as well as the appliance of emerging tokenized ones.
* Use of peer-to-peer networks to minimize file distribution costs and unlock more value for content producers.
* No intrinsic value cuts for maintaining centralized for-profit entities.

Behind this effort is a team that combines decades of advertising market practice, blockchain technical expertise and game theory academic background. Development is kickstarted under the sponsorship of BossaNova, one of Brazil’s biggest film production companies.

## I.a. A novel opportunity in video distribution
Online video distribution is currently dominated by an oligopoly of a few large players. The habit of offering content in exchange for free hosting (and, in some cases, a fraction of the revenue they generate) led to a rising dependence on private networks: as of 2015, more than 50% of all transatlantic data flows relied on networks of this kind [3], while video’s share of global internet traffic already surpassed 70%. Facebook and Google now account for practically half of all money spent on digital advertising worldwide [4], a concentration hardly ever approached on other electronic and even print channels.

Of particular interest for the field are peer-to-peer file sharing systems, of which Bittorrent, which has over 170 million monthly active users [5] (close to twice as many subscribers as Netflix had at the end of 2016 [6]) is probably the most well-known example. However, Bittorrent lacks a proper incentive structure: users are not rewarded in a systematic way for providing bandwidth and storage capacity, and the network must therefore depend on the goodwill of its participants to offer these. A number of projects aim to use blockchain-based accounting and a game theoretic approach for adding incentives and creating truly sustainable peer-to-peer exchanges of storage and bandwidth. These developments are inevitably reshaping the digital content distribution landscape, lowering the barrier of entry to a market whose oligopoly is partly based on hosting and delivering video at a cost that is unattainable for new entrants. 

### Value proposition 1: in-browser peer-to-peer filesharing

The abovementioned initiatives not only promise a diminution of the costs associated with hosting and distribution - they also differ from centralized solutions in how such systems scale. Traditionally, audience becomes expensive in the sense that the more demand there is for a certain video, the more bandwidth it requires (popularity is a multiplying factor on the streaming cost equation). Decentralized approaches twist this formula to avoid scaling bottlenecks: as more people demand a file, competition among nodes to serve it increases, lowering retrieval costs. 

In addition, allocation of costs is fundamentally different: given that value exchanges are triggered and executed on a peer-to-peer basis, media delivery doesn’t need to be paid for by a central account or a single agent. Thus, the playing field for content providers is levelled. Instead of paying for for hosting and distribution itself, the users of the networks can now “pay” for accessing content by making content accessible themselves. 

Furthermore, open source libraries under very active development [7] are gradually proving it possible to achieve high quality, low-latency streaming directly in the browser via WebRTC [8] peer connections. The Paratii player leverages this capability to provide frictionless access to a fair video distribution system, while remaining competitive to legacy platforms in terms of user experience and performance.

### Value proposition 2: microeconomic fairness to all key network participants

Current dominant video distribution solutions are arguably unfair to their key stakeholders: 

* _Content creators_ have little or no control over the monetisation model that is used to sell their work. Either way, the revenue they generate will undergo significant taxation by a for-profit entity.
* _Users_ have their attention sold for fractions of cents in AVOD models (measured in impressions), while their personal data is mined and sold for profit - in most cases, users are essentially marginalized in the economic equation, under the premise of free content and basic service.
* _Attention marketers_ (advertisers, sponsors and agents alike) to whom audience is a key metric must trust third-parties for what they buy. Incentives between them are clearly unaligned: fraudulent charging mechanisms of traditional platforms not only account for fake traffic, but are also reportedly more permissive with such when audience is being paid for [8.5].

In short, participants in the video distribution ecosystem are effectively disenfranchised. There are emergent blockchain technologies tackling such issues: Yours.org [9], Basic Attention Token [10] and AdChain [11] (respectively focused at content creators, users/attention marketers and attention marketers) are examples. Paratii’s proposition is to reward all network participants simultaneously, offering access to an Ethereum-compatible base layer of services for media distribution, through a single open source software client that’s fueled by a liquid native token.

The advent of cryptographic assets makes it possible to design socio-economic systems that are governed by novel, programmable types of incentives and interactions. Paratii explores these possibilities in a variety of manners. First, by incorporating media distribution activities in the economic system (users can earn tokens by providing bandwidth, which can then be exchanged for video content as well). Second, by creating a communication market in which content creators can choose between different ways of monetizing their content - from classical pay-per-view or advertisement-based payments to individualized price negotiations. Moreover, users can enjoy the platform as mere consumers, but also become active in offering curating services or content sharers, being subject to tailored remuneration for such contributions. Paratii’s design goal is to present a framework within which a rich set of economic interactions can make for a more cost-efficient system, ultimately translating into higher earnings for content producers and multiple advantages for users.

