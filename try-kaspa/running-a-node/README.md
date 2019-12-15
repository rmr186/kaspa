---
description: Setup and run a Kaspa full node.
---

# Full Node

### What is a Kaspa Full Node?

A full node is the basic participant in the Kaspa network. The network relies on these nodes to connect to each other, thus forming a peer-to-peer network of nodes that maintains the distributed ledger. A node receives blocks and transactions from its peer nodes, validates them, and spreads them across the network. A node also mines.

Kaspa's codebase is based on Bitcoin's Go implementation and you will need to setup [a Go development environment](https://app.gitbook.com/@kaspa/s/kaspa/~/drafts/-LsLyvtECTPlocs1u90J/primary/getting-started/running-a-node/build-a-node-server-from-source-code#prerequisites) to build and run Kaspa node, although you can interact with an existing node from any application using [JSON-RPC](interact-with-a-node/node-json-rpc-api.md) channels.

Kaspa does not rely on trusted 3rd parties or centralized servers, relying instead entirely on its peer-to-peer network nodes.  

### Who Needs to run a Full Node?

Users interested in mining Kaspa will need to connect their miner to a full node. The full node does contain a CPU miner, but you will need a full node to connect an external dedicated miner to.

Users interested in developing a wallet will need to run a local development network \(DevNet\) consisting of at least two interconnected full nodes. 

[An API server](../api-server/) is available for , connecting to a full node and serving wallet and block explorer requests.

### Setup Instructions

There are two ways to setup a Kaspa full node:

* Quick way, using a Docker image \(~10 minutes\): [here](full-node-quick-setup-with-docker.md).
* Manually building it from source code \(~30 minutes: [here](build-a-node-server-from-source-code.md).

### Working with a full node

After completing setup you can use either a [CLI commands](interact-with-a-node/node-cli-interface.md) or [JSON-RPC](interact-with-a-node/node-json-rpc-api.md) to interact with your node.







