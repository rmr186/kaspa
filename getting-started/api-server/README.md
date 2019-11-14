# API Server

The API Server is a programmatic interface for interacting with the Kaspa network. The Kaspa network consists of inter-connected Kapsa full nodes. The API Server connects a Kaspa full node, connected to the Kaspa network, and exposes a list of RESTful API endpoints that allow interacting with the network. This allows clients, such as wallets and block explorer, connected to the API Server to easily interact with the Kaspa network. The API includes endpoints for sending a transaction, getting an address balance, listening for updates, and querying for blocks and transactions.

If you are a developer building an application such as a wallet or a block explorer, you may connect to one of the community maintained API servers or you can set up your own server. If you choose to set up your own API server, you will also need to connect it to an a full node you have access to. If you do not have access to one, you can run one yourself and connect it to the Kaspa network.

If you want to extend the functionality of the API Server, or fork it, visit the GitHub.

* For a list of RESTful API calls see the [API Server reference](./).
* For setting up your own server, you can either use the [API Server Docker](../running-a-node/full-node-quick-setup-with-docker.md) or [compile from source](run-an-api-server-docker-container.md).
* For contributing to the API Server development, visit GitHub.

