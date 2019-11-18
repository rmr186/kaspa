# Introduction

## Kaspa Overview

Kaspa is a blockDAG decentralized ledger network using proof-of-work consensus & trust-less VM layers.  Kaspa's code repository is open-source, and network governance is dictated in the consensus algorithm.

This quick-start guide contains everything you need to launch a pre-configured node and interact with the Kaspa Testnet using CLI, WebRPC, and Data API Server.

After going through this guide you should have a good feel for how Kaspa works and where to go to learn more about the workings of a node & and the rest of the Kaspa codebase.

## Quick-Starting a Kaspa Node from Docker Image

In order to get started quickly, you can download and run the following Docker image containing a pre-configured Kaspa node and the  necessaries tools for working with it.

### Pre-Requisites

All you need to have running on your machine is Docker, and you should be able to run basic bash commands using a CLI.

### Download & Run the Docker Container

The following command downloads and installs the Docker image for a pre-configured Kaspa full node:   

```bash
:~/code$ docker run --name kaka -p 42069:3306 -e MYSQL_ROOT_PASSWORD=pass mysql
```

{% hint style="info" %}
You can download and learn more about using Docker [here](https://hub.docker.com/).
{% endhint %}

After launching the Docker image, run the following code in a separate terminal to start the full-node within the Docker environment:

```bash
:~/code$ docker exec -it kaka bash
```

Run the following command to verify successful node installation:

```text

```

## Basic Node Operations using Kaspa CLI

## Connect to Node using Websocket RPC 

## Deploying Kaspa Data API Server from Docker Image

## Querying the Kaspa BlockDAG using JSON-RPC

## Using the Kaspa CLI-Wallet

## Get Started Mining using _getwork_ RPC Method

## Deploying Kaspa-Explorer from Docker Image

## Join the Community

