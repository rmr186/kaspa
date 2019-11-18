---
description: Setup and run a Kaspa full node from source (~30 minutes).
---

# Build from Source

## Prerequisites

Before building a node you will need to setup [Go runtime](https://golang.org/pkg/runtime/) and [Git](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup) onto the machine you want to use.

Next, make sure you have the Go development environment installed \(if not, [use these instructions](https://golang.org/).\)  

```bash
:~/code/btcd$ go version
```

This guide uses version `go1.13.4 linux/amd64`.

## Build a Node from Source <a id="build-from-source-code"></a>

Clone the Kaspa repo using the following url:

```bash
:~/code/repos$ git clone https://github.com/daglabs/btcd
```

Now it's time to build and install the Kaspa full node code.  From the the _btcd_ directory, run the following:

```bash
:~/code/btcd$ go install . 
```

Check that you have successfully installed the node by viewing the node command line help:

```bash
$ ./btcd --help
```

```bash
Usage:
  btcd [OPTIONS]

Application Options:
  -V, --version               Display version information and exit
  -C, --configfile=           Path to configuration file (default: /home/roni/.btcd/btcd.conf)
  -b, --datadir=              Directory to store data (default: /home/roni/.btcd/data)
      --logdir=               Directory to log output. (default: /home/roni/.btcd/logs)
  -a, --addpeer=              Add a peer to connect with at startup
```

Next, you will launch your node and connect to a Kaspa network.

## Run the Node <a id="launch-the-node"></a>

To start the node with default configuration, use the following command: 

```bash
:~/code/btcd$ ./btcd
```

The above command runs a full node but does not yet connect to any Kaspa networks.  To connect to Devnet use the following parameters when launching the node \(these parameters will obviously be different for future Testnets.\):

```bash
$ ./btcd --devnet --rpclisten=localhost:18334 --rpcuser=user --rpcpass=pass --notls --acceptanceindex --txindex
```

| Command Option | Use |
| :--- | :--- |
| `--devnet` | Kaspa network node should connect to |
| `--rpclisten` | Node port for rpc connection |
| `--rpcuser` | User for node rpc connection |
| `--rpcpass` | Password for node rpc connection |
| `--notls` | Setting: use rpc user instead of TLS certificate |
| `--acceptanceindex` | turns on the acceptance index, which allows one to quickly look up the blocks and transactions that were [accepted by a certain block](../../about-kaspa/kaspa-overview/dag-consensus/block-acceptance.md). |
| `--txindex` | turns on the transaction index, which allows one to quickly look up the blocks in which a certain transaction exists. |

{% hint style="warning" %}
Remember to use **--rpcuser=user** and **--rpcpass=pass** later when setting up your rpc connection to this node instance.  
{% endhint %}

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

## Node Internal Operations

Upon connecting to the network your node completes the following tasks:

1. Loads the network's block database, indexes, & DAG state.
2. Loads the network's current UTXO set.
3. Creates Websocket RPC connections
4. Loads addresses from DNS seed
5. Listens for new Websocket connections.

The node connect to the network specified at launch, in this case `--devnet`.  The RPC connections are using the port, user, and password specified as well when launching the node.  

### Next Steps

At this point your node is active and you can interact with it directly via [CLI](../interact-with-a-node/node-cli-interface.md) or using [JSON-RPC API](../interact-with-a-node/node-json-rpc-api.md) calls described in the next section.









