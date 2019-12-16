---
description: A RESTful API and MQTT message broker for interacting with the Kaspa network
---

# Kasparov API Server

Inheriting its name from the chess grandmaster, Kasparov is a programmatic interface for interacting with the Kaspa network. The Kaspa network is an inter-connected web of Kapsa nodes, working together to maintain the distributed ledger. Kasparov bridged between the Kaspa network and clients, such as wallets and block explorers. It does so by connecting to a node in the Kaspa network, and exposing a list of RESTful API endpoints, and an MQTT message broker, that allow connected clients to send requests and receive notifications from the Kaspa network. Kasparov allows things like sending a transaction, getting an address balance, listening for updates, and querying for blocks and transactions.

If you are a developer building an application such as a wallet or a block explorer, you have two options:

* You may connect to a community maintained Kasparov API Server. Visit the [community guide](../../community/community-guide/) for more info.
* You may run your own Kasparov API Server. There is a Docker image that you can run to [install Kasparov with all its dependencies](../../try-kaspa/api-server/api-server-setup-using-docker.md). Alternatively, you can setup everything independently.

{% hint style="info" %}
If you want to make JSON-RPC API calls using any authenticated JSON-RPC client without running a dedicated serer please check out[ the JSON-RPC API reference](../kaspa-full-node/rpc-api/).  
{% endhint %}

## 





