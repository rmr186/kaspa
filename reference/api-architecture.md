# Components

## Kaspa P2P Network

The Kaspa network is a P2P web of inter-connected nodes, working together to maintain the distributed Kaspa ledger.

### A Kaspa Node

If you already know how a Bitcoin node works, you can skip this part.

A Kaspa node is a single participant in the Kaspa network. Each Kaspa node connects to a small subset of all the other nodes in the network. Nodes perform many kinds of operations, the most important operations are data exchange, data validation, and mining.

#### Data Exchange

Since each node is not connected to all other nodes, but only to a small subset of them, and since new data can originate on any node in the network, nodes need to pass messages between themselves from one node to another and so on, using what is referred to as the "gossip protocol" to make sure information such as new blocks and transactions propagate through the entire network and reach all nodes quickly.

#### Data Validation

The Kaspa network implements the Kaspa social contract - a set of rules agreed upon all participating nodes. Nodes do not need to rely on the honesty of other nodes. Instead, each node can validate all new data that it receives. If it receives data that it cannot validate, it rejects this data. If a malicious node keeps spreading invalid data, honest nodes can choose to disconnect from it \(out-casting it\).

#### Mining

Each node is also capable of mining Kaspa, or in other words, expending computational resources for a chance to find a new block and its attached reward of Kaspa coins. Mining is not mandatory, but optional.

JSON-RPC

CLI

## Kasparov API Server

Kasparov is an API server for interacting with the Kaspa network. While the Kaspa node handles communications internal to the Kaswpa P2P network, the Kasparov API server handles communications of the P2P network with the outside world.

It was designed as a separate component, with the thought to decouple these two distinct sets of operations. A Kaspa node is very good at communicating with other nodes and does that very fast. It was not designed to query and serve large volumes of data quickly.

Kasparov API server is able to serve blocks and transactions data quickly and at large volumes, by saving the data in a way that allows quick and efficient polling.

The Kasparov API Server is comprised of two components: a data connection component and an interface component. These components were separated because while each Kasparov API Server needs one Data Connection to the Kaspa network, there may be many separate interface instances needed, depending on the number of clients needed to serve.

### Data Connection

The data connection component is responsible for data synchronization between the Kaspa network and the Kasparov API server. It uses a Kaspa node to get new blocks and transactions and store them in its database for quick querying. It sends new transactions to the Kaspa network by relaying them to the through the Kaspa node.

### Interface

The interface component is comprised of two sub-components: a RESTful API and an MQTT message broker.

#### RESTful API

This is a RESTful API providing endpoints for sending http requests such as querying for blocks, transactions, addresses and for sending transactions to the network.

#### MQTT API

This is an MQTT pub/sub message broker implemented using RabbitMQ for subscribing to topics and receiving notifications, such as new blocks and transaction updates.

