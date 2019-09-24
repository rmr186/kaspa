# Kaspa BlockDAG Core Repo

This repository holds the core Golang implementation of a Kaspa - a modified PHANTOM protocol forming the world's first truly decentralized & robust value transfer using Proof of Work consensus & Staking based trustless smart contracts.

## Kaspa Repo Structure

All the basic components needed for running a p2p transaction ledger are available in the Kaspa Core Repo on [Github](https://github.com/daglabs).  Mainly these can be categorized as follows:

* Kaspa BlockDAG Nodes:
  * Full Node \(FN\):
    * Based on btcd \(Golang Bitcoin implementation\), compiling kspd,go will build a Kaspa full node locally.
  * P2P Full Node:
    * Same as FN, runs locally but connects to neighboring nodes on the Kaspa network
  * P2P Pruned Node:
* JSON.RPC API for node communication
* Kaspa BlockDAG Server API:
  * Network data via  centralized API server
  * Fast, reliable network analysis tools
* Kaspa Package Utilities:
  * Commonly used Kaspa packages are outlined in the next section.

## Next Steps

We highly suggest you spend a little time understanding how BlockDAGs work, how they are different then Bitcoin as well as in what way both these 'proof of work' consensus protocols really work the same.

After getting a feel for Kaspa \(and maybe reading the GhostDAG protocol's white paper\), you will most likely next start getting to know how Kaspa packages work as you start to interact with the Kaspa Devnet.







