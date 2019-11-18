---
description: Introducing the Kaspa BlockDAG Currency
---

# Kaspa Overview

## What is Kaspa?

Kaspa is a high throughput, photonic proof-of-work, expressive, sound money.

Kaspa uses [Phantom](https://eprint.iacr.org/2018/104.pdf), a highly scalable and decentralized blockDAG consensus protocol, that generalizes over the [Nakamoto](https://bitcoin.org/bitcoin.pdf) consensus. It is called a block_DAG_ rather than a block_chain_ because instead of a chain of block, Kaspa's ledger is stored in a directed acyclic graph \(DAG\) of blocks. What this means is that many blocks can be created in parallel.

Kaspa uses optical proof of work \(OPoW\) - an implementation of traditional proof of work \(PoW\) with photonic optical processors rather than silicone processors. OPoW retains the same security properties of traditional PoW, and because photonic processors are more energy efficient and green, allows for more sustainable and geographically decentralizing mining, and fair participation in mining rewards.

for a Kaspa is optimized for sound and scalable store of value, payments, settlements, transfer of digital assets, and proof of publication. Kaspa enables a computation layer inspired by [Arbitrum](https://www.usenix.org/node/217514) and [TrueBit](https://people.cs.uchicago.edu/~teutsch/papers/truebit.pdf), and decoupled from the consensus layer, allowing for a scalable native currency while supporting and enhancing upper-layer use cases, such as stable tokens and decentralized credit systems.

## How Does it Work?

All the basic components needed for running a p2p transaction ledger are available in the Kaspa Core Repo on [Github](https://github.com/daglabs).  Mainly these can be categorized as follows:

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

