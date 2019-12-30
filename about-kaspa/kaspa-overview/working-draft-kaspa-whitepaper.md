# WORKING DRAFT: Kaspa Whitepaper

### Sound, expressive money

Bitcoin was the first decentralized, permissionless digital currency, due to its revolutionary use of proof of work and a blockchain ledger for consensus. Ethereum was an extension of this concept, its smart contracts providing programmability and expressiveness. However, Bitcoin suffers from increasing mining centralization, low transaction processing, slow transaction confirmation times, and low expressiveness, impeding its scalability and usability. Ethereum’s governance centralization and uncertainties, its coupling of computation and consensus, and its planned move to proof of stake hinder its sound money conviction and chokepoint its complete expressiveness.

Kaspa presents a needed new paradigm for sound and expressive money by revisiting core concepts of previous generation blockchains. The Kaspa community believes the core design of the proof of work "Nakamoto consensus" of Bitcoin is the best-proven foundation for a store of value currency. Thus, Kaspa starts with a highly scalable and decentralized data and consensus layer using the [PHANTOM](https://eprint.iacr.org/2018/104.pdf) consensus protocol, which generalizes Nakamoto's blockchain into a directed acyclic graph of blocks \(blockDAG\), and [optical proof of work](https://arxiv.org/pdf/1911.05193.pdf) \(OPoW\), which is a sustainable and decentralizing proof of work algorithm. Kaspa has a computation layer similar to [Arbitrum](https://www.usenix.org/node/217514), [TrueBit](https://people.cs.uchicago.edu/~teutsch/papers/truebit.pdf), and [Optimistic Rollups](https://medium.com/plasma-group/ethereum-smart-contracts-in-l2-optimistic-rollup-2c1cef2ec537) \(Ethereum scaling proposals\), which is decoupled from the base layer to allow for a fast and robust native currency while supporting upper-layer use cases, such as stable tokens and undercollateralized credit systems.

Notably, Kaspa diverges culturally from Bitcoin: the Bitcoin community promotes extreme self-sovereignty—censorship resistance, privacy, anonymity, avoidance of debt, and verification of all historical transactions across all users—at any computational and expressiveness cost. While Kaspa decouples financial applications from the money base, making it easy for users to reap any or all of these values, Kaspa optimizes for fast day-to-day operation of users and support for all kinds of financial products. Kaspa promotes sufficient self-sovereignty: we believe it is sufficient that the right to “be your own bank” exists, and that a small minority exercises it.

This whitepaper describes the core concepts that motivate the Kaspa project, and outlines the system’s technical architecture and use cases.

## 1. Core Concepts

Kaspa is centered on several core concepts. In describing these concepts, this section also introduces the components of Kaspa.

### 1.1. The Satoshi Nakamoto Narrative

\[1 paragraph intro to Bitcoin - explain nakamoto consensus, pow\]

The Nakamoto system was the first to achieve scalable, permissionless, and trustless consensus in a simple and provably secure way. Combining Nakamoto proof of work with public key cryptography to prove ownership of coins, Bitcoin became the first decentralized cryptocurrency, and remains the gold standard of sound digital money.

To this day, Bitcoin uniquely enjoys the positioning as an _alternative money base_.

Indeed, we believe that a system’s ability to gain trust \(and wealth\) as a store of value alternative to fiat, gold, and real estate is associated with having the following Bitcoin-like characteristics:

* Proof of work consensus
* Thin design, isolation, predictability, determinism \(i.e., a UTXO model\)
* Fixed monetary policy
* Lack of a formal governing organization

In these characteristics, Kaspa follows the Bitcoin tradition.

#### 1.1.1. Alternative Consensus Tradeoffs

Alternative consensus protocols for cryptocurrencies, notably, proof of stake, are still highly experimental with untested security properties.

\[incomplete\]

Nakamoto consensus is the most robust and best-proven consensus system, and so Kaspa builds on its core design.

### 1.2. Mining Decentralization

While Nakamoto consensus is the best-proven and most trusted foundation for a store of value cryptocurrency, Bitcoin’s increasing mining centralization is increasingly undermining its soundness.

Bitcoin’s one-block-per-ten-minutes block creation rate, while a crucial compromise between security and speed within the Nakamoto paradigm, hurts mining decentralization: as the total Bitcoin network hashrate grows astronomically and the block rate remains constant, small mining entities weaken and must join larger and larger mining pools to eke out a regular income and remain viable.

Alternative projects’ attempts to decentralize mining by reverting to commodity mining hardware or ASIC-resistant algorithms jeopardize security: ASICs, specialty mining hardware, financially align miners with the success of the system. However, current ASIC mining is dominated by operational expenses \(OpEx\), making it feasible only under vast economies of scale, aided by deals with governments and power companies, leading to dangerous levels of centralization. Therefore, systems designed for specialty mining and incentivize decentralization at scale are much needed.

Kaspa is better designed for mining decentralization than existing cryptocurrencies in three aspects: more decentralized consensus, more decentralized proof of work mining, and more decentralized money distribution.

#### 1.2.1. Decentralized Consensus

Kaspa uses PHANTOM consensus, which is a blockDAG consensus protocol that generalizes over Nakamoto consensus. PHANTOM, by including all “orphan” blocks in the directed acyclic graph \(DAG\) ledger and ordering them in such a way to fairly extract transaction consistency, solves blockchain’s traditional orphan rate problem, and so can achieve orders-of-magnitude faster and bigger blocks than can Nakamoto consensus.\* This greatly reduces the variance of mining income, thus increasing the viability of smaller mining entities.\*\*

\*For an introduction to blockDAGs and the orphan rate problem, see [this blog post](https://blog.daglabs.com/an-introduction-to-the-blockdag-paradigm-50027f44facb).

\*\*BlockDAG protocols like [SPECTRE](https://eprint.iacr.org/2016/1159.pdf) and PHANTOM allow an arbitrary block rate and size increase without compromising security, but, to avoid network congestion, the increase must be limited to what the nodes can handle. PHANTOM can conservatively achieve 10 1MB blocks per second in a typical network, sped up from Bitcoin's 1 block per 10 minutes. This speedup allows a miner with one 16 TH/s DragonMint ASIC in a 100M TH/s network to increase his block creation expectation from 1 block in every 120 years to 1 block a week.

#### 1.2.2. Decentralized Proof of Work

Kaspa uses optical proof of work \(OPoW\), a proof of work algorithm that is secured with a Bitcoin-like SHA hash function, but designed for mining with ultra energy efficient photonic chips, which require high capital expenses \(CapEx\) and low operational expenses \(OpEx\). This shift from the traditional high OpEx mining model to a high CapEx low OpEx model enables more geographical decentralization of mining, since mining is no longer heavily advantaged by energy economies of scale. Furthermore, small players will not suffer losses from ongoing OpEx; they will simply have a longer return on investment period.

#### 1.2.3. Decentralized Money Distribution

Kaspa uses hashrate-pegged inflation \(HAPI\) **\[tentative\]**, a novel coin issuance model that decreases inflation as network hashrate increases. This yields an interesting effect: by increasing one’s own hashrate, one decreases the marginal profits on one’s existing mining machines. This effect exists in Bitcoin as well, but is enhanced by HAPI. It can be shown that a mining entity is disincentivized to increase their hashrate earlier \(i.e., in smaller hashrates\) than it would be in a Bitcoin-like minting setup.

Thus, Kaspa’s decentralized money distribution, along with its decentralized proof of work and consensus, contribute to its mining decentralization, a key property of a store of value cryptocurrency.

### 1.3. Scalability

Bitcoin’s one-block-per-ten-minutes block rate not only hurts mining decentralization, but also greatly limits its transaction processing speed and confirmation times\*, highlighting that it is a naive, version one protocol. As regularly cited, PayPal and Visa’s transaction volumes eclipse Bitcoin’s by many orders of magnitude, and Bitcoin’s default six-block confirmation time contribute to its infeasibility as a widespread medium of exchange. Further, Bitcoin’s “everyone should be their own bank” ethos, which encourages the proliferation of full nodes that verify all historical transactions, limits its throughput and speed of syncing new nodes. These are part of Bitcoin’s traditional scaling problem, its inability to handle an increasing amount of work or accommodate an increasing number of participants, compared to centralized transaction systems.

Many Bitcoin proponents argue that Bitcoin sacrifices computational scalability for a greater good—social scalability—as Nick Szabo defines in [this blog post](https://unenumerated.blogspot.com/2017/02/money-blockchains-and-social-scalability.html):

> “Social scalability is the ability of an institution—a relationship or shared endeavor, in which multiple people repeatedly participate, and featuring customs, rules, or other features which constrain or motivate participants’ behaviors—to overcome shortcomings in human minds and in the motivating or constraining aspects of said institution that limit who or how many can successfully participate.”

Indeed, Satoshi’s innovation, while it “offends the sensibilities of resource-conscious and performance-measure-maximizing engineers and businessmen alike”, minimizes trust and vulnerability, arguably increasing the number and variety of people who can successfully participate in the system. Applied to the already socially scalable performance of money, it enables a greater variety of people to participate in exchange without the immense human and cognitive capital of accounting, legal, and security controls—it allows a global range of people to coordinate without global knowledge—socially scaling Bitcoin leagues over PayPal and Visa.

\[incomplete\]

Kaspa preserves the social scalability of Bitcoin while aspiring to high computational scalability. As previously mentioned, Kaspa’s base-layer PHANTOM can achieve an orders-of-magnitude higher block rate over Bitcoin. Arguably, the faster and more efficient consensus of blockDAGs results in faster confirmation times. Kaspa also eases computational scalability by having users “opt in” to the extreme, but computationally burdensome level of self-sovereignty that Szabo claims facilitates social scalability.

\*For an introduction to and comparison of confirmation times in Bitcoin and blockDAGs, see [this blog post](https://blog.daglabs.com/confirmation-times-in-spectre-7f68fec0d997).

#### 1.3.1. Fast Confirmation Times

The confirmation time of a transaction is the time it takes for the transaction to become effectively irreversible. PHANTOM arguably achieves fast confirmation times since it has a high block rate, thus fast one-block confirmations; and blockDAG reorgs only affect conflicting transactions \(whereas blockchain reorgs affect whole forks that contain conflicting transactions\), thus nodes can adjust their confirmation times even further down depending on the presence of an attacker, as described in [this blog post](https://blog.daglabs.com/confirmation-times-in-spectre-7f68fec0d997).

However, many argue that the security budget \(i.e. the total miner revenue of block rewards and transaction fees, which, in proof-of-work blockchains, is a function of the network hashrate\) defines the cost of attack, so confirmation times are purely a function of the security budget regardless of efficient consensus.

A counterargument is that in an ASIC environment, the cost of attack is not captured entirely by the security budget. It needs to take into account the capital expense of the attackers’ ASICs, the long term depreciation of the ASICs due to the attack, the total amount of mining equipment available, how specialized the ASICs are to the coin, etc. In other words, miner “revenue” includes its ability to mine in the future and on other blockchains. Therefore, it is not clear that network hashrate alone determines ease or cost of attack; context is necessary.

Regardless, having a fast “first confirmation” is in itself a highly useful improvement, particularly for use cases where merchants are concerned with the time to the first block, or confirmation, but not to irreversibility.

#### 1.3.2. Vis-À-Vis Layer 2 Scaling Solutions

Using PHANTOM to achieve fast confirmations is a much simpler solution than Bitcoin’s layer 2 Lightning Network solution, which comes with a list of challenges: its complex UX, its need for trusted and available watch towers, its unfitness for large payments, its centralization dynamics, etc.

\[incomplete\]

#### 1.3.3. High Throughput

An improvement on confirmation speed entails an improvement on throughput. PHANTOM’s high block rate allows Kaspa to scale in throughput orders of magnitude over Bitcoin.\* Moreover, in addition to Bitcoin’s low block rate, Bitcoin’s cultural norm of "full validation as a necessary condition for self sovereignty" is a scalability bottleneck.

The Bitcoin community encourages the proliferation of full nodes, each of which, by the community’s definition, downloads and verifies all historical transactions in an initial blockchain download \(IBD\), whether this transaction history is kept after that \(as in archival full nodes\) or not \(as in pruned full nodes\). This is, the norm dictates, a prerequisite to full self-sovereignty. Consequently, to ensure fast syncing of new full nodes, transaction throughput must be suppressed \(and vice versa—higher throughput entails slower syncing of new nodes\).

We argue that IBDs should not be a default practice for new nodes, and that it is not necessary for a meaningful state of self-sovereignty. In general, non-altruistic users are only concerned about the security of their own transactions and unexpected inflation, and not about the security of other transactions in and of itself. Verifying only the present UTXO set\* is necessary and sufficient for addressing these concerns: recent invalid transactions are likely to break consensus, to cause a split or reorg the state, which is bad for all users, especially for those who transacted recently. In contrast, most cases of historical corruption do not affect current users \(e.g., if such corruption happened past a finality window\).

Users do care about having the option to verify all historical transactions, and some will exercise this option, and throughput should still be limited such that this is feasible for a philanthropic few—but this should not be imposed upon all. Kaspa instead optimizes for sufficient self-sovereignty, fast day-to-day operation, high throughput, and fast syncing, through the above-described lightweight trust model.

In sum, Kaspa’s underlying blockDAG contributes not only to mining decentralization, but also to speed and throughput. Furthermore, Kaspa opts for a trust model that preserves sufficient social scalability and self-sovereignty while optimizing for high computational scalability.

\*The question of how miners select and are incentivized to select transactions that avoid throughput-lowering collisions are addressed in [this blog post](https://blog.daglabs.com/transaction-selection-games-in-blockdags-602177f0f726).

\*\*In non-Bitcoin communities, e.g., Ethereum, it is common to refer to nodes that are validating present transactions as full nodes even if they didn't validate the history upon joining. We opt for this definition of “full”.

### 1.4. Privacy

\[incomplete\]

### 1.5. Expressive Money

Application environments for decentralized, expressive money are highly useful and demanded. Decentralized financial products should be built on top of sound money bases, and vice versa—to contribute to the “DeFi” landscape, a decentralized, scalable, and privacy-oriented currency should serve as a railing for financial products.

Bitcoin aimed to be simply a decentralized monetary system, therefore optimizing its consensus base for security, deliberately limiting the expressiveness of the system by restricting the set of allowable state transitions. Thus, Bitcoin is not conveniently designed for supporting financial products; it is primarily used as a store of value.

Ethereum aimed to support decentralized financial applications, therefore optimizing for expressiveness, allowing developers to encode any logic into the Ethereum Virtual Machine’s state transition. However, Ethereum is not a sound money platform, for reasons including:

* Ethereum’s native currency minting schedule is tweaked by developers.
* Ethereum lacks a scalable data layer\*, and Ethereum layer 2 scaling constructions such as STARKs require larger data throughput on-chain.
* The planned redesign of the system \(i.e., Ethereum 2.0\) has prompted much community uncertainty.
* Ethereum plans to move to proof of stake consensus, which is yet to be proven as a secure platform and one that people are willing to adopt for store of value.

These challenges prevent Ethereum from serving as a proper financial railing.

The fundamental challenge is the second bullet point, Ethereum’s architectural coupling of the application/computation layer with the base layer, which necessitates running each step of the EVM inside the base consensus. The Ethereum community is fully aware of the problems this causes, from state bloat to gas pricing to hard-to-manage dependencies.

\*Ethereum founder Vitalik Buterin expands on this need for a scalable data layer in [this post](https://ethresear.ch/t/bitcoin-cash-a-short-term-data-availability-layer-for-ethereum/5735).

#### 1.5.1. Decoupled Layers

Kaspa attempts to solve Ethereum’s architectural challenge by decoupling its system into two fundamental layers: a base layer and a computation layer. The base layer is a sound money layer that maintains global consensus and provides reorg resistance, proof of publication, and data availability. In Kaspa, this layer uses PHANTOM consensus, which is, as described above, a Nakamoto-like blockDAG protocol. The computation layer is a smart contract system that uses the payload data of base-layer transactions to record function calls to the smart contract virtual machine. The VM state is defined deterministically by the order of these function calls. Thus, every interested party can compute the VM state locally and is guaranteed that all other clients that read the VM agree on its state \(modulo the waiting time for sufficient irreversibility as guaranteed by the base layer’s reorg resistance\). Uninterested parties simply treat it as payload data and ignore its meaning; miners and full nodes on the base layer do make sure the data has been published \(proof of publication\). Moreover, different use cases can live separately in different VM instances—hence, we call these VM instances “silos”.

The concept of using the payload data of base transactions to implement expressive contracts is not new; Colored Coins and Counterparty have previously tried it with Bitcoin. This achieves the goal of an expressive computational layer, but not the goal of expressive, sound money, which requires that the contracts interact bidirectionally with the base layer money. Bitcoin could not have served as a trustless financial railing, since the contracts could only read, and not write, to the Bitcoin blockchain. Kaspa, on the other hand, is equipped with a two-way peg between the base and computation layer, allowing a bidirectional relationship.

Additionally, Bitcoin suffers from high confirmation times and is not natively built to support these use cases \(i.e., using payload data is not convenient in the Bitcoin client\); Bitcoin developers often fought against “parasite protocols” like Colored Coins, claiming that these harm the system by occupying block space for other-than-Bitcoin use cases. Thus, we need a way to connect a computation layer to a scalable base consensus layer without requiring that base consensus participants verify all computation and maintain the VM state. To this end, Kaspa is designed such that the base consensus is aware of the VM instruction set and runs only an individual instruction upon a dispute; in particular, the base consensus is not required to maintain the global state. Furthermore, disputes are highly costly to attackers, who must deposit stake in order to initiate a dispute. Our solution is highly inspired by Offchain Labs’ solution, Arbitrum, which is itself inspired by Truebit and is similar to Plasma Group’s Optimistic Rollups.

In short, Kaspa aims to make up Bitcoin’s expressiveness shortcomings and Ethereum’s soundness shortcomings, contributing to the DeFi landscape, by providing a decoupled but two-way-pegged stack of a base consensus layer and a smart contract environment computation layer.

### 1.6. Decentralized Credit

The DeFi landscape has not reached its full potential in expressiveness—it currently lacks an effective platform for undercollateralized credit.

There are three apparent reasons for this:

1. Anonymous DeFi applications are currently overcollateralized as a hedge against what happened in the many initial coin offering \(ICO\) scams of 2017, in which companies ran off with patrons’ money.
2. Bitcoin’s culture dictates that everything should be overcollateralized, that we should avoid all debt, financial instruments, physical assets, and anything that can introduce counterparty risk, tying into the extreme self-sovereignty norm. Thus, there are no financial applications built on top of Bitcoin, let alone debt-based financial applications, because of not only infrastructural chokepoints as explained in the previous section, but also cultural chokepoints.
3. Many argue that a debt-based system needs a global source for credit scoring, which does not exist.

Kaspa aims to address these issues and diverge from crypto’s overcollateralized economy by providing a third, topmost layer in its stack that serves as an environment for sufficiently self-sovereign, locally-rooted, decentralized credit, reputation, and trust systems. These systems are [Lightning Network](https://lightning.network/)-like; although Kaspa obviates the need for layer 2 solutions for fast payments, we need Lightning’s concept of off-blockchain networks and crawling user graphs for extended expressivity and complexity, using the blockchain only as an arbiter. Diving deeper into the graph theory of Lightning, we arrive at the original "Web of Trust" concept.

#### 1.6.1. Web of Trust

Kaspa’s third layer aims to be an environment to bootstrap webs of trust. “Web of Trust” originated from the Pretty Good Privacy \(PGP\) encryption program as a method for peer-to-peer public key verification—direct trust relationships established between friends or at key-signing parties would digitally chain, interweave, and cumulate into larger webs, or graphs, of trust, enabling well-informed trust paths among complete strangers.

Importantly, each participant's view of the PGP web of trust, i.e., his trust assignment to other participants, is local and independent. The local context that informs each participant's social trust graph differs among everyone; there is no need for agreement on global trust values. We view this as a form of logical decentralization, which Vitalik Buterin defines in [a blog post](https://medium.com/@VitalikButerin/the-meaning-of-decentralization-a0c92b76a274) as follows:

> “Logical \(de\)centralization—does the interface and data structures that the system presents and maintains look more like a single monolithic object, or an amorphous swarm? One simple heuristic is: if you cut the system in half, including both providers and users, will both halves continue to fully operate as independent units?”

According to this definition, a blockchain, with its single, monolithic ledger, is logically centralized, and the internet, a vast, amorphous entity, is not. This contrast, perhaps, highlights a missing link. By empowering hubs of local knowledge and verification, equipping the logical decentralization that has driven the internet’s success with innovations in cryptography and incentive design, the blockchain, crypto, and internet communities can enable and enhance a rich set of use cases, starting with decentralized reputation and credit systems, using Web of Trust à la PGP.

In Kaspa, webs of trust are logically decentralized but can piggyback on the global consensus of the base blockDAG layer for a sound financial railing, data availability, proof of publication, dispute resolution, and more.

Webs of trust end and start with local context. They are bootstrapped by direct, local trust relationships. In a credit web of trust, for instance, trust between a local grocery store and its customers, between an accountant and her clients, between business partners, can cascade into large, highly connected credit networks. Information accuracy increases with localization, and this is the key to solving the knowledge problem: how do we achieve economic efficiency when knowledge is highly distributed? Economist-philosopher Friedrich Hayek waxes prophetic in his 1945 essay [The Use of Knowledge in Society](http://bev.berkeley.edu/ipe/readings/The%20use%20of%20knowledge%20in%20society.pdf):

> “We cannot expect that this problem will be solved by first communicating all this knowledge to a central board which, after integrating all knowledge, issues its orders. We must solve it by some form of decentralization ...because only thus can we insure that the knowledge of the particular circumstances of time and place will be promptly used.”

Localization and logical decentralization, thus, allow webs of trust to efficiently capture real-world complexity. Locally bootstrapped webs of trust, leveraging real-world social relationships, also allow social reputation to replace stake as collateral. This moreover eases possibilities for debt forgiveness and locally restricts prejudice and nepotism.

The connectivity of webs of trust enables complex trust relationships among participants. Particularly, participants can assign trust or lend according to how much skin in the game their trustees have, for instance, lending .75x what a highly trusted peer lends in a particular contract. This further exploits the social graph structure, distributes risk, and enhances users' trust metrics.

In sum, Kaspa aims to provide, as its topmost layer, an environment for undercollateralized, decentralized credit, inspired by Lightning and web of trust systems. This environment is, furthermore, informed by concepts of logical decentralization, local knowledge, and skin in the game.

## 2. Architectural Details

This section expands on the Kaspa architecture. Kaspa is a symbiotic consensus stack that enables a robust, trustless store of value \(SoV\) currency, medium of exchange currencies \(MoEs\) and other programmable financial products, and rich webs of trust \(WoTs\) that provide real-world context. Particularly, this stack consists of:

* **The consensus layer** at the bottom, which uses [PHANTOM](https://eprint.iacr.org/2018/104.pdf), a PoW-based blockDAG consensus protocol that generalizes over Nakamoto consensus, optimized for sound and scalable SoV, payment, settlement, transfer of digital assets, and proof of publication. Our first client was developed as a fork of [btcd](https://github.com/btcsuite/btcd/blob/master/docs/README.md).
* **The smart contract layer** in the middle, which has a two-way peg to the base layer; events in this middle layer are ordered agnostically by the base layer and agreement on their interpretation is enforced by a committing-challenging on-chain mechanism; this layer is, essentially, a permissionless, stake-based version of [Arbitrum](https://www.usenix.org/node/217514). It is decoupled from the base layer, yet piggybacks on its security, and its internal “siloing” prevents shared-state bloat and design choke points.
* **The rich web of trust layer** on top, which is an environment for bootstrapping expressive, verifiable networks, or “webs of trust” \(WoTs\), in which interpretation and trustworthiness of data depend on users’ local worldviews, and behavior can be automated by context-, people-, and relationship-aware smart contracts.

These layers are described in detail in the following sections.

### 2.1. Consensus Layer

The consensus layer is a SoV environment, containing Kaspa transactions.

#### 2.1.1. PHANTOM

[PHANTOM](https://eprint.iacr.org/2018/104.pdf), developed by Yonatan Sompolinsky and Aviv Zohar at the Hebrew University of Jerusalem, is a proof-of-work based blockDAG consensus protocol, that generalizes over the Nakamoto consensus. Rather than pointing to the latest block, or the tip of the single longest chain, consequently forming a chain of blocks; in PHANTOM, blocks point to all tips of their miners' locally observed graph, even when there are double spends, creating a directed acyclic graph \(DAG\) of blocks—a blockDAG. Each node extracts transaction consistency from its locally observed blockDAG, by running the PHANTOM algorithm on it, which assigns a linear order to the blocks, and thus, a linear order to the transactions, from which double spends can be eliminated. As shown in the original paper, PHANTOM guarantees fast, probabilistic agreement among all nodes, despite any differences in their locally observed blockDAGs. By alleviating Bitcoin’s high-orphan-rate-induced scalability–⁠security tradeoff, PHANTOM achieves subsecond block times, fast confirmation times, high throughput, and—consequent to the higher granulation of block reward—higher mining decentralization, all without compromising consensus security; PHANTOM diminishes consensus security as a bottleneck for scalability, and by doing so also introduces resilience to [selfish mining](https://www.cs.cornell.edu/~ie53/publications/btcProcFC.pdf) schemes.

PHANTOM takes a blockDAG as input, and outputs an ordering of its blocks. It does so by identifying a cluster of well-connected \(blue\) blocks, and ordering the blockDAG in a way that favors them over less connected \(red\) blocks. The red blocks’ low connectivity in the DAG implies that, with high probability, they were produced by attacker nodes that withheld them, or deliberately referenced an outdated state of the DAG. This assumes the attackers’ do not control more than 50% of the hashrate.

**Terminology**

Borrowing from the PHANTOM whitepaper, let$$G = (C, E)$$be a DAG locally observed by a miner, where$$C$$denotes blocks, and$$E$$denotes edges, i.e. hash references to previous blocks. Let$$B$$be a block in$$C$$.

* $$past(B)$$is the set of blocks reachable from$$B$$\(blocks, that$$B$$directly or indirectly points to\).
* $$future(B)$$is the set of blocks, from which$$B$$is reachable \(blocks, that directly or indirectly point to$$B$$\).
* $$anticone(B)$$ is the set of blocks outside of the cone$$\{B, past(B), future(B)\}$$\(blocks that cannot reach$$B$$, nor be reached from$$B$$\). Due to the lack of global clock, it is unclear whether a block came before or after other blocks in its anticone.
* $$tips(G)$$ is the set of blocks, not referenced by any other blocks \(usually, the most recent blocks\).

![DAG concepts: past, future, cone, anticone, tips](../../.gitbook/assets/image%20%287%29.png)

**The PHANTOM Protocol**

Miners in PHANTOM are expected to reference, in their new blocks, all blocks in$$tips(G)$$, and publish their blocks as quickly as possible. Assuming honest miners control over 50% of the hashrate, intuitively, blocks created by them are more "well-connected" than blocks created by miners who do not follow the default policy. The PHANTOM authors expand on this as follows:

> Let$$D$$be an upper bound on the network’s propagation delay. If block$$B$$was mined by an honest miner at time$$t$$, then any block published before time$$t-D$$necessarily arrived at its miner before time$$t$$, and is therefore included in$$past (B )$$. Similarly, the honest miner will publish$$B$$immediately, and so$$B$$will be included in the past set of any block mined after time$$t+D$$. As a result, the set of honest blocks in $$B$$'s anticone is typically small, and consists only of blocks created in the interval$$[t-D, t+D]$$. The proof-of-work mechanism guarantees that the number of blocks created in an interval of length$$2·D$$is typically below some $$k$$.

In other words, due to the low assumed network propagation delay, which is approximated by a constant $$k$$_,_ if$$B$$is an honest block, then the honest blocks in$$B$$'s anticone are few and capped at $$k$$. PHANTOM captures the well-connected set of blocks in a DAG by defining a$$k$$-cluster, as follows:

> Given a DAG$$G = (C, E)$$, a subset$$S ⊆C$$is called a $$k$$-cluster, if $$∀B ∈S: |anticone(B) ∩ S | ≤ k$$

In other words, a$$k$$-cluster is a subset of blocks in a DAG such that each block in the subset has$$k$$or fewer of the other blocks in the subset in its anticone. In order to find the most well-connected group of blocks, PHANTOM solves the optimization problem of finding the largest possible $$k$$-cluster in a blockDAG. This problem is formalized as follows:

> **Maximum** $$k$$**-cluster SubDAG \(**$$MCS_k$$**\)  
> Input:** DAG $$G = (C, E)$$   
> **Output:** A subset $$S\text* ⊂ C$$ of maximum size, such that$$|anticone (B) ∩ S\text*| ≤ k$$ for all$$B ∈ S\text*$$

PHANTOM "colors" well-connected blocks in the$$k$$-cluster blue \(probably honest\), and other blocks red \(probably malicious\). We use an example of a 3-cluster from the paper to illustrate PHANTOM:

![A 3-cluster, a k-cluster with k=3](../../.gitbook/assets/image%20%284%29.png)

> It is easy to verify that each of these blue blocks has at most $$k=3$$ blue blocks in its anticone, and \(a bit less easy\) that this is the largest set with this property.

After solving$$MCS_k$$, PHANTOM topologically sorts the DAG, favoring blue blocks over red blocks, e.g. by adding a red block right before a blue block$$B$$only if it is in $$past(B)$$, resulting in a global block order from which a consistent transaction order can be extracted.

$$MCS_k$$, however, is [NP-hard](https://en.wikipedia.org/wiki/NP-hardness), and thus is not suitable for a growing blockDAG. Kaspa uses a greedy version of PHANTOM, called GHOSTDAG, that is possible to implement.

**The GHOSTDAG Protocol**

GHOSTDAG is a _greedy, recursive algorithm_ that approximates the largest _k_-cluster in a DAG. A _greedy algorithm_ for an optimization problem is one that uses a heuristic to find a local optimum at each step of the process, in order to approximate a global optimum in a short amount of time. A _recursive algorithm_ for a problem is one that solves the problem by first solving smaller instances of the same problem; thus the algorithm contains at least one call to itself.

The authors introduce GHOSTDAG as follows:

> The algorithm constructs the Blue set of the DAG by first inheriting the Blue set of the best tip _B_\_max, i.e., the tip with the largest Blue set in its past, and then adds to the Blue set blocks outside _B_\_max’s past, provided that the _k_-cluster property is preserved.

In other words, the problem to be solved by _recursion_ is finding the blue set \(maximal _k_-cluster\) of the entire current DAG, which relies on the incrementally smaller problems of finding the blue set of each of the DAG's tips. Solving a problem with recursion is possible with a _base case_—in this case, the _genesis_ block—where the solution is trivial—in this case, the blue set consists of itself. The _greedy heuristic_ is to construct the curent blue set with the largest blue set of these tips, then add to it all _k_-cluster preserving blocks outside the selected tip's past.

> Observe that this greedy inheritance rule induces a chain: The last block of the chain is the selected tip of _G_, _B_\_max; the next block in the chain is the selected tip of the DAG _past_\(_B_\_max\); and so on down to the _genesis_. We denote this chain by _Chn_\(_G_\) = \(_genesis_ = _Chn_\_0\(_G_\), _Chn_\_1\(_G_\), . . . , _Chn_\_h\(_G_\)\).

Henceforth, we refer to this chain as the _selected chain_ of the DAG.

> The final order over all blocks, in GHOSTDAG, follows a similar path as the colouring procedure: We order the blockDAG by first inheriting the order of _B_\_max on blocks in _past_\(_B_\_max\), then adding _B_\_max itself to the order, and finally adding blocks outside _past_\(_B_\_max\) according to some topological ordering. Thus, essentially, the order over blocks becomes robust as the colouring procedure.

In GHOSTDAG, finding the current blue set is needed to find _B_\_max in the future DAG, and future _B_\_max \(and its blue set and ordering\) is needed to find the blue set and ordering in the future DAG. Like coloring, ordering the blocks is recursive and greedy. Coloring and ordering of blocks happen in the same iterations, and the recursive call is to the entire GHOSTDAG algorithm. Note that the ordering favors the blue set, by nature of it favoring _B_\_max. Once a total block order is reached, a consistent transaction set can be extracted. GHOSTDAG returns an ordered list of blocks and a blue set to be inherited by the future DAG.

For the formal GHOSTDAG algorithm and a walk-through of it, see section 2.4.1 of [the PHANTOM paper](https://eprint.iacr.org/2018/104.pdf).

#### 2.1.2. UTXOs

Like Bitcoin, the consensus layer uses a UTXO model; specifically, our core client is a fork of [btcd](https://github.com/btcsuite/btcd), a Bitcoin implementation in Go. In the UTXO model, currency balances are updated by individual _unspent transaction outputs_ \(UTXOs\), which are, as named, the outputs of transactions, and are spent as input to other transactions. The total supply of spendable outputs is the _UTXO set_. This model, in which transactions determine changes in the UTXO set, has strong benefits over the account model, as used by Ethereum, in which transactions are events in the blockchain state machine: transactions in the UTXO model can be verified in parallel and with no extra overhead, and any smart contract running on top of this layer can more easily manage its own state, which is attached to the UTXOs. Moreover, the UTXO model is deterministic and allows the consensus layer to remain thin and isolated.

#### 2.1.3. Full Node Efficiency

The consensus layer uses UTXO commitments, pruning, and a finality window, which ease the maintenance of existing full nodes and the syncing process for new nodes. As described in section 1.3.3, we believe this lightweight trust model effectively balances high throughput and self-sovereignty.

**UTXO Commitments**

\[incomplete\]

**Pruning**

\[incomplete\]

**Finality**

\[incomplete\]

#### 2.1.3. Two-Way Pegging

The consensus layer also provides native support for smart contract consensus: it supports staking, dispute resolution, transfer of transaction fees to virtual machine instances, and more. Crucially, it supports two-way pegging, rendering Kaspa the financial railing of all financial applications in the smart contract layer. We use the term "bridge" when referring to components that connect the consensus layer to the smart contract layer. The bridge architecture is detailed in the “Smart Contract Layer” section.

#### 2.1.4. Privacy

\[incomplete\]

#### 2.1.5. OPoW

This layer uses optical proof-of-work \(OPoW\) consensus, whose R&D efforts are primarily led by [PoWx](https://www.powx.org/). OPoW is a novel approach to PoW mining that runs on energy-efficient photonic co-processors. It shifts the cost of mining from operational expenses \(OPEX\), which contribute to energy waste, to capital expenses \(CAPEX\). It inherits its basic security from the SHA hash function, which is composed with a permutation that is optimized for photonic ASICs. OPoW solves PoW’s traditional shortcomings: it alleviates the environmental effects of mining and promotes geographical decentralization, mining democratization, censorship-resistant mining, and robust growth of hashrate.

Our plan is to launch with a dual PoW and \(algorithmically\) transition to OPoW, as this technology matures and is capable of contributing sufficient hashrate to secure the network.

More information about OPoW can be found in the [OPoW litepaper](https://www.powx.org/download-litepaper).

#### 2.1.6. Coin Issuance

\[incomplete\]

### 2.2. Smart Contract Layer

On top of the consensus layer, the smart contract layer is an environment for operating virtual machine instances. This layer is decoupled from the base layer, such as in the family of “rollup” scaling solutions, which includes [Offchain Labs](https://offchainlabs.com/)' [Arbitrum](https://offchainlabs.com/Arbitrum-USENIX.pdf) and [Plasma Group](https://plasma.group/)'s [Optimistic Rollups](https://medium.com/plasma-group/ethereum-smart-contracts-in-l2-optimistic-rollup-2c1cef2ec537) \(OR\).

#### 2.2.1. High-Level Description

A virtual machine instance can be thought of, simply, as a set of interrelated smart contracts that are bootstrapped together and form this instance’s state. Henceforth we refer to these virtual machine instances as “silos”.

A silo is validated and secured by a permissionless set of stakers \(or “managers”\)—the consensus layer does not regularly interpret or verify silo data. The silo managers use the base layer consensus only for ordering silo transactions and resolving disputes. They publish short _commitments_ to silo state on-chain \(i.e., on the consensus layer\). A commitment is in the form of an accumulated hash to full silo state. The integrity of the computations is preserved via _staking_; any silo manager can validate a commitment against its own accumulated hash; if the hashes are different, the manager can challenge the false commitment and start a _dispute process_, which ultimately proves the correct state and slashes the stake of the malicious party. The main responsibility of the bridge \(the components that connect the consensus layer to the smart contract layer\) is to identify the correct commitments and to achieve _finality_ regarding these commitments and their outcomes.

Arbitrum and OR differ in the way disputes are managed and proofs submitted, and we intend for Kaspa to support both constructions. \(Kaspa deviates from Arbitrum’s design in some core manners, most importantly in using the base layer to record all transactions and function calls, and in making the set of validators permissionless.\) Importantly, these constructions rely on a robust, highly available, and censorship resistant data layer, for which the consensus layer, using PHANTOM, is a natural fit.

**The Any-Trust Security Model**

The security of a silo relies on the dispute mechanism. The ability to use miners as referees in case of a dispute allows us to rely on the _any-trust_ assumption: it suffices to assume that a single honest manager exists and is online to provably finalize silo state commitments. This stems from the fact that any honest manager can use the dispute mechanism to challenge and penalize any false claim, hence by remaining silent an honest manager passively confirms correctness of claimed state.

#### 2.2.2. Silo State and Commitments

A commitment to a silo state needs to specify the state it started executing from and the set of transactions it processed. Kaspa therefore requires that each commitment point to a previous commitment which represents the state it started executing from, and that it specify a reference block in the base-layer DAG, meaning it includes all silo transactions from the previous commitment up to the reference block.

![On-chain commitments](../../.gitbook/assets/image%20%288%29.png)

To simplify the discussion we abstract the base-layer's DAG structure into a blockchain by looking only at the _selected chain_ of the DAG. Commitments to a silo will only use reference blocks from the selected chain. This simplifies commitment management by providing a clear point of reference for synchronizing commitments made by different managers. Since silo commitments must be executed sequentially anyway, there is no apparent serialization cost by restricting commitments to the selected chain; high throughput is still achieved by parallel transaction mining and due to multiple silos operating independently in parallel.

**Tree Structure**

When a silo is created, it defines its initial state and provides a commitment to it. We refer to this initial commitment as the genesis commitment for this silo. Following the genesis commitment, any other commitment must point at some previous commitment as its logical ancestor, thus forming a tree structure. A fork in this tree means that two or more commitments in the tree are pointing to the same parent commitment. A fork can occur if \(i\) two identical commitments were made by two different managers, in which case Kaspa treats them as one; \(ii\) two different unsynced commitments were made \(i.e., pointing at different reference blocks\), in which case we define a rule to sync and logically unify them, described in the following section; and \(iii\) two different synced commitments were made, which means the two committing managers disagree regarding silo state and the disagreement must be resolved by a dispute resolution process.

At times of normal execution, when no malicious managers are involved, we expect the commitment tree to immediately converge into a chain. If an attack occurs, the dispute process will allow us to eventually ignore and abandon the false commitment branches. A commitment node is hence considered finalized when it has no siblings in the tree and finality confirmation criteria have been reached for it, as described in the “Finality” section below. Finality rules imply that a commitment is finalized only if all its ancestors are finalized, thus forming an ever growing finality chain at the upper part of the tree, starting from the genesis.

**Logical Fork Merging**

In the “Staking” section we describe the lottery mechanism by which a silo manager "wins a ticket" to perform a commitment. For now it suffices to say that lottery should result in a single commitment per reference block in expectation. However, Kaspa must deal with the cases where no commitments are made for a reference block \(i.e., no winning ticket\), or multiple commitments were made \(multiple simultaneous winners\), causing a fork in the commitment tree.

To address these cases, rather than sending a single commitment for the current reference block only, a commiting manager should explicitly specify commitments to all not-yet-committed-to reference blocks. More formally, we define the following commitment rule:

* **Scenario:** A manager wins a ticket for committing to reference block _j_+_k_ and the last commitment transaction it locally observes in the DAG is for reference block _j_.
* **Action:** the manager reports _k_-1 commitments starting from reference block _j_+1 and ending with reference block _j_+_k_.
  * There is no need to report a commitment for a reference block if it contains no transactions for this silo.

This way, when commitment transactions are received on-chain, any gap in the commitment chain can be immediately filled, and identical commitments can instantly be identified and merged in the tree.

If, however, two synced commitments are found to be different, participants must use the dispute process. The management of forks in the tree data structure in such cases is described in the “Disputes” section.

**Fees**

Each commitment to silo state should reward the manager making the commitment with a fee \(once the commitment is accepted as finalized\). The fee is collected from the fees paid by users when publishing their function call transactions which were included in the commitment.

When a miner adds a function call transaction to a block it pushes half of the transaction fee to the corresponding _native silo account_ \(grouped for all transactions in the block\).

Eventually, the fee should be equally split between managers/challenger which committed the finalized commitment. Usually, the manager\(s\) publishing the commitment would be those who won the lottery tickets to commit. However, when challenges are involved, the fee should be paid to the winning challenger \(although he had no ticket to commit\).

Although fee payment is essentially an unpegging operation \(pulling currency from the silo account\), unlike usual unpegging transactions, the manager cannot explicitly state the destination for the fee in the outbox operation. This is because \(i\) specifying his own address will render the commitment different for each manager \(since outbox operations are part of state commitment\), and \(ii\) the amount of fee to be paid depends on the number of managers making the same commit \(i.e. \# winning tickets\), which is unknown in advance. Thus, the operation only specifies the amount of fee to be paid without the destination. Following finality, a fee payment transaction is published with a witness to the fee amount, and the fee is equally split between all committers.

**Finality**

Commitments are confirmed by the fact that no honest manager successfully challenged their correctness. This requires some sort of commitment finality criteria, such that we can apply its implications \(e.g., permission to unpeg currency\) to the base layer. In the Kaspa smart contract layer, there are three forms of finalization:

* **Passive confirmation \(minimum time\).** Kaspa requires that a minimal amount of time passes, so that honest managers get the chance to challenge malicious commitments, henceforth passively confirming it if they do not.
* **Active confirmation \(minimum distinct stake\).** Commitments form a tree structure by pointing to previous commitments. By pointing to a previous commitment, the following commitment confirms it, and can possibly lose its stake if that previous commitment is challenged. Kaspa uses a policy where commitments can only be made according to a uniform lottery. This way, as more distinct, uniformly-chosen managers actively participate in a specific commitment branch, confidence in that branch increases and finalization speeds up.
* **Maximum time.** Finality should eventually be reached also at times with no silo activity if enough time has passed.

All three criteria contribute to finality in the Kaspa smart contract layer.

**Consensus Layer Reorg**

Silo finality is logically "internal" to consensus layer finality. This means a consensus layer reorg may undo operations which were recorded on-chain post silo finality. This happens if the reorg affects the validity of the source commitment. In order to be able to reverse back silo state in case of a reorg, managers must keep checkpoints of the complete silo state backwards in time to a point where base-layer finality was already achieved.

Silo state must be periodically saved in _checkpoints_ to allow recovery from base-layer reorgs. Managers must at least keep a single checkpoint with the following properties: \(i\) the checkpoint is for some state in the past which already reached consensus layer finality, and \(ii\) all transactions in the DAG from that step on are still available, so they can replay the execution according to the new order. If the selected chain of the DAG gets modified often by slight reorgs, they should also keep checkpoints which are much closer to the current time.

**Syncing**

Syncing to silo state requires that one or multiple managers holding the complete silo state must send it to the interested newcoming manager. The receiving manager can check the validity of the state by hashing and comparing to on-chain silo commitments. The new manager should avoid making commitments to the silo until the state he received was proven as finalized. Another option is that he receives a checkpoint from a point at time which has been finalized already, so he can re-execute from that point on his own and make commitments as soon as possible.

#### 2.2.3. Staking

To incentivize correct execution of a silo, miners must be able to penalize \(or reward\) silo managers for making \(or challenging\) false commitments. To support this, we define a staking mechanism in the base layer. Staking is also used to enforce an economically fair lottery for making commitments to a silo.

When a silo is created it specifies in its configuration a _staking unit_ \(in the native currency\). This staking unit reflects an amount of currency that will be used to penalize false commitments and is high enough to discourage malicious behavior for this silo.

The _staking transaction_ is a transaction for locking native coins to participate in silo management. The output of a staking transaction is in the form of a wrapped staking token we call "USTO" \(Unspent Staking Transaction Output\), which holds the value of a single staking unit and grants permission to perform commitments/challenges within the silo. The staking transaction receives as input a target silo and native UTXO coins, and outputs one or more USTOs according to the pre-configured staking unit of the target silo.

A USTO token is the primary input for commitment transactions. USTO tokens are included in the UTXO set of the DAG; however, they are not considered "spent" when used for commitments. The token should be considered spent \(and thus extracted from the UTXO set\) only when being slashed following a false commitment or when requested to be unlocked by its owner. To unlock its stake, a silo manager must call the _unlock stake transaction_. The unlocking should be rejected if the specified token was recently used by any not-yet-finalized commitment.

**Lottery**

USTOs are also used for managing a lottery of commitment "tickets". For each current USTO and new reference block Kaspa sets a lottery over the combined hash of the USTO and the reference block. The USTO wins the lottery if the resulting hash is small enough. More specifically, the lottery function is defined as the result of the following predicate:

$$
{xor(hash(token)),hash(block)) \over maxhash} < {1 \over n}
$$

where _n_ is the overall number of current USTOs for this silo. This lottery results in an expectation of a single commitment ticket for each reference block \(a Binomial distribution with _n_ trials and _p_ = 1/_n_\). Overall, the chance to win a ticket grows linearly with the number of USTOs one holds. In the long run, this results in a fair distribution of commitment fees to all managers, where each manager earns fees proportionally to the number of tokens he owns, i.e. proportional to the amount of stake he was willing to lock as deposit.

#### 2.2.4. Disputes

At any time, a commitment to silo state can be challenged by any other interested party. Commitments can only be challenged while not yet finalized. The challenger must provide stake for backing up his challenge. This stake can be in the form of an already existing USTO or with native coins \(equal to a staking unit\). Once a challenger declares a challenge and the base layer processes it, a dispute process begins. Following the dispute process rules, which are described in the “Dispute Process” section below, one of the arguing parties loses and its stake is slashed. The winning party initiates the slashing as a special _request slashing transaction_ which "spends" the loser’s USTO, pays the winner half a staking unit in native coins, and burns the other half.

A challenge contains an alternative commitment to silo state. This commitment immediately creates a fork in the commitment tree. Alternatively, a dispute can begin when two conflicting commitments are made. In the latter case, Kaspa considers both commitments as pending until one of them is challenged.

Challenges to a commitment should be made within the finality time window. If multiple challenges are made within this time, the challenged manager must address them one by one, until he loses once or wins them all. The order in which the manager addresses the challenges follows the consensus layer’s ordering of the challenging transactions.

**Commitment Chain**

A challenged commitment may already have a commitment chain following it. All following committers to this chain implicitly agree about all previous commitments. Hence, if a commitment is challenged and loses, all following committers are slashed as well.

However, if the commitment loses a dispute, this can be an impostor attack in which the committer deliberately loses the dispute although the commitment is correct. To address this, Kaspa allows any following committer to defend the commitment as well. Likewise, any losing commitment can be reclaimed by any other manager willing to challenge the former challenger.

In other words, the dispute process never reveals something as "pure truth". Miners are only convinced that one of the parties did not stand for his claim.

**Dispute Process**

Once a dispute process begins between two parties, Kaspa uses a dispute data structure to follow the state of the interaction between them. The dispute proceeds by the challenged manager \(the "prover"\) sending the overall number of instructions it executed, T, and _k_-1 commitments representing intermediate silo states at _k_-1 predefined locations with T/_k_ instruction intervals between each. Note that _k_ is a parameter which affects the overall number of interactions required to resolve the dispute \(log\__k_\(T\) interactions\), and is a predefined parameter which depends on the delay between rounds and the cost of large vs. multiple transactions. The challenger then points at the first false commitment of the _k_ commitments, and the process repeats recursively within the interval between the false commitment and the commitment prior to it. This recursive logic continues until the challenger points at two consecutive commitments at times _t_, _t_+1 which are the exact points of conflict. The prover then needs to send a proof \(see sections 4.2 and 4.3 of [Arbitrum's academic paper](https://www.usenix.org/system/files/conference/usenixsecurity18/sec18-kalodner.pdf) for a detailed explanation of such “one-step proofs”\) showing the correct execution of that step. If the proof is correct, the prover wins the dispute, otherwise the challenger wins. Each of the parties must respond at his turn within a specified time period, otherwise he loses.

![Dispute Process](../../.gitbook/assets/image%20%285%29.png)

This dispute process image is inspired from page 10 of [Arbitrum's whitepaper](https://offchainlabs.com/arbitrum.pdf).

Participants perform all dispute interactions as special on-chain transactions. They save the sent data in the dispute data structure so the ending proof can be validated against prior commitments made.

#### 2.2.5. Silo Outbox

While executing silo function calls, the user may request, via the smart contract logic, to perform base-layer transactions. Such transactions may include transferring coins back to the consensus layer \(unpegging\), or making function calls to other silos \(composability\). We call such transactions the "output transactions" of a silo function call. Output transactions can only be published on-chain when the commitment including their initiating function call reaches finality. We now describe the process of post-finality publication of these transactions.

During function execution, any output transaction is appended to a dedicated list within the silo. When a group of function calls \(all calls from a specific reference block\) is finished processing, the hash of all output transactions is combined in a Merkle tree with the hash of the silo's memory space, and together they form a "commitment" to the silo state. Additionally, this list of transactions is appended to a pending-finality transaction queue \(the "outbox"\) to be published on-chain when the corresponding commitment is finalized. Upon reaching finality, these transactions are dequeued from the outbox and published on-chain, referencing the originating commitment. The published transaction must include a Merkle witness showing that the operation was indeed included in the claimed commitment.

The responsibility for publishing outbox transactions is on the managers who made the commitment \(or committing manager with the lowest lottery hash, to avoid the possible redundant double publication\). These managers have an incentive to publish the transactions, since they include their own commitment fees. If the committing managers are offline at the time finality is reached, Kaspa moves the responsibility to managers who made subsequent commitments.

**The Silo Native Account**

Each silo includes a special native account. This native account functions as a money pool representing all shared resources pegged to silo control, and managers manage partitioning of this pool within silo state. This account also functions as a bridge for transferring fees from miners \(when mining silo function calls\) to silo managers \(once commitments to those function calls are finalized\).

Sending currency to this account is as simple as any currency transfer \(signed by the sender to the public address of the account\); however, sending currency from this account back to the user requires that all silo managers agree with the transaction \(any-trust model\), which is accomplished when silo state commitments reach finality.

**Pegging**

Pegging is the process of transferring currency from the consensus layer to silo control. From the base layer's perspective it is a simple signed transaction passing native coins from a user address to the public address of the silo. However, this transfer must be accompanied with a special silo function call for reporting the transfer to the internal state of the silo. To link the two operations atomically, Kaspa uses a special transaction that combines the native coin transfer with a silo function call in its payload for reporting the currency deposit.

**Unpegging**

Unpegging is the reverse process, sending native currency from the silo account to a user account. As explained above, unpegging requires waiting for finality of the initiating commitment and thus, the operation passes through the silo outbox.

There is, however, a challenge in managing such unpegging transactions. For instance, consider a basic scenario where a silo manages an account-based model and the user would like to pull currency from its internal silo account to its native public address. The user calls a function in the silo's contract for requesting the operation. However, in order to create a native transaction, the silo contract code must access UTXO coins, which are associated with the silo’s native account, to use them as inputs. This is problematic since it requires access to data external to the silo state \(to enforce deterministic execution which is required for the dispute mechanism, I/O should not be possible within a silo\). Another problem is that those coins must be "locked" until the transaction is actually published on-chain post finality.

To address this challenge, instead of preparing actual transactions in the outbox, the outbox operation only includes the required metadata for the operation, in the form of "user x should get paid y coins from silo account". Later, when finality is reached, a manager can group all these operations into a single native unpegging transaction, passing currency from the silo to the list of specified users with the specified amounts, along with the corresponding witnesses to the source commitment. At this stage, the manager can easily access any required UTXO inputs, since this step is not executed from within silo contract code. To prevent double spending of such a transaction \(replay\), a boolean field attached to each finalized commitment should indicate if its unpegging transaction has already been processed; another option is managing an increasing nonce for each silo for tracking all unpegging transactions.

**Composability**

Outbox transactions may contain function calls to other silos. Replay attacks and authorization are the concerns of the receiving silo. This means that function calls to other silos may be published before finality \(some function calls may include pegging native coins to the other silo and hence require post-finality\).

#### 2.2.6. Virtual Machine

The virtual machine is the execution environment for the silo's smart contracts. The environment consists of a memory space representing silo state, smart contracts representing silo logic, and an interpreter for executing contract functions over silo state. In order to function properly over the consensus layer bridge and to support the any-trust model, the VM needs to \(i\) have completely deterministic execution, so distinct distributed managers can agree on its state; \(ii\) support hashing its entire state to a constant-size hash which will be used by managers to make on-chain commitments; and \(iii\) be able to provide proof for correct execution of a single step of the machine. Since the Kaspa VM is modelled from the Arbitrum VM, more details can be found in section 4.3 of the [Arbitrum paper](https://offchainlabs.com/Arbitrum-USENIX.pdf), and also in the VM section of the [extended Kaspa smart contract layer document](https://github.com/kaspanet/documentation/blob/master/Smart%20Contract%20Layer/Smart-Contract-Layer.md#virtual-machine).

### 2.3. Rich Web of Trust Layer

On top of the smart contract layer, the rich web of trust layer is an environment for bootstrapping independent webs of trust \(WoTs\). A WoT is, simply, a trust graph whose topology is digitally signed by the participating nodes, and which enables users to authenticate and verify data and events without relying on trusted third parties. Existing smart contracts platforms fall short in addressing counterparty risks, as there is no native interaction between the contracts and real-world data and relationships. Rich WoTs intend to serve as a layer where real-world trust relationships are recorded, in different trust contexts: credit worthiness, identity attestation, personal integrity, reputation, etc. These WoTs enhance the expressiveness of the system by allowing smart contracts to interact with and verify the WoT data. They enable automatic on-contract interactions with third parties while protecting the user from counterparty risk.

We refer to “rich” WoTs to allude to \(i\) a broader usage of these trust graphs than the prevalent usage, namely, decentralized identity attestation, \(ii\) the interaction of WoTs with the consensus layers, to affect and be affected by their logic.

#### 2.3.1. Webs of Trust

A WoT is represented by a weighted directed graph \(V, E, w\), where V and E represent the set of nodes and the set of edges, respectively, and w is a weight function on edges. An edge _e_ = \(_u_, _v_\) ∈ E and its weight w\(_e_\) are considered valid only if they are signed by _v_’s public key. The interpretation of \(_e_, w\(_e_\)\) depends on the context of the corresponding WoT: it can represent the degree of general trust, the assigned reputation score, the allowed credit flow, an identity attestation, etc.

Importantly, the WoT structure induces a trust relationship between any pair \(_u_, _v_\) of two nodes that are directly or indirectly connected: if _u_ and _v_ are directly connected \(_e_ = \(_u_, _v_\)\), then w\(_u_, _v_\) = w\(_e_\). Otherwise, w\(_u_, _v_\) can be inferred from the WoT structure in a number of ways, depending on the use case and design. w\(_u_, _v_\) can be set to the maximum flow between _u_ and _v_, a multiplication of the edge weights on the minimal or maximal path between _u_ and _v_, or some other approximation of cumulative weight.

![Web of Trust](../../.gitbook/assets/image%20%282%29.png)

The above image is an example of a small web of trust. If we measure weight between two indirectly connected nodes as, for instance, half the sum of the edge weights on the minimal path between them, then $$w(A,H)=.5*(4+1+1)=3$$.

#### 2.3.2. Interaction with Lower Layers

WoTs can be organically incorporated into smart contracts’ logic, by conditioning certain contract behaviors on given users’ contexts. For instance, Alice can let her funds be governed by a smart contract that will direct them as a loan according to her local view of a credit WoT. This replaces P2P lending platforms with a peer-to-contract flow, which saves on operational and intermediary costs, and interacts with the underlying SoV \(consensus layer\) and MoE \(smart contract layer\) in an organic way. More generally, smart contracts can now be context-aware, increasing their expressiveness, allowing integration of real world data and relationships, and, at the same time, preventing systemic damage from sybil nodes and fake accounts.

The interaction between a smart contract and a WoT can go both ways: not only can the behavior of the smart contract be conditioned on WoT data as described above, but also, WoTs can be programmed to update based on on-chain data; for instance, the trust from Alice to Bob automatically increases upon an on-chain transaction from Bob to Charlie, which can make sense, e.g., if Charlie is an exchange compliant with AML/KMC regulations.

#### 2.3.3. Sybil Resistance

Without the proof-of-work that fuels the consensus layer, WoTs in the rich web of trust layer need another method to counter sybil nodes. The basic observation is that when the graph topology represents some real-world relationships, attackers are limited by their connectivity to honest nodes—it is not easy for an attacker to convince many honest users to trust them. Formally, for an honest node u and attacker node v, w\(v, u\) can be proven to be small under the assumption that the attacker’s connectivity to \(or trust from\) honest nodes \(technically, the size of the cut from honest nodes to attacker nodes\) is smaller than some threshold.

Note that we are, in most cases, not trying to achieve global agreement on the identity of sybil nodes.\* Different honest nodes may assign different trust scores and potentially disagree on the identity of sybil nodes. Naive honest nodes who assign high trust scores to sybil nodes may be hurt, but the effect provably will not cascade to healthy honest nodes \(see, e.g., [this discussion](https://pdfs.semanticscholar.org/29a0/536f1e607400402434de8d51e94ca7585f33.pdf) from Alvisi, et al.\).

\*In order to generalize this notion into a global sybil-resistant consensus, the protocol would need to predefine at least one honest node \(in order to break any symmetries designed by the attacker between his faction and the honest faction\), which undermines the idea of decentralized global consensus—it would simply become a leader-election protocol with a hardcoded leader.

The following image roughly represents the full Kaspa consensus stack.

![Kaspa Stack](../../.gitbook/assets/image%20%286%29.png)

## 3. Applications

This section describes example applications of the Kaspa stack.

### 3.1. Stable Tokens

\[incomplete\]

We plan to implement a smart contract silo that will support a medium of exchane \(MoE\) stablecoin. There are two possible constructions:

* Implementing a native stablecoin whose relationship to the native Kaspa will resemble that of Dai’s relationship to Ethereum.
* Abstracting an existing stablecoin such as Dai or Tether.

In the short term we will work on abstracting an existing stablecoin, and will assess the need to develop an independent stable token as the need arises and regulation status clears.

### 3.2. Web of Trust Applications

The rich web of trust layer opens up a host of potential use cases, many of which the decentralized identity community has explored. Our primary use case is decentralized lending and credit. Web of trust applications include:

* **Peer-to-contract lending \(web of credit\):** We envision a WoT that represents credit relationships and functions as a decentralized underwriting platform. Nodes assess the credit worthiness of loan-seeking nodes, and give out loans according to their local credit scoring of nodes \(recall we are not trying to achieve a global consensus on reputation\). A peer-to-contract lending process automates a credit issuer’s decisions and allows her to be a passive participant, delegating the lending operation and sybil protection to the contract. It allows her to invest in companies, projects, instruments, and even individuals in ways that are impractical with current P2P lending platforms, let alone traditional lending vehicles. With such a system, we can easily imagine how credit can flow from a Korean businesswoman to a young student who has joined an Income Share program to cover his studies; or from a Russian financier to a Filipino worker who needs a stream of payday loans to bridge the gap between his daily expenses and payday. These credit paths will be found and governed by smart contracts, automatically, thereby forming a faster, cheaper, and orders-of-magnitude more connected credit market.
* **Kaspa airdrop WoT:** As an initial demonstration of a WoT, we intend to bootstrap the Kaspa network by airdropping coins to as many people and entities as possible via social networks and other online platforms. Previous airdrop attempts by other groups have been susceptible to sybil attacks by fake accounts. However, as described by the footnote in 2.3.3, there exist mechanisms that counter sybils by interpreting a social \(or other real-world\) network from a single honest node’s perspective, thereby breaking the symmetry to honest subgraphs that sybils may attempt to create \(achieving micro-global consensus without full decentralization\). In our case, the airdrop smart contract will prescribe this role to DAGlabs’ node within the corresponding social network, thus guaranteeing resistance to bots and ensuring we only airdrop Kaspa to real entities that are interested in the project.
* **Key management:** A WoT for identity attestation and private key revocation can be bootstrapped \(ideally with the help of some crypto-wallet company\) to solve one of the biggest anxieties of cryptocurrency users. Users will select trusted peers in this WoT then deposit their money in a smart contract acting as a safe. Then, in the case of key loss, the user can reclaim her funds to a new public key address by presenting a \(threshold\) signature from a sufficient set of peers in her trust zone.
* **Identity management:** Many applications require authentication of the user’s real-world identity. The authentication process can be outsourced from the \(decentralized or centralized\) application to the identity WoT: the application designer selects a set of trusted oracles within the WoT, which in turn may select other trusted identity oracles and delegate the trust to span a wide range of users. This enables authentication processes that are not limited to one single trust zone \(such as Google authentication, Facebook login, etc.\). This proposed identity management system is a special case of a more general concept—decentralized Certificate Authority systems, which will similarly enable applications to use a certificate authority of their choosing.
* **Recommendation system:** Recommendation systems are a natural use case of reputation systems. There are two problems with existing recommendation systems: first, many entities are incentivized to produce fake positive reviews for themselves and fake negative reviews of their competitors. Second, people’s tastes are different, and so the recommendations produced by real users are not uniformly relevant to the end user and should be weighted in more nuanced ways.

Web of trust allows us to actualize these decentralized trust systems, in the identity context and beyond.

## 4. Conclusion

As world citizens continue to question the soundness of fiat currencies and centralized financial institutions, we recognize the need for an alternative, sound money base that supports extensive financial applications in a cohesive, decentralized platform. Kaspa aims to fill this need with its three-layered stack of \(1\) global consensus, decoupled from \(2\) smart contract computation, with \(3\) webs of trust operating on top of the smart contracts. It makes up Bitcoin’s technological shortcomings, primarily, by using the novel PHANTOM blockDAG consensus protocol, which is a generalization on the core innovation of Satoshi Nakamoto, but improving upon Nakamoto consensus in transaction speed and mining decentralization. It also diverges from Bitcoin’s “everyone should be his own bank” norm, enabling a flexibility that expressive financial products require. At the same time, Kaspa adheres to several crucial Bitcoin-like characteristics---including a UTXO model, proof of work, thin base layer, and fixed monetary policy---which Ethereum, a project that aims to extend the usability of blockchain, lacks. Kaspa’s smart contract layer is similar to the family of “rollup” scaling solutions that have been proposed for Ethereum—but it operates on top of the fast, secure, blockDAG base layer. Finally, Kaspa’s topmost layer serves as a platform for web of trust applications, such as undercollateralized peer-to-peer credit, that have been thoroughly discussed in the decentralized identity and cryptography communities, which can piggyback off of the programmability of Kaspa’s smart contracts and the security and speed of its base layer.

