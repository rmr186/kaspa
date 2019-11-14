---
description: Introducing the Kaspa BlockDAG Currency
---

# Kaspa Overview

## What's Kaspa?

Kaspa is a decentralized cryptocurrency based on the PHANTOM BlockDAG protocol.  

It uses proof-of-work \(PoW\) mining with a staking based smart contract layer, with the promise of improved PoW transaction-throughput, decentralized smart-contract layer, as well as innovation in network-participation economics.

You can learn more at [docs.kas.pa/org](https://docs.kas.pa/organization).

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

Finally, you can dig deeper into [Kaspa transactions](), or dive into our [codebase reference](../../codebase/code-ref/).

