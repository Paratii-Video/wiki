# **Paratii**
### **Blueprint for a trustless video-on-demand distribution system**

_Diego di Caro diego@paratii.video, Jelle Gerbrandy jelle@paratii.video, 
Paulo Perez paulo@paratii.video, Felipe Sant’Ana felipe@paratii.video_

_[paratii.video](www.paratii.video)_ | _August 2017_ | _version 0.2_ | _**under adjustments**_

## Abstract				

The world of online video is undergoing a radical transformation that blurs the distinction between service providers, value providers and end users. In the present document, we outline a video-on-demand distribution system that leverages recent developments in the fields of peer-to-peer networking and cryptoeconomics to move beyond the model of centralized platforms and to place full revenue control in the hands of content producers.	

*This is an alpha version of this document. It’ll be continually updated until the deployment of 
Paratii Player 1.0 - please check back for changes.*


## Content			

* [[I. Introduction|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#i-introduction]]
 * [[I.a. A novel opportunity in video distribution|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#ia-a-novel-opportunity-in-video-distribution]]	
  *   [[Value proposition 1: in-browser peer-to-peer filesharing|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#ia-a-novel-opportunity-in-video-distribution]]
  *   [[Value proposition 2: microeconomic fairness to all key network participants|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#ia-a-novel-opportunity-in-video-distribution]]	
 *   [[I.b. The vision of Paratii|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#ib-the-vision-of-paratii]]
* II. [[The *paratii* token|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#ii-the-paratii-token]]	
* III. [[Crowdsourcing the video distribution stack|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#iii-crowdsourcing-the-video-distribution-stack]]
*  [[III.a. Decentralised Storage Networks|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#iiia-decentralised-storage-networks]]
*  [[III.b. Reputation and Governance|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#iiib-reputation-and-governance]]	
*  [[III.c. *Proof of Quality*|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#iiic-proof-of-quality]]
*  [[III.d. Content Policy & Rights|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#iiid-content-policy--rights]]
*  [[III.e. Multiplexing|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#iiie-multiplexing]]
*  [[IV. *Proof of View*|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#iv-proof-of-view]]	
*  [[V. Selling Advertisements|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#v-selling-advertisements]]
*  [[VI. The economics of Paratii|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#vi-the-economics-of-paratii]]
*  [[VI.a. Issuance Model|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#via-issuance-model]]	
*  [[VI.b. Distributing *paratii* - who earns what|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#vib-distributing-paratii---who-earns-what]]
*  [[VI.c. A *communication market*|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#vic-a-communication-market]]	
*  [[VII. Implementation|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#vii-implementation]]
*  [[VII.a. Road Map|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#viia-road-map]]
*  [[VII.b. Paratii Foundation and capital allocation|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#viib-paratii-foundation-and-capital-allocation]]
*  [[VII.c. Risks & Challenges|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#viic-risks--challenges]]
*  [[VIII. Conclusion|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#viii-conclusion]]
*  [[IX. References|https://github.com/Paratii-Video/paratii-player/wiki/Paratii-Blueprint#ix-references]]	

## **I. Introduction**

Digital video advertising moves close to U$20 billion in annual expenditure [1], and still replicates the power structures of legacy models. This document outlines a system designed to be a decentralised alternative to these legacy platforms and to free the value they withhold from content creators and their audiences, through three main principles:

1. Distribution of earnings via a rich set of economic interactions among the individuals that sustain the ecosystem, including a flexible and creator-centric monetization scheme that allows the coexistence of AVOD, TVOD and SVOD [2] business models, as well as the application of new tokenized earning models.

2. Use of peer-to-peer networks to minimize file distribution costs and unlock more value for content producers.

3. No intrinsic value cuts for maintaining centralized for-profit entities.

Behind this effort is a team that combines decades of advertising market practice, blockchain technical expertise and game theory academic background. Development is kickstarted under the sponsorship of BossaNova, one of Brazil’s biggest film production companies.

## I.a. A novel opportunity in video distribution

Online video distribution is currently dominated by an oligopoly of a few large players. The habit of offering content in exchange for free hosting (and, in some cases, a fraction of the revenue they generate) led to a rising dependence on private networks: as of 2015, more than 50% of all transatlantic data flows relied on networks of this kind [3], while video’s share of global internet traffic already surpassed 70%. Facebook and Google now account for practically half of all money spent on digital advertising worldwide [4], a concentration hardly ever approached on other electronic and even print channels.

**Value proposition 1: in-browser peer-to-peer filesharing**

Of particular interest for the field are peer-to-peer file sharing systems, of which Bittorrent, which has over 170 million monthly active users [5] (close to twice as many subscribers as Netflix had at the end of 2016 [6]) is probably the most well-known example. However, Bittorrent lacks a proper incentive structure: users are not rewarded in a systematic way for providing bandwidth and storage capacity, and the network must therefore depend on the goodwill of its participants to offer these. A number of projects aim to use blockchain-based accounting and a game theoretic approach for adding incentives and creating truly sustainable peer-to-peer exchanges of storage and bandwidth. These developments are inevitably reshaping the digital content distribution landscape, lowering the barrier of entry to a market whose oligopoly is partly based on hosting and delivering video at a cost that is unattainable for new entrants. In a sense, the playing field is leveled.

Incentivized file-sharing systems not only promise a diminution of the costs associated with hosting and distribution - they also differ from centralized solutions in how such systems scale. Traditionally, audience is expensive: the more demand there is for a certain video, the more bandwidth it requires (popularity is a multiplying factor on the streaming cost equation). Decentralized approaches twist this formula to avoid scaling bottlenecks: as more people demand a file, competition among nodes to serve it increases, lowering retrieval costs. 

In addition, allocation of costs is fundamentally different: given that value exchanges are triggered and executed on a peer-to-peer basis, media delivery does not need to be paid for by a central account or a single agent. Instead of paying for for hosting and distribution itself, the users of the networks can now "pay" for accessing content by making content accessible themselves. 

Furthermore, open source libraries that under very active development [7] are gradually making it possible to achieve high quality, low-latency streaming directly in the browser via WebRTC [8] peer to peer connections. The Paratii player leverages this capability to provide frictionless access to a fair video distribution system, while remaining competitive to legacy platforms in terms of user experience and performance.

**Value proposition 2: microeconomic fairness for all key network participants**

Current dominant video distribution solutions are arguably unfair to their key stakeholders: 

* *Content creators* have little or no control over the monetisation model that is used to sell their work. Either way, the revenue they generate will undergo significant taxation by a for-profit entity.

* *Users* have their attention sold for fractions of cents in AVOD models (measured in impressions), while their personal data is mined and sold for profit. In most cases, users are essentially marginalized in the economic equation, under the guise of "free" content and basic service.

* *Attention marketers* (advertisers, sponsors and agents alike) to whom audience is a key metric must trust third-parties for what they buy. Incentives between them are clearly unaligned: fraudulent charging mechanisms of traditional platforms not only account for fake traffic, but are also more permissive with fake traffic when audience is being paid for [9].

In short, participants in the video distribution ecosystem are effectively disenfranchised. There are emergent blockchain technologies that aim to tackle such issues: Yours.org [10], Basic Attention Token [11]  and AdChain [12] (respectively focused at *content creators*, *users/attention marketers* and *attention marketers*) are examples. 

Paratii’s proposition is to reward all network participants simultaneously, offering access to an Ethereum-compatible base layer of services for media distribution, through a single open source software client that’s fueled by a liquid native token.

The advent of cryptographic assets makes it possible to design socio-economic systems that are governed by novel, programmable types of incentives and interactions. Paratii explores these possibilities in a variety of manners. First, by incorporating media distribution activities in the economic system (*users* can earn tokens by providing bandwidth, which can then be exchanged for video content as well). Second, by creating a *communication market* in which *content creators* can choose between different ways of monetizing their content - from classical pay-per-view or advertisement-based payments to individualized price negotiations. Moreover, *users* can enjoy the platform as mere consumers, but also become active by offering curating services or by sharing content, and will receive tailored remuneration for such contributions. Paratii’s design goal is to create a framework within which a rich set of economic interactions results in a more cost-efficient system, ultimately translating into higher earnings for content producers and multiple advantages for users.

### I.b. The vision of Paratii

The Paratii player interfaces with a decentralized video distribution system that presents a disintermediated, fair and open alternative to centralized platforms. The system’s core proposition is to showcase videos that transform or influence people in a positive way - according to people’s own judgement - and to reward the creators of these videos accordingly. Paratii introduces a cryptographic token (the PTI), that is exchangeable for hard currency, and whose distribution depends, in part, on people’s own perception of how much particular pieces of content affects them. This is a measure to ensure that not only makers of commercially successful videos will earn, but that also the long tail is rewarded for the value it produces for the network.

    PTI = Power to Transform Individuals
    (The more your content inspire others, more of this power you earn. The more PTI you hold, the more of this power you can exert.)

In the sections below, two different mechanisms to achieve this goal are described. Firstly, part of the income of video makers depends directly on the ratings given by their viewers. Secondly, individual viewers can directly influence video makers’ income by participating in a *communication market*. Instead of defining a monetisation model from the beginning, like most video-on-demand platforms traditionally do, Paratii leaves this decision to each content producer. Interactions triggered by viewers, advertisers and other parties can increase or decrease everyone’s final earnings. 

The implementation of this vision in a smoothly functioning PTI distribution mechanism makes the concepts of an "end user" or of a “mass market” meaningless. In lieu, fine-grained individual choices activate a feedback loop between viewers, creators, curators and other agents that contribute to the network. This opens up the possibility of organising cooperation in a fully autonomous and decentralized fashion, through an entity that “lives” on the Ethereum blockchain, manages its own capital, whose backend logic has no for-profit built-in taxation and on which internal parameters are tweakable by the participants themselves.

## **II. The _paratii_ token**

Paratii presumes the existence of a currency unit with specific utility to oil the engines of value exchange within its network - the *paratii* token, or PTI, for short. Its basic circulation is simple: contributors to the network receive PTI, while expenditure for media inside Paratii is paid for with PTI. 

**IMAGE 1 HERE**
*A diagram of basic value flows inside Paratii’s system* 

We expect the main driver of the circulation to be a combination of expenditure on paid videos (*pay-per-view*) and advertising revenue. The latter, together with newly issued PTI *(see section VI.a)*, will be distributed to contributors of the ecosystem. Content creators, curators, audience validators, video sharers and viewers have their contribution inputs mapped and earn in proportion to the value they add to the network. 

Besides making and consuming content, a wide range of tasks - from transcoding video files to tracking delivered advertisements, from identifying botnet activity to recommending content - must be handled to make an apparently simple watching experience possible.

Modularizing each service in this range (in opposition to the traditional approach of stacking them in a proprietary bundle) allows us to build a media player upon existing decentralised protocols for use cases that apply, while implementing additional needed services ourselves.

In other words, a guiding principle is to crowdsource work as much as possible. To pay for this, Paratii combines two capital streams into a "redistribution pool" *(see section VI.b)*. The first is a portion of the revenue video makers generate through their monetisation model of choice. The second comes from a mining process that will gradually release new tokens to this “redistribution pool” over a period of 15 years *(see section VI.a)*. 

## **III. Crowdsourcing the video distribution stack**

 Paratii aims to leverage decentralized protocols of the Ethereum ecosystem in support of its player, wherever useful and viable. Various projects in the space solve or aim to solve specific issues that incur to the operation of a video distribution system (their native accounting tokens are noted in parenthesis): 

* Decentralised Storage Networks (DSNs) [13]: Swarm (ETH), IPFS (FIL), Storj (STORJ, an ERC20 token).

* Media multiplexing and live streaming: LivePeer (LPT, an ERC20 token).

* Off-chain Computation: TrueBit (ETH).

* Decentralised liquidity providers: Bancor (BNT, an ERC20 token), 0x (ZRX, an ERC20 token), RaidEx (undisclosed, probably ETH), Swap (SWAP, an ERC20 token).

* Global identity validators: Humaniq (HMQ, an ERC20 token), Keybase (not present), Consensy’s uPort (not present yet), piggy-back type solutions such as register.eth.

The list of projects is long and ever growing - it’s still early to predict how successful and competitive each will become in comparison to classical solutions. Therefore, modularity and agnosticity are requirements for future-proofing the system.

There are roughly three approaches to how systems integration can be done. A classical "*outsourcing*" rationale simply assigns tasks to a third party and expends capital accordingly. “*Complete integration*” refers to incorporating existing algorithms  (for example, deploying a Bancor-derived smart contract as part of the Paratii codebase). Tokenized systems - a category most of the projects mentioned above fall under - make a third approach possible: integration at the “*protocol level*”, by tying these different systems together in single software client and providing liquidity through an automated market maker and the PTI native token, such as described, for example, in the next section on DSNs.

Complete decentralization is a medium-to-long-term vision *(see section VII.a)*. In this section, we address a number of the abovementioned fundamental issues for a functional video player, and evaluate scenarios on how these solutions can be incorporated or prototyped.

### III.a. Decentralised Storage Networks

Users of the Paratii player automatically take part in the file distribution process, not just downloading content, but making it available to other viewers as well. 

Concretely, this means that the player will be a peer in the DSN, sending and receiving chunks of files, paying and being paid accordingly. Viewers can earn small amounts of DSN’s native tokens by streaming files, or will need to pay correspondent amounts if they download more than they make available. 

Behind the scenes, Paratii provides liquidity - i.e. it makes it very easy to convert any excess balance to PTI, or vice versa. In practice, this means that viewers may exchange bandwidth (which is paid for in the DSN’s coin) for access to a premium video or for ad-less versions of free videos (paying in PTI).

Even if such interactions may turn relatively complex, user experience can be made seamless and transparent by abstracting the details under the hood. The fact that Paratii player manages tokens other than PTI will not be standardly visible to end users, who manage a total equivalent balance in PTI. 

As a condition for content producers’ full control on revenue, they are also fully responsible for the availability of their own files. This is pivotal for breaking the vicious cycle established by traditional platforms whenever creators get used to "selling out" their content in exchange for free hosting. Availability of files can be guaranteed by different mechanisms, depending on the DSN in question: a creator can host the files herself, or pay someone else to do this. In the latter case, the costs of guaranteeing file availability are relatively small [14], and PTI earned at the moment of account validation should be enough to cover costs of publishing a small number of videos. Since accounting happens under the hood, the requirement may be noticed only by heavy users.

Paratii is committed to the Ethereum ecosystem, of which Swarm forms an integral part (its development is conducted by the Ethereum Foundation and its incentives mechanism and content addressing scheme run on the Ethereum blockchain). This makes it DSN of choice for Paratii in the long term. However, technical restrictions (the player needs a javascript implementation with webRTC support) might delay integration of Swarm in favour of frameworks that are already compatible.

### III.b. Reputation and Governance

Paratii’s decision making processes happen in various spheres: 

* Institutional level: e.g. deciding on strategic partnerships; allocating capital.

* Technological level: e.g. inclusion/subtraction of modules in the technology stack; changing or adding new features.

* Daily micromanaging level: e.g. mediating disputes related to content property; detecting and unlisting "offensive content" and generally deciding on the quality of content

As it’s become standard practice in the field, a reputation system will be implemented and used to measure individual influence. Paratii is committed to be community-driven from day one, but rushed implementations of such systems carry serious risks [15]. The approach adopted is to initially put in practice a partially decentralised governance system *(see section VII.b)*  for daily micromanaging,  iterating and broadening its scope as the community matures.

Each user has a certain "reputation score" that determines the amount of influence held in the various decision-making processes. Reputation is inherently linked to the agent who earned it, can never be transferred to anyone else, and is accumulated by contributing to the platform in a positive way. It can be both assigned as a precise amount in reward for specific actions (a straightforward approach well-tested by platforms such as Reddit and StackOverflow [16]), and be redistributed among users who virtually bet against each other through a curation mechanism that returns staked reputation with positive or negative interest through Backfeed-style [17] feedback loops that automatically reward trend-setters and decisions that turn out to be aligned with the community’s consensual evaluation.

Initially, most reputation will be assigned to a limited group of trusted users. This initial "seeding" is important because it sets the “value system” of the organization. As the community grows, the initial reputation is gradually diluted among other trusted users, thereby gradually transferring influence from the original initiators to a more extended group in an organic fashion. Users who complete a validation process automatically receive a small amount of reputation.

It’s dangerous to depend on a one-user-one-vote system - specially in cases where it is easy to create new user accounts. The stockholders’ model, that ties the influence of users directly to their economic weight, avoids this risk, but means that power is in the hands of the rich, and can be bought and accumulated. A reputation system mitigates both issues by giving relevance to users with a proven track record, as represented by their reputation score. Severing the link between money and influence (between tokens and reputation) opens up the possibility to precisely control the accumulation of influence (for example, by imposing maximum limits on the reputation a single individual may hold) without interfering in the free tradability of the tokens themselves.

### III.c. _Proof of Quality_

A major goal of Paratii is to reward videomakers not only on the basis of their immediate commercial earnings (those from selling advertisements), but also on basis of the quality of their content. To obtain this goal, the price-discovery mechanisms of the *communication market* *(see section VI.c)* are complemented with a system that makes the earnings of a video maker depend directly on his audience explicit evaluation of his videos. 

In its first iteration, Paratii’s *Proof of Quality* is a simple mechanism in which viewers are encouraged to provide a binary judgment, subsequently aggregated on the basis of the amount of reputation they have. This results in a "quality score" that is used to calculate the proportion of earnings from the “redistribution pool” a video maker can earn in addition to “direct” revenue from advertisements or pay-per-view. The amount of PTI that is available from the redistribution pool is subject to “difficulty” adjustments (translatable to the number of views or contributions needed to earn rewards): the amount will be deseasonalized, in order to signal the real growth of the platform while avoiding the noise generated by ephemeral factors. 

Apart from rating videos by a simple thumbs-up/thumbs-down mechanism, users can also label the content they watch through the insertion of tags. Categorization of content is important not only because it favours content discoverability, but also because it increases efficient placement and enrichens the value proposition to advertisers. Paratii follows the IAB taxonomy [18] in the suggestion of tags, but viewers can also create their own.

### III.d. Content Policy & Rights

As an open video system, Paratii must offer tools for copyright holders to claim *licensed content being infringed*, as well as some mechanism to collect subjective user input on the matter. The same applies to *spam* and *offensive material* in general. 

An interface for "flagging" will allow users to (limitedly) bet reputation on videos they think shouldn’t be there for any of the reasons above. However, efficiently excluding - and not only signalling - malicious content [19] is in some sense the “bottom line” of curation. Having clear thresholds for determining a video “suspicious” (both in terms of “flags” *vs*. “ratings” and in terms of “upvotes” *vs.* “downvotes”) is a first step.

A straightforward design can rely on a reputation-weighted "bounty hunting" scheme to provide rapid detection of potentially malicious videos, but also on a safe logic to only punish content whose judgement upon has been securely assessed. This can be achieved by randomly selecting validators and to assign a bounty proportional to the video’s earnings under judgement to those who end up aligned with the final outcome of a multiple-round consensus-seeking process, returning stakes with positive or negative interest. 

A complementary gamified approach to the detection of copyright infringement can also emerge as a corollary of a *d**roit de suite** *content monetisation model (*see section VI.c*): recently uploaded videos might have their initial "share price" (as for investors, not consumers) determined by algorithmic factors such as the account’s past audience, creator’s reputation, averaged rating of the account’s videos, averaged returns of content similarly classified (tagged), associated licenses, and so on. The price of a video’s “share” can then be correlated to its “total earnings” (including revenue coming in as *bonus* and the *communication market*), going to zero if the content is delisted or deemed offensive by the community. You’d want to “buy in” great viral content early, but stay away from potentially infringing stuff. Videos with increasing view counts, detached from their “share” price, can trigger the validation logic abovementioned, leveraging the market itself as a signalling tool.

The Paratii foundation* (see section VII.b)* may adopt an additional set of procedures to protect against abovementioned threats, ranging from automatic monitoring systems to security checks that make it difficult for potentially fraudulent users to "cash out". It will always make itself available to engage in discussion with offended users or alleged holders of property unauthorizedly being broadcast via the system.

Licensing rights are intimately linked to the monetisation model of content, and thus traditionally imposed by legacy digital content distribution channels. Blockchains change the field by allowing producers to effectively gain fine-grained control of their intellectual rights. It is however still unclear how to migrate existing attributions and licenses from legacy registries to a blockchain-friendly format. By using a canonical data model for representing an object stored on the blockchain, one can register unique identifiers, linkable across protocols, on a DSN, leaving aside minimal information to be input to a smart contract. This brings to light unprecedented possibilities for producers to define their license of choice for each new creation. 

Coala IP [20] is an open source protocol designed upon IPLD [21] namespaces for representing licenses as hash-linked data structures that can be traversed from metadata into Ethereum into other merkleized structures. It can be implemented to record attribution information and authenticate claims, interfacing with standard "handler" contracts that encode rights in specific terms as well as with* content contracts* *(see section VII)* linked to their associated videos.

### III.e. Multiplexing

Video and audio that is streamed over a heterogenous network needs to play on a wide range of different devices: mobile phones expect different types of screen size and video file formats than a 4K TV does. In video processing, combining audio and video into a single coherent data flow can be done according to a variety of codecs, bitrates and container formats.

To ensure that the requirements of all devices are met, proprietary platforms rely on media servers who convert ("multiplex") source files into suitable formats before handing them over to content delivery networks, or CDNs, that take care of distribution, that perform the following tasks:

* Transcoding: decoding to an intermediate uncompressed format and re-encoding into a target format, in order to support popular codecs.

* Transrating: altering bitrate to support bandwidth-scarce broadcasts and/or adjust bitrate according to available bandwidth, making streaming adaptive.

* Transmuxing: packaging data into various container formats and delivery protocols.

LivePeer [22] is developing a protocol for the incentivized live streaming of content and uses a token (called LPT) for its internal accounting. One of its components is the LPMS [23] an open source media server that allows one to manipulate and broadcast video using RTMP as input format and outputting RTMP/HLS streams. In general terms, in the LivePeer protocol,  job requests are posted to a smart contract together with an escrow in LPT, while transcoders and broadcasters interact to verify input data and ensure enough persistency, perform the job and output the target stream. When the job ends, transcode claim data is to the smart contract in a form that allows for verification, and rewards are distributed accordingly. Integration of Paratii with nodes of the LivePeer network is a way of providing decentralised multiplexing to videos streamed to Paratii users. 

## **IV. _Proof of View_**

Telling apart real people’s views from fraudulent views is difficult. Representative bodies report that, on average, 23% of video ad views are non-human [24], while other estimates range to as high as 75% [25] of fake views. Content that is affected by botnet activity appears to perform very well, lowering publishers’ incentives to pursue and curb this kind of abuse [26]. 

Blockchains offer a new value proposition to advertisers with regard to the measurement of audience, due to their radical level of transparency. *Proof of View* is the name of the current logic and associated smart contract employed by Paratii to leverage this property as efficiently as possible while avoiding redundant transaction costs.

Ideally, an optimal design should ensure that:

* Views (impressions) get registered along with associated navigation behaviour that enriches value offered to *attention marketers* as much as possible.

* Privacy of users is balanced with the needs of *attention marketers*: individual behaviour is untraceable to personal identity.

* Every view that ends up being "paid for" must be registered on a public ledger.

A straightforward approach is to ensure navigation history and behaviour remain on the client side for each individual logged in user, before being anonymized [27], signed by the end user application and stored as "proofs of the views" in a DSN, referenceable through hashes. 

Subsets of these views can then be packaged by the Open RTB API (*see next section*) as "pools of views" that share common attributes (as in segmentation), and are made available real-time to *attention marketers* to buy. The requirement of speed is prohibitive to a common blockchain with more than sub-second delays (e.g. Ethereum), and the requirement of public availability makes the use of one-to-one state channels unfeasible. Hubs or many-to-many state channels are the most feasible alternatives for providing trustlessness and scale to the mechanism (also heavily dependent on zk-SNARKS implemented in Ethereum). This remains a research front.

Eventually, close to all major information about traffic history of logged-in users gets written on a public ledger, though not individually. This means that advertisers do not need to trust Paratii for its figures: it is simply impossible to mislead them by misrepresenting the data [28]. 

Needless to say, traditional methods can complementarily be implemented in a centralized manner (i.e. calling an oracle) to fraudulent activity, mostly combining traffic analysis (distribution of impressions during the day, identifying anomalous changes in conversion rates, identifying sources of traffic by IP or other means) and analysis of individual user behaviour. 

In any case, fraud prevention against fake traffic is an arms race with botnets and malwares of all sorts - it’s unclear how a decentralised entity can compete with resource-intensive industry-sponsored computing efforts dedicated solely to hunting for botnets. Some interesting approaches come from within the blockchain research field. MetaX’s AdChain [29] aims to provide a public verifiable whitelist of publishers and platforms accredited through a native token and a challenge-response game theoretic setup. Basic Attention Token proposes a novel monetisation model that accounts literally for user’s time, and might circumnavigate the issue as a whole, outdating the notion of "impressions" . 

An alternative way to presently disincentivize fraudulent activity is to make it harder for fraudulent users to make money of their activity. Non-sensitive information regarding user interactions within the ecosystem (i.e. social and some registration information) is public, and provides the base for a complementary approach: in addition to the validation of views, a user, before having access to his funds, must be himself validated by the community. 

When a account reaches a certain threshold, a bounty is set that is proportional to that account’s total PTI balance. Invitations to validate the user are sent to random contributors who’ve demonstrated interest in bounty hunting inside the ecosystem and hold high reputation. Once the invitation is accepted, the user may choose to complement these data with sensitive information that is communicated privately (precise geolocation and scanned documents are two examples). Validators’ judgements are binary, and a decision is reached when a predetermined threshold of positive or negative evaluations is met. Accounts with higher balances provide higher bounties, and therefore tend to be validated faster. Users that want to ensure an efficient validation are led to keep hold of their PTI until their balance generates a bounty attractive enough to speed up the process (failed validations decrease user reputation but don’t affect their holdings, up to a certain threshold). Lastly, creators have control over how much sensitive data they want to risk versus how fast and easily they want their accounts to be verified.

## **V. Selling Advertisements**

Different approaches to traffic validation means there’s multiple types of requirements that *attention marketers* can specify when buying audience: Paratii aims to offer impressions from registered users or non-registered users (in whitelisted platforms whose measurements can be requested via oracles); they might be sold through traditional auctioning with slightly delayed delivery, via real-time bidding (RTB), or as future contracts; they might not be traded at all once proper attention measurement metrics [30] are in place.

Real time bidding (RTB) is a standard in the industry.  The speed at which RTB needs to operate presents challenges for Paratii’s underlying protocols. In the software’s first iterations, traditional auctioning will take place. Selling media space in real-time, in exchange for PTI, will be done through an orderbook, to which creators’ *content contracts* publish *ask orders* determining the conditions of the advertisement space being offered. The real time bidding (RTB) auctions for impressions work as follows:

1. When a user requests an advertising-compatible video, the associated *content contract* publishes a bid request in the orderbook conveying attributes about the content in question, the user and the impression that it triggers. For example, the bid request regards a 5 minute video about scuba diving in Patagonia; the viewer is known to be female, from Tokyo, and in her thirties. The offer also indicates conditions of the ad slot being sold - e.g. that the winner should be a 5 seconds pre-roll ad, for a minimum price of 0.010 PTI.

2. Advertisers (or resellers), who must be registered beforehand, have access to an API that allows for monitoring current offerings, placing bids in the orderbook at any time and uploading creatives to the underlying DSN. Bids specify price and conditions (context, keywords, or user demographics) along with a reference to the creative (the actual advertisement to be shown). Bids can also be made as a response to a particular request, and include a certain amount of PTI that is held in escrow and used to settle the bid if it’s accepted. In case it expires, this money is returned to the bidder.

3. For each newly published request, the system waits a short amount of time (in the order of 200 milliseconds) for any new bids to arrive, and then awards the impression to the highest bid in the book that fits the conditions specified by the *ask order,* attributing the winning bid to the impression delivery event that’s registered in the* content contract* as a response to the original bid request.

4. The video player fetches the content associated with the hash provided along the winning bid and shows the advertiser’s creative to the user, in proper conjunction with the video.

5. When the user has seen the ad, the payment is settled by immediately. The video maker receives his share of the bid - the exact sum depends on the amount going to the "redistribution pool" *(see section VI.b)* - and, according to revenue share percentages set by the video owner, some earnings may go to the viewer as well.

**IMAGE2 HERE**
*An overview of the impression auctioning process from the original user request to revenue distribution, steps I to VI*

Advertisers rarely bid directly on individual media slots.  Demand Side Platforms (DSPs), Supply Side Platforms (SSPs), ad networks and ad exchanges are some key intermediaries that act as bridges to publishers. DSPs aggregate inventory offered by publishers and ad networks (who, in turn, aggregate inventory from multiple websites in categories) in order to offer access to varied options and facilitate media buying for advertisers or their agencies. SSPs - Supply Side Platforms - are the counterpart of DSPs: by bundling media slots and offering an accessible interface to publishers and DSPs, they optimize publishers’ revenues and automatize the process of selling ads. Ad exchanges are platforms that execute RTB auctioning and determine winning bids.

Paratii’s ad selling scheme is designed to fit in the existing logic that the industry is already used to. Our roadmap includes the creation of a supply-side platform that intermediates between Paratii’s core contracts and the existing market. This Paratii SSP will follow industry standards as much as possible - in particular implementing the OpenRTB standard [31]. The SSP service allows DSPs to publish orders in the orderbook, and facilitates the bundling of offers into thousands, so third-party brokers can resell Paratii impressions for themselves. The service also offers an integrated exchange of PTI for fiat currency, so that third party brokers can easily integrate the Paratii platform in their existing offers without being obliged to acquire and transfer PTI tokens. The SSP service, finally, also actively seeks bids for the inventory by posting new bid requests to compatible DSPs.

Paratii’s video player will implement the industry standards to be compatible with major DSP services - and in particular the specifications of IAB’s (Interactive Advertising Bureau) Video Ad Serving Template (VAST) [32] and the Video Player-Ad Interface Definition (VPAID). The player supports two main kinds of ads: linear ads (pre-roll, mid-roll, post-roll) and non-linear ads (overlay text or image ads).

Future contracts trading will be made available once a proper supply-side platform establishes an easy interface for *attention marketers* to interact with the abovementioned orderbook, similarly to what NYAX [33] has pioneered in the US. This can funnel speculative capital during early stages, bootstrapping creators’ earnings and value flows within the system (for creators, there’s no established platforms to trade future inventory at scale; for advertisers, what usually lacks in such markers is the possibility of re-trading positions).

## **VI. The economics of Paratii**

### VI.a. Issuance Model

Paratii is a token of finite supply: a total amount of 21 million PTI will ever be created. The majority of the supply *(see section VII.b)* will be issued at a constant rate, targeting a period of 17 years to finish issuance.

PTI is ERC20 compliant [34]. As it’s conventional among similar tokens, PTI will be divisible by 10^18, with larger denominations intended to be used for user level transactions, and smaller scales for accounting and microtransactions.

PTI’s issuance will follow a daily rate of 2400 tokens. The fixed issuance implies a decreasing inflation rate on the outstanding supply, starting from 11.3% in year one and ending with 4,2% after a little more than 15 years.

**IMAGE 3 (CHART) HERE**

After this initial minting period, the *redistribution pool* will rely only on tokens’ circulation within the platform. 

Paratii’s average CPM (cost per thousand impressions) as expressed in dollars is determined by the market, and may be either higher or lower than the price of centralized competitor’s CPMs. This metric is decoupled from the coin’s price, giving content owners and advertisers a stable parameter to their earnings and spendings.

### VI.b. Distributing _paratii_ - who earns what

The value of a video - in terms of direct revenue from media sales, pay-per-view or other income - is not generated by its producer only. Other contributors to the network are essential to make any value transaction in the first place. Among these contributors are curators that rate and tag videos and flag illegal or fraudulent content, the creators of the platform itself, video makers that choose not to monetize their work (but do help attract audience), and, essentially, the viewers that share and comment videos. These contributions are not directly monetizable, but do add to the network value that revenue-generating creators exploit.

It is not only fair, but also necessary for the functioning of the system, that these contributions are remunerated. For this reason, Paratii introduces a *redistribution pool* - an account funded from the newly minted PTI together with an* onus* cut on direct (pay-per-view or advertising) revenue. The *onus* can be significantly lower than typical intermediaries’ fees taken by centralized platforms (e.g. Youtube’s average 45% cut on ads revenue) due to the fact that that no stockholders need to be compensated, or an entity needs

For video makers, the effective *onus* will generally be lower than the nominal* onus*, because a part of the *redistribution pool* is allocated back to video makers themselves on the basis of the quality of their content *(see section III.c)*. The effective *onus* can even be negative, making a creator earn more than the direct revenue generated from her content. 

**IMAGE 4 HERE**
*A high-level overview of Paratii’s value redistribution engine*

In this distribution scheme, the earnings of a video maker consist partly of a percentage of the direct sales of advertisements and pay-per-view, and partly of the *bonus*. In addition, the *communication market*, described below, offers creators the possibility to raise (or lower) the amount they make on each video by sharing its revenue with the audience of that video. 

The chart below simulates what the creator of *Video Z* could earn from a little more than 54.000 views, in a day on which all videos in Paratii generated a sum of 236.706 views, and the total advertising plus pay-per-view revenue of Paratii in the past 24 hours amounted to 1.510 PTI. In this example, a popular video is seen by close to a fourth of the daily audience, and rated 5% better than the average ratings of the last 24 hours. It allows its creator to earn more than 100% of the revenue directly generated by it, while still sharing 20% of the income with his audience. His netto revenue of 510,69 PTI is equivalent to a fiat amount that, of course, depends on PTIs unpredictable market value at the time. 

**IMAGE 5 (CHART) HERE**

For the sake of comparison, an equivalent amount of audience (54.682 views) for an ad-monetized video on the world’s biggest video streaming platform currently yields an estimate of U$164,04 (presuming a generous RPM of U$3,00). Therefore, in the hypothetical scenario, any market price above U$0,32 for 1 PTI would already make the producer earn more than in this centralized alternative. If we assume that CPMs being paid by advertisers in Paratii are on average just slightly cheaper than those paid on legacy media buying platforms (U$7, in comparison to a market average of around U$7,60), the relation between Video Z’s ad sales and views give us an equivalent of U$382,77 dollars spent in the form of PTIs, and yielding earnings more than twice higher than the same audience would have amounted to in the fictional abovementioned traditional platform.

_Worth noting: the market prices simulated are for illustrative purposes only. They are not to be taken as promises or predictions. Every content producer is encouraged to perform his own due diligence and hypothetical experiments before joining Paratii._

### VI.c. A _communication_ _market_

Digital replicability has made information so cheap that the only limiting factor in its consumption is the attention space of the users. In the words of economist Herbert Alexander Simon, "in an information-rich world, the wealth of information means a dearth of something else: a scarcity of whatever it is that information consumes" [35].

However, this reversal of the "information economy" - from scarcity of information to scarcity of attention - has hardly yet evolved into a corresponding market of attention. Users - attention spenders - have few or no tools to manage their scarce resources in a conscious way.  There are no established markets where attention providers (such as viewers of videos) and attention seekers (advertisers and video makers) can meet and negotiate.

As a consequence, attention on the internet is often grossly undervalued. For example, a CPM of $7,60 for a video of one minute translates the activity of watching a single traditional VoD advertisement to just a little more than 7 thousandths of a dollar (about $0,45 an hour) of revenue for the platform, which is only partially redistributed to producers. In comparison to the total time spent by viewers on the platform, this value is shockingly low.

This disparity is highlighted by data of other distribution channels for video. Illustratively, TV shows average a U$15,00 to U$25,00 cost for a 30 second commercial per thousand people in the US [36] - between U$1,80 and $3,00 earned by the channel for the attention of one person, per one hour.

Blockchain technologies may offer the possibility to create a proper attention market  through tokenization and smart contracts. According to classical economic theory [37], externalities’ internalization and formalization of nonmarket economic interactions can occur when negotiation costs no longer exceed the value brought by such interactions. Through tokenization, trustless micropayments and blockchain-based trade assistants, such negotiation costs can be minimized. This is not a novel observation: tokenizing the attention economy is a focus for several ongoing projects in the field (Synereo [38], Akasha [39] and Steem [40] are a few, besides the previously mentioned Basic Attention Token).

Properly valuing attention as the scarce resource that it has become reverses the economic power equation. By paying in attention, every buyer is empowered, because attention is scarce whereas video streaming is relatively cheap. Redistribution is truly meritocratic and does not depend on budget constraints that can distort the real utility exchange in ordinary markets: there are no attention capitalists, at the same time there are no attention poor.

The starting point for Paratii’s *communication market* is that creators can decide to share any percentage of the revenue they earn with their audience. This is a way for a producer to promote content without having to pay for it upfront. Instead of sharing revenue, the creator of a video also has the option to ask for a contribution from the viewer, effectively offering the content on a pay-per-view basis. 

User experience to convey this flexibility must be minimal and seamless. By clicking play, any user accepts the split which can be expressed by a single rounded number added to or subtracted from her balance. Content producers, on the other hand, simply have to decide how much revenue they want to share (up to 100%) or how they want to price a specific video at the moment of each upload. The middleground, considering the range of options available, is neither sharing nor charging, like in traditional AVOD models.

As shown above, the amounts that viewers will receive per view are quite small  - and we do not expect them to make decisions upon these values. Instead, to viewers, the market is presented as a place to exchange attention for content. By way of illustration, to watch a video that charges a 50% percent premium (on top of what it makes from every view from advertisements or other revenue source), a viewer can "pay" by watching two equivalent videos that are offering to share 25% of their proceedings, hence gaining access to a premium video with the PTI earned by watching other videos.

The goal is to make this market as rich as possible, offering video makers and viewers a large range of options for negotiation in the form of different types of contracts through which users barter attention or PTI for content.

* *Bundles*: viewers pay for content on a per-view basis, but earn increasing discounts the larger the bundle they acquire from the same producer or channel (think a soap opera).

* *Subscriptions* are like bundles, but user discounts comes due to his commitment along time (a month, a week, a year), and not the amount of content sought.

* Pay-per-comment: users are faced with a paywall before being able to natively join the discussion on certain video.

* *Droit de suite*: viewers are invited to stake and vest tokens at a *content contract *associated to a video, playlist or channel, in order to receive back proportionally to the future earning of that *content contract*.

The *communication market* reveals users’ opinions about the quality of content in the platform: they will be willing to pay more for videos they like, and less for videos they don’t. It can be seen as way of rewarding the quality of videos that is complementary to the *Proof of Quality* *(see section III.c) *approach. Thanks to the *communication market*, creators (also those who decide not to show ads) can earn proportionally to the willingness to pay attention for their works. 

In the mid-long term, creators can compete for the "recommendation space" of users, inciting them to be conscious regarding the value of their own attention and the attention they potentially command. An illustrative approach is that of implementing* shelves*: a limited number of slots for videos, in a prominent feature of users’ social profile, available for a certain period of time. Videos displayed on* shelves* are always free for friends to watch, but only to friends, internalizing an attention value that belongs to others but is earned by some users as they attract the attention of friends. Requiring viewers to promote a specific video by *shelving it* instead of (or in addition to) asking for a monetary compensation allows for creators to alternatively promote themselves in the *communication market*, while also constituting a parallel recommendation system that enriches interactions. If in an ordinary social network we can only share content (and an engine decides which of our shares are seen by our friends), the scarcity of *shelves* can incentivize users to not only *share*, but also *exchange* content with their peers. Realistically, though, since the primary input comes from the creator, people might not coordinate in order to market videos around, but rather be under dynamics of opinion leadership, laying the ground for a simple peer-to-peer incentivized recommendation engine. Overcoming the paradigm of echo chambers and achieving a subtle balance with serendipity is an ultimate goal for such mechanisms, but they are traditionally powered by algorithms alone. Userfeeds [41] is a tokenized content-ranking system that aims to unbundle recommendation engines from the platforms they run on, allowing for a more liquid and inclusive approach - exploring its integration is on the roadmap for Paratii.

Efficiency of the *communication market* translates not only as complete internalization of the attention value of the network, but also as clear, fair and diverse set of options for network participants to consciously or unconsciously allocate this attention in a non-ordinary way (a way that’s *transformative*).

# **VII. Implementation**

Paratii’s general architecture follows a hub-and-spoke model, the hub being the Paratii core, which manages the PTI tokens and defines a basic economic ruleset. The core is implemented as a set of smart contracts that run in a trustless way. They will be implemented as Smart Contracts on the Ethereum blockchain complemented by suitable technologies that address efficiency issues: communication and micro-transactions will be implement using state channels;  data will references in smart contracts but can be stored off-chain in IPFS or Swarm.

This core engine has of a number of sub-components:

* An ERC20 T*oken contract* that tracks the PTI balances of the individual users and allows them to transfer these tokens, and that allows the issuance contract to distribute new tokens.

* A *Content contract* that creators interface with to set up parameters when they upload monetised videos, and that updates such information by interacting with child contracts associated to specific content consequently deployed by creators.

* An *Issuance contract* that, based on output of the *Proof of Quality contract* and the *Proof of View contract*, allocates PTI to a redistribution pool, and then to content producers and other contributors.

* A *Reputation contract* that updates the reputation of contributors and interfaces with the current iteration of *Proof of Quality* to weight the outcome of user-performed curation contributions.

* A *Proof of Quality* contract that computes evaluations from users with respect to the "quality" of the videos they rate, flag or curate.

* A *Proof of View* contract that records traffic history via bundles of aggregate data referencing *content (child) contracts* or orders they correspond to in the orderbook of impressions.

* An off chain orderbook for advertisements that’s accessible through an Open RTB API and eventually settles transactions on the *Proof of View contract*.

The Paratii core implements essential logic behind the player, but the ecosystem is not complete without a number of specialised modules and services that connect to user-facing web applications, such as an in-browser video player and its light wallet, or interfaces to external services that execute file sharing, user validation or additional activities.

For general users, the main point of access to the core is the Paratii video player, an embeddable javascript application that lives in third-party web sites or native Ethereum dApps. The player incorporates a light wallet that holds PTI and potentially other tokens, besides functioning as an in-browser node in the underlying DSN. It is designed to offer a browsing experience for average users and producers that is as fluid, streamlined and consistent with the decentralised nature of web 3.0 as possible. In addition, other services must be implemented or integrated. For example, the Paratii ecosystem includes a supply side platform for advertisers: a gateway providing an API that implements the OpenRTB standard and shows real-time information about impressions’ auctions and bid requests setups, providing seamless integration with demand-side platforms.

## VII.a. Road Map

The requirements on data storage, speed, cost, and transaction throughput to support a large scale video distribution service are considerable. Paratii is very much grounded in the Ethereum blockchain and its associated ecosystem of applications and tools, but, as of writing, it is not yet mature enough to support such load. Our approach to this bootstrapping problem is to start with an implementation of the core components that is mostly centralized. Subsequently, as the ecosystem matures, these components will be ported to a trustless version (fully implemented on the blockchain).

Incentivized file sharing and distribution, a central function for Paratii, also is not production-ready at the current time - although there’s a great number of promising players on the market: Swarm, IPFS, Storj [42], Siacoin [43] or maidsafe [44] are some examples. In the long term, Swarm seems to be the DSN that is most suitable for Paratii: native Ethereum integration means that it is possible to benefit from network effects; and there’ll be complementary services to take advantage of, such as secure PSS-messaging [45], plausible deniability and adaptive streaming through the Swatch subprotocol [46]. However, to be able to function as a node in the browser, Swarm needs to be ported to javascript, as well as support WebRTC as a communication layer. Thus, in its first incarnation, Paratii will make use of non-incentivized peer-to-peer technologies such as Webtorrent.	

* **Indigo Release (Minimal Viable Product)** - The MVP consists of rudimentary versions of various essential components of the core system, and offers a first version of the main user-facing application: an in-browser video player with basic functionality for content discovery, account management and content rating systems. The player supports streaming over webtorrent and centralized technologies, and has an integrated light wallet for sending and receiving PTI and ETH.  Many functions will at this stage be implemented in a centralized fashion, although all monetary transactions will be settled on a (private testnet) Ethereum blockchain. This first release will serve mostly as proof of concept. Its scope is both to demonstrate the technical viability of the project as well as to serve as a first model for the end-user experience. 

* **Atlantic Release (Alpha Version)** - Atlantic is a release focused in improvements for the content producer, and includes interaction with the PTI token on the main net. The option of monetising content by pay-per-view will be made available and inaugurate Paratii’s *communication market*. This release will provide testing grounds for the basic economic mechanisms behind our system and aims to demonstrate its viability. IPFS’ libp2p integration will provide Paratii users with a javascript node that runs in the browser and fetches content from both temporary nodes and non-browser regular clients. Technically, the Paratii framework at this stage is still only partially decentralized: many functions, such as the validation of views, will be implemented in a classical, centralized manner. Bootstrapping is a key concern, and Paratii counts on the participation of early content providers with an already established audience on other platforms - content uploading directly through the embeddable player will be permitted from the Atlantic release on, first for selected producers, then to the general public. Public gateways will be established to help support the network’s expected load and welcome anyone wanting to participate in the ecosystem, even without registering first. This release will also bring a first version of the *Proof of Quality* (PoQ) mechanism implementation.

* **Antarctic Release (Beta Version)** - Antarctic’s purpose is to strengthen Paratii’s peer to peer media stack and increase performance, in order to offer more reliability to end-users as well as start welcoming *attention marketers*. If devp2p has added support to WebRTC connections at this stage, the release will include a light-swarm javascript implementation. Further iterations of our reputation system will have undergone testing and the choice of which one to follow on with will be handed to the user base. The reputation logic will be ported to a family of smart contracts. The release will bring an off chain orderbook, ideally built upon the Raiden Network, for *attention marketers* to bid on bundles of media space.

* **Atlantis Release (Full-fledged version)** - Atlantis is a release focused on scaling the *communication market* and further decentralizing the economic gears of the system. A first version of a formal governance mechanism will be implemented as a notification system for registered users to decide on minor upgrades and tweaking of internal parameters. An API implementing the OpenRTB standard will allow for seamless integration with DSPs and increase the competitivity of Paratii to *attention marketers*. Attention payers (or users) will be compensated with the deployment of an evolving recommendation system that will compare (and potentially mix) open algorithms developed "in-house" to components of third-party pluggable engines. Automatized “bounty hunting” is to be put in place and ever refining contribution rewards’ parameters should keep the network’s growth traction.

### VII.b. Paratii Foundation and capital allocation

The Paratii Foundation, a nonprofit entity, will be responsible for the development of the Paratii ecosystem and software, up to the moment when both are fully autonomous. Its functions include the management of daily financial operations and digital asset management; recruitment and hiring of personnel or external contractors; auditing; legal and taxation research and operation; fostering the growth of a development community; running bug bounty programs; besides promoting and marketing Paratii. More broadly, the foundation will advocate for transparency, disintermediation and the power to transform individuals through visual storytelling. 

This entity will be managed as transparently and openly as possible. The foundation resorts to reputation-weighted voting as a decision making mechanism to resolve disputes and determine the use of funding resources, code-related decisions and other issues, whereas initial reputation allocation is centralized on the Initiators and will be further diluted among network participants *(see section VII.a)*; being spread out, in between, to up to a dozen producers, strategic partners and advisors.

As previously mentioned, 37% of the total supply of *paratii* will be pre mined. Close to half of these, 15%, will be allocated to a token distribution event whose details are out of the scope of this document. 10% is set aside for compensating early investors and initiators, the latter under a vesting schedule. And 12% will be released, also under a vesting schedule, to the Paratii Foundation. Both vesting schedules last for a year, and have three installments, equally distant from each other, to be released if respective milestones are met. The foundation’s share might be used for further token distribution events if deemed necessary. The remaining 63% of tokens are to be issued via a minting process to the network.

This project was conceived under the patronage of BossaNovaFilms, a Brazilian film and entertainment production company. Most of its design decisions were made by a small team of individuals, and development permitted by an angel investment quota, as well a round of private seed funding. Both combined represent one fourth (2,5%) of the "Initiator and Investors" share. The rest of this share, for now belonging to Initiators, can be voluntarily diluted by them for further fundraising with investors before any token distribution event.

Roughly, breakdown of initial token allocation is as illustrated by the chart above. Proceedings from any token distribution events will be allocated by the foundation as outlined below.

The proportions may vary in the occasion of further token distribution events (i.e. the foundation can offer a small percentage of its PTIs to fundraise development of a specific new feature or to acquire original content). Details on the foundation’s governance structure are to be publicized once it is established. It’s creation is driven by guiding principles of FOSS: incentivized collaboration among community members, meritocracy as the determinant factor for projects that rise and gather effort from the community, and free flows of information. As with the code, Paratii is committed to make most of its conversations open source.

### VII.c. Risks & Challenges

The trustless technology Paratii is based on may fail to fully deliver its promise. The Ethereum blockchain is already a sophisticated tool that handles billions of dollars in assets, but as a world computer it is still expensive, slow, and not adequate for large amounts of data, nor to natively perform complex computations. These issues are being worked on by the Ethereum community - keywords are "proof of stake", “sharding”, “raiden” and “plasma” - but it is difficult to say with confidence when they will be ready. Our implementation approach will be pragmatic: all technology that is not yet ready to be implemented on the blockchain will be developed using a classical centralized model, and then be ported to the Ethereum blockchain once it is mature enough. 

The success of the project depends on the availability of an incentivized decentralized storage network. As of writing, the Swarm native component of Ethereum is in a testing stage and does not have a working incentives layer yet. As already described, we may rely upon alternatives.

Privacy is another concern. It is inherent to the model here presented that users are identified (in the sense that all users have an account associated with their PTI), and that all "paid for" views of videos are eventually registered on the blockchain. This means that, even if users are pseudonymous, their viewing history is public. In centralized proprietary platforms, this kind of information is collected and either kept secret or, if made available to third parties, presumably “anonymized” as much as possible. This practice offers if not a guarantee, at least some degree of assurance to the user that the data about his behavior remains private.

Keeping secrets on the blockchain is difficult, but not impossible. ZCash is a Bitcoin-like payment system in which only "zero-knowledge-proofs" [47] of valid transactions are registered - the data about who owns how much is not itself available on the blockchain. Another interesting application in this sense comes from the hedge fund Numerai, which uses homomorphic encryption [48] to make proprietary financial data available to data scientists. The encryption preserves certain structural characteristics of the datasets, allowing algorithms to work on them even though not all of the raw information are available. Other systems specifically designed to hide information that identifies users may be used as well to secure privacy. 

Apart from the technological risks, there are also organizational ones. Creating and managing a complex and sophisticated organization as ambitious an autonomous entity is something that simply has never been successfully done yet. The experience of the few precedents is mixed: while what’s happened to "The DAO" (a decentralized investment fund on the Ethereum blockchain) was disappointing, to say the least [49], projects such as Bitcoin can arguably be seen as DAOs themselves, and provide examples where the model does work reasonably well. To mitigate this risk, we will develop Paratii incrementally, starting with an implementation that is mostly centralized and decentralizing its components in a piecemeal way. The roadmap below provides more details.

Finally, it is important to remember that, just like everything else in the field of cryptocurrencies, Paratii is first and foremost an experiment. However, it doesn’t harm to remind readers that, throughout human history, progress has always been driven experiments.

## **VIII. Conclusion**

This blueprint outlines our vision for a scalable, efficient and trustless video-on-demand distribution system. Its technology stack is modular, and built upon a decentralized network layer. The increased efficiency and fairness of this approach, in comparison to legacy systems, is one of the premises on which Paratii is founded. The other is that tokenizing value to incentivize early users is indeed a way out of the chicken-egg problem of network effect, which historically has been a growth impeditive for virtual networks.

Paratii’s design and roadmap aim to leverage existing open source frameworks as much as possible. A founding principle is that of never imposing a monetisation model on content producers, but to establish grounds on which socio-economic experiments can be safely played out instead, and successful models gradually made autonomous. 

In the mid term, even giants of the current siloed landscape of digital content distribution may see themselves under the threat of systems that achieve higher efficiency and profitability for value creators through disintermediation. There will probably emerge many - instead of a few monolithic - major decentralized alternatives to patch up broken parts of the online video distribution and monetisation value chain. Paratii aims to be close to the core of this ecosystem, providing seamless streaming and user experience - besides a fair economic game - to any user, in any corner of the web. 

## **IX. References**

1 - Statista. **Digital video advertising revenue worldwide from 2015 to 2021, by device (in million U.S. dollars)**. **2017**. URL: [www.statista.com/statistics/456747/digital-video-advertising-revenue-device-digital-market-outlook-worldwide](https://www.statista.com/statistics/456747/digital-video-advertising-revenue-device-digital-market-outlook-worldwide) 

2 - There are three main business models when it comes to video on demand services: advertising-supported video on demand (AVOD), where advertising is included as part of the content offered (i.e: Youtube); transactional video on demand (TVOD), on which content is payed for whenever it’s to be watched or rented (think Apple’s iTunes); and subscription video on demand (SVOD), on which a periodic fee is charged to allow retrieval from a catalog of content (for example, Netflix).

3 - WONG, Joon Ian. Quartz. **The internet has been quietly rewired, and video is the reason why. **2016. URL: [www.qz.com/742474/how-streaming-video-changed-the-shape-of-the-internet](http://www.qz.com/742474/how-streaming-video-changed-the-shape-of-the-internet) 

4 - EDDY, Nathan. **Google, Facebook Dominate Digital Ad Revenue.** 2016. URL: [www.eweek.com/small-business/google-facebook-dominate-digital-ad-revenue.html](http://www.eweek.com/small-business/google-facebook-dominate-digital-ad-revenue.html)

5 - BitTorrent. URL: [http://www.bitmedianetwork.com/](http://www.bitmedianetwork.com/)

6 - Statista.** Number of Netflix streaming subscribers worldwide from 3rd quarter 2011 to 3rd quarter 2016 (in millions**). URL: [www.statista.com/statistics/250934](http://www.statista.com/statistics/250934)

7 - Protocol Labs. **The JavaScript implementation of the libp2p networking stack. **2017. [https://github.com/libp2p/js-libp2p ](https://github.com/libp2p/js-libp2p)

8 -  [https://github.com/webrtc/samples](https://github.com/webrtc/samples)

9  -  MARCIEL, Miriam; CUEVAS, Ruben; BANCHS, Albert; GONZALEZ, Roberto; TRAVERSO, Stefano; AHMED, Mohamed; AZCORRA, Arturo. **Understanding the Detection of View Fraud in Video Content Portals**. 2015. URL: [http://www.recred.eu/sites/default/files/fake_views_imc15.pdf](http://www.recred.eu/sites/default/files/fake_views_imc15.pdf) 

10 - CHARLES, Ryan. **What is Yours?** 2017. URL: [https://stories.yours.org/what-is-yours-cefebc322ce3](https://stories.yours.org/what-is-yours-cefebc322ce3) 

11 - Brave Software. **Basic Attention Token (BAT) - Blockchain Based Digital Advertising. **2017. URL: [www.basicattentiontoken.org/BasicAttentionTokenWhitePaper-4.pdf](http://www.basicattentiontoken.org/BasicAttentionTokenWhitePaper-4.pdf) 

12 -  GOLDIN, Mike; SOLEIMANI, Ameen; YOUNG, James. **The AdChain Registry.** 2017. URL: [https://adtoken.com/white-paper.pdf/](https://adtoken.com/white-paper.pdf/)

13 - Protocol Labs. **Filecoin: a Decentralised Storage Network.** 2017. URL: [http://filecoin.io/filecoin.pdf](http://filecoin.io/filecoin.pdf)

14 - Storj cites costs of $0.015$/GB a month.

15 - @dantheman. **Brief Update on Reputation Score. **2016.** **[https://steemit.com/steemit/@dantheman/brief-update-on-reputation-score](https://steemit.com/steemit/@dantheman/brief-update-on-reputation-score)

16 - [https://stackoverflow.com/help/privileges](https://stackoverflow.com/help/privileges) 

17 - Backfeed. **Decentralized value distribution system to blockchain-based applications.** 2016. URL: [http://backfeed.cc/assets/docs/TechnicalSummary.pdf](http://backfeed.cc/assets/docs/TechnicalSummary.pdf) 

18 - IAB.** IAB Tech Lab Content Taxonomy.** 2015. URL: [www.iab.com/guidelines/iab-quality-assurance-guidelines-qag-taxonomy](http://www.iab.com/guidelines/iab-quality-assurance-guidelines-qag-taxonomy)

19 -  Paratii cannot literally "remove content": it is built on a peer-to-peer file sharing network that’s uncensorable by design. Paratii cannot force all peers in the system to erase all chunks associated with that file; what it can do instead is remove a title from its catalogue.

20 - DAUBENSCHUETZ, Tim; McMULLEN, Greg; SUN, Brett. **Coala IP Specs / Whitepaper.** 2017. URL: [https://github.com/COALAIP/specs](https://github.com/COALAIP/specs) 

21 - [https://github.com/ipld/specs/tree/master/ipld](https://github.com/ipld/specs/tree/master/ipld) 

22 -  PETKANICS, Doug; TANG, Eric. **Protocol and Economic Incentives For a Decentralized Live Video Streaming Network.** 2017. URL: [https://github.com/livepeer/wiki/blob/master/WHITEPAPER.md](https://github.com/livepeer/wiki/blob/master/WHITEPAPER.md) 

23 - [https://github.com/livepeer/lpms](https://github.com/livepeer/lpms) 

24 -   ANA and White Ops. **The Bot Baseline: Fraud in Digital Advertising**. 2014. URL: [http://www.ana.net/content/show/id/botfraud-2016](http://www.ana.net/content/show/id/botfraud-2016) 

25 -  VRANICA, S.. **A ‘Crisis’ in Online Ads: One-Third of Traffic Is Bogus.** 2014. URL: [www.online.wsj.com/news/articles/ SB10001424052702304026304579453253860786362](http://www.online.wsj.com/news/articles/)

26 - Some suppliers have tried to resolve these issues, only to notice that solving the problem makes their clicks, conversions and impressions drop below those of others who choose to turn a blind eye on fraudulent activity. What’s more, researchers at the IMDEA Networks Institute found fake views are not only sold to advertisers but also that "YouTube applies different penalization schemes to the fake views in the monetized and public view counter, with the former being much more permissive than the latter". Combating fraud is rendered even more difficult because any approach that puts a burden on the valuable customers in terms of friction or performance is generally avoided.

27 - HOHENBERGER, Susan; MYERS, Steven; PASS, Rafael; SHELAT, Abhi. 2016. **Anonize: A Large-Scale Anonymous Survey System.** URL: [https://anonize.org/assets/anonize-oak-camera.pdf](https://anonize.org/assets/anonize-oak-camera.pdf); [https://gitlab.com/abhvious/anonize2](https://gitlab.com/abhvious/anonize2) 

28 - The fact that much of our user behavior is registered on the blockchain, and thereby rendered public, presents some obvious privacy issues (*see section VII.c*).

29 - GOLDIN, Mike; SOLEIMANI, Ameen; YOUNG, James.**The AdChain Registry.** 2017. URL: [https://adtoken.com/white-paper.pdf](https://adtoken.com/white-paper.pdf) 

30 - See *"Basic Attention Metric"* @ [11]

31 - IAB. **OpenRTB Dynamic Native Ads API Specification. **2015. URL: [https://www.iab.net/media/file/OpenRTB-Native-Ads-Specification-1_0-Final.pdf](https://www.iab.net/media/file/OpenRTB-Native-Ads-Specification-1_0-Final.pdf); 

32 - IAB. **Digital Video Ad Serving Template (VAST) 4.0. **2016. URL: [https://www.iab.com/guidelines/digital-video-ad-serving-template-vast-4-0/](https://www.iab.com/guidelines/digital-video-ad-serving-template-vast-4-0/)

33 - [https://www.nyiax.com](https://www.nyiax.com) 

34 - [https://github.com/ethereum/EIPs/issues/20](https://github.com/ethereum/EIPs/issues/20)

35 -   Interestingly, one of the precursors of bitcoin, Adam Back’s hashcash, was invented exactly to address another problem of that is related to an abundance of communication with regard to the scarcity of attention: that of email spam. 

36 - It's hard to compare TV with the internet, since CPMs are different in digital, where audience measurement happens on an individual level, and TV, where the device (TV) is tracked, and attributed to the family, or the home. Some sources ([http://www.siteadwiki.com/2013/11/television-media-average-cpm-rate.html](http://www.siteadwiki.com/2013/11/television-media-average-cpm-rate.html)) postulate an ~U$40 average for a thousand homes, in the US, currently (for 30s). In Brazil, the value is lower (~U$8), but also varies drastically between open TV *vs.* cable TV. 

37 - DEMSETZ, Harold. **Toward a Theory of Property Rights.** The American Economic Review, Vol. 57, No. 2, Papers and Proceedings of the Seventy-ninth Annual Meeting of the American Economic Association. (May, 1967), pp. 347-359.

38 - KONFORTY, Don; ESTRADA, Daniel; MEREDITH, Lucius Gregory. **Synereo: The Decentralized and Distributed Social Network.** 2015. URL: [https://coss.io/documents/white-papers/synereo.pdf](https://coss.io/documents/white-papers/synereo.pdf) 

39 - [https://akasha.world](https://akasha.world) 

40 - LARIMER, Danie; SCOTT, Ned; ZAVGORODNEV, Valentine; JOHNSON, Benjamin; CALFEE,  James; VANDEBERG, Michae.  **Steem: an incentivized, blockchain-based social media platform.** 2016. URL: [https://steem.io/SteemWhitePaper.pdf](https://steem.io/SteemWhitePaper.pdf) 

41 - OLPINKSI, [Maciej](https://blog.userfeeds.io/@maciejolpinski?source=post_header_lockup). **On blockchains, ‘fake Trump news’ and uberization of Facebook. **2016. URL: www.blog.userfeeds.io/on-blockchains-fake-trump-news-and-uberization-of-facebook-b7a74fe5c371

42 - WILKINSON, Shawn; BOSHEVSKI, Tome; BRANDOFF,  Josh; PRESTWICH,  James; HALL, Gordon; GERBES, Patrick; HUTCHINS, Philip; POLLARD, Chris. **Storj - A Peer-to-Peer Cloud Storage Network. **2016. URL:  [https://storj.io/storj.pdf](https://storj.io/storj.pdf) 

43 -  VORICK, David; CHAMPINE, Luke. **Sia: Simple Decentralized Storage. **2014. URL: [www.sia.tech/whitepaper.pdf](https://www.sia.tech/whitepaper.pdf) 

44 -  [https://maidsafe.net/](https://maidsafe.net/)

45 - [https://gist.github.com/zelig/d52dab6a4509125f842bbd0dce1e9440](https://gist.github.com/zelig/d52dab6a4509125f842bbd0dce1e9440) 

46 - [https://gist.github.com/zelig/74b3486bcd5523a0b61e12d804d3c00d](https://gist.github.com/zelig/74b3486bcd5523a0b61e12d804d3c00d)

47 -  BEN-SASSON; Eli; CHIESA, Alessandro; GARMAN, Christina; GREEN, Matthew; MIERST, Ian; TROMER, Eran; VIRZA, Madars. **Zerocash: Decentralized Anonymous Payments from Bitcoin. **2014. URL: http://zerocash-project.org/media/pdf/zerocash-extended-20140518.pdf

48 - CRAIB, Richard. **Encrypted Data For Efficient Markets. **2016. URL: [https://medium.com/numerai/encrypted-data-for-efficient-markets-fffbe9743ba8#.cdj18pmng](https://medium.com/numerai/encrypted-data-for-efficient-markets-fffbe9743ba8#.cdj18pmng)

49 -  JENTZSCH, Christoph.**The History of The Dao and Lessons Learned.** 2016. URL: [https://blog.slock.it/the-history-of-the-dao-and-lessons-learned-d06740f8cfa5](https://blog.slock.it/the-history-of-the-dao-and-lessons-learned-d06740f8cfa5) 

