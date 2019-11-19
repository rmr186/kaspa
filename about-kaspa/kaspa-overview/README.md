---
description: Introducing the Kaspa BlockDAG Currency
---

# Kaspa Overview

## What is Kaspa?

Kaspa is a high throughput, photonic proof-of-work, expressive, sound money.

Kaspa is optimized for sound and scalable store of value, payments, settlements, transfer of digital assets, and proof of publication.

## How Does Kaspa Work?

Kaspa uses [Phantom](https://eprint.iacr.org/2018/104.pdf), a highly scalable and decentralized blockDAG consensus protocol, that generalizes over the [Nakamoto](https://bitcoin.org/bitcoin.pdf) consensus. It is called a block_DAG_ rather than a block_chain_ because instead of a chain of block, Kaspa's ledger is stored in a directed acyclic graph \(DAG\) of blocks. What this means is that many blocks can be created in parallel. This in turn allows for near-immediate transaction confirmations.

Kaspa uses optical proof of work \(OPoW\) - an implementation of traditional proof of work \(PoW\) with photonic optical processors rather than silicone processors. OPoW retains the same security properties of traditional PoW, and because photonic processors are more energy efficient and green, allows for more sustainable and geographically decentralizing mining. OPoW, coupled with the rapid block creation rate of the blockDAG, means it is much more likely for miners to find blocks, and makes Kaspa mining rewards fair game.

Kaspa enables a[ smart contract](../smart-contracts.md) computation layer, inspired by [Arbitrum](https://www.usenix.org/node/217514) and [TrueBit](https://people.cs.uchicago.edu/~teutsch/papers/truebit.pdf), and decoupled from the consensus layer, allowing the native currency to remain lightweight and scalable, while supporting and enhancing upper-layer use cases, such as stable tokens and decentralized credit systems.

All the basic components needed for running a p2p transaction ledger are available in the Kaspa Core Repo on [Github](https://github.com/daglabs). Mainly these can be categorized as follows:

* Full Node \(FN\):  Compiling kspd,go will build a Kaspa full P2P node.
* Pruned Node \(PN\): Doesn't keep archival tx info, or other node overhead.
* JSON.RPC API for node communication
* Kaspa BlockDAG server API for network data via  centralized API server
* Package Utilities: commonly used packages outlined in the next section.

## Next Steps

We highly suggest you spend a little time understanding how BlockDAGs work, how they are different then Bitcoin as well as in what way both these 'proof of work' consensus protocols really work the same.

After getting a feel for Kaspa \(and maybe reading the GhostDAG protocol's white paper\), you will most likely next start getting to know how Kaspa packages work as you start to interact with the Kaspa Devnet.

[Next, try running your own local full node.]()

After you've got your node running, [check out this brief tutorial on node operations]() to learn how to generate transactions.

Finally, you can dig deeper into [Kaspa transactions](), or dive into our [codebase reference](../../api-reference/code-ref/).

