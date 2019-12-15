---
description: A RESTful API for interacting with the Kaspa network
---

# Kasparov API Server

The API Server is a programmatic interface for interacting with the Kaspa network. The Kaspa network is an inter-connected web of Kapsa full nodes, working together to maintain the distributed ledger. The API Server connects to one of the Kaspa full node, and exposes a list of RESTful API endpoints, that allow clients, such as wallets and block explorer, connected to the API Server, to send requests and receive notifications from the Kaspa network. The API allows things like sending a transaction, getting an address balance, listening for updates, and querying for blocks and transactions.

If you are a developer building an application such as a wallet or a block explorer, you have two options:

* You may connect to a community maintained API Server. Visit the [community guide](../../community/community-guide/) for more info.
* You may run your own API Server. There is a Docker image that you can run to [install an API Server with all its dependencies](api-server-setup-using-docker.md). Alternatively, you can setup everything independently.

Once you have access to an API Server, you can start interacting with the Kaspa network.

* For a list of RESTful API calls the API Server supports, see the [API Server reference](./).
* You can download the command line wallet to create a wallet and send your first transaction.

For contributing to the API Server development, visit [GitHub](https://github.com/daglabs).

