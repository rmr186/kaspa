---
description: Interact with your node using JSON requests
---

# JSON-RPC API

The full node includes an rpc server configured to listen on a specific port for JSON-RPC requests from your client application.

To get started, first ensure you have a full node running, connected to --devnet, and listening for rpc requests.  You can find this information on the node's command line terminal output:

![full-node Terminal output](../../.gitbook/assets/full-node-terminal-output.png)

The JSON-RPC API allows your client application to securely interact with your node, allowing you to do things like generate blocks, verify blocks and transactions, and publish transactions to the network.

![Kaspa JSON-RPC Architecture](../../.gitbook/assets/architecture-v1.png)

### Authentication

All API requests are being served by your local rpc connection. The user credentials for your rpc connection were defined in the node command options:

```bash
$ ./btcd --devnet --rpclisten=localhost:18334 --rpcuser=user --rpcpass=pass --notls --acceptanceindex --txind
```

### JSON-RPC API Reference

For a list of available API requests and their format please see the [JSON-RPC API Refrence](../../api-reference/rpc-api-ref/).









