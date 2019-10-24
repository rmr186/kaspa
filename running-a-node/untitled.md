---
description: Get your own node running on the Kaspa TestNet
---

# Running a Full Node

The full nodes play a critical role in the Kaspa network.  Every client application ultimately needs a Kaspa node to connect with the network.  

If you are an application developer, you will likely want to run a node acting as a network point of access. If you are running a [Kaspa API Server](../api-reference/api-calls/api-server-setup.md) you will need a Kaspa node to connect it to the network.  If you are using [Kaspa's Websocket API ](../codebase/kspa-codebase/rpcclient.md)rest assure there is a Kaspa node connecting it to the network.

The following instructions will help you get a full node setup with basic configuration.

## Prerequisites

* **Install the Go distribution.**  Kaspa's code is based on Bitcoin's Go implementation, you will need to [setup a Go development environment](https://golang.org/doc/install) to build and run Kaspa code.
* \[Nice to have\] ****[**Docker installed** & Docker Hub user account.](https://hub.docker.com/)  While these instruction are not using pre-made Docker images, there are several Docker images with useful functionality, such as running a self contained local Kaspa network, and other testing & development tools.
* **Git installed on your machine.**  For instructions[ installing & using Git please see this page](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup).

## Build from Source Code

If you haven't done so already, clone the Kaspa repo using the following url:

```bash
:~/code/repos$ git clone https://github.com/daglabs/btcd
```

Next, make sure you have the Go development environment installed \(if not, [use these instructions](https://golang.org/).\)

```bash
:~/code/btcd$ go version
```

Next, install _btcd_ \(Kaspa Full Node\) using the Go _install_ command \(from the the _btcd_ directory\):

```bash
:~/code/btcd$ go install . 
```

## Launch the Node

To start the node with default configuration, use the following command:

```bash
:~/code/btcd$ ./btcd
```

The above command runs a full node but does not yet connect to any Kaspa networks.  To connect to DevNet use the following parameters when launching the node \(these parameters will obviously be different for future TesNet & MainNet\):

```bash
$ ./btcd --devnet --rpclisten=localhost:18334 --rpcuser=user --rpcpass=pass --notls --acceptanceindex --txindex
```

If successful, you should see the following response confirming your Kaspa node is operational:

```bash
2019-10-24 13:25:31.499 [INF] BTCD: Version 0.12.0-beta
2019-10-24 13:25:31.499 [INF] BTCD: Loading block database from '/home/roni/.btcd/data/devnet/blocks_ffldb'
2019-10-24 13:25:31.520 [INF] BTCD: Block database loaded
2019-10-24 13:25:31.524 [INF] INDX: Transaction index is enabled
2019-10-24 13:25:31.524 [INF] INDX: acceptance index is enabled
2019-10-24 13:25:31.524 [INF] BDAG: Loading block index...
2019-10-24 13:25:31.525 [INF] BDAG: Loading UTXO set...
2019-10-24 13:25:31.526 [INF] BDAG: DAG state (chain height 0, hash 000033abb09b45e09ef5e3a2b92185b7503a074565ef5706904167c60b37d6f4)
2019-10-24 13:25:31.527 [INF] RPCS: RPC server listening on 127.0.0.1:18334
2019-10-24 13:25:31.529 [INF] ADXR: Loaded 3 addresses from file '/home/roni/.btcd/data/devnet/peers.json'
2019-10-24 13:25:31.529 [INF] CMGR: Server listening on 0.0.0.0:18333
2019-10-24 13:25:31.529 [INF] CMGR: Server listening on [::]:18333
2019-10-24 13:25:31.811 [INF] CMGR: 3 addresses found from DNS seed n.devnet-dnsseed.daglabs.com
2019-10-24 13:26:03.766 [INF] RPCS: New websocket client 127.0.0.1:51374
```

## Documentation & Next Steps

For more information about the Kaspa network or the workings of a Kaspa Node,[ please see Kaspa Documentation.](../about-kaspa/kaspa-overview/)

For API reference as well as instructions on running your own Kaspa API Server, [please see this guide](../api-reference/api-calls/api-server-setup.md).

For more information about Kaspa's BlockDAG network, use cases, and research,[ please see the following repository.](https://docs.kas.pa/research/)



