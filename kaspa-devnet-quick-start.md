# Running a local Kaspa node using Docker

Lucky for you we made a handy little Docker container that actually does a lot:

1. Compiles & runs the Kaspa code creating 2 distinct Kaspa Nodes.
2. Connects the 2 Kaspa nodes forming a local Kaspa network .

Follow these instructions to download and run the Kaspa Devnet container:

### Clone the Kaspa Github \(btcd for now\) repository locally

```text
roni@ori-OptiPlex-7060:~/kaspacoin$ git clone https://github.com/Moshiach770/btcd
Cloning into 'btcd'...
remote: Enumerating objects: 16341, done.
remote: Total 16341 (delta 0), reused 0 (delta 0), pack-reused 16341
Receiving objects: 100% (16341/16341), 15.66 MiB | 1.88 MiB/s, done.
Resolving deltas: 100% (11487/11487), done.
roni@ori-OptiPlex-7060:~/kaspacoin$
```

### Locally run the Kaspa DevNet's Docker shell script:

{% code-tabs %}
{% code-tabs-item title="run-dev.sh" %}
```bash
roni@ori:/usr/local/btcd$ sudo chmod + run-dev.sh
[sudo] password for roni: 
roni@ori:/usr/local/btcd$ ls
addrmgr    btcec    config     deploy.sh  docs        integration  logger   mining     release           server              test.sh     version
apiserver  btcjson  connmgr    dnsseeder  goclean.sh  Jenkinsfile  log.go   netsync    rpcclient         service_windows.go  txscript    wire
blockdag   CHANGES  dagconfig  doc.go     go.mod      LICENSE      logs     peer       run-dev.sh        signal              upgrade.go
btcd.go    cmd      database   docker     go.sum      limits       mempool  README.md  sample-btcd.conf  telegram.sh         util
roni@ori:/usr/local/btcd$ go run . --help
Usage:
  btcd [OPTIONS]
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Discover basic network info using the **'go run'** command as follows:

{% code-tabs %}
{% code-tabs-item title="run-dev.sh" %}
```bash
roni@ori:/usr/local/btcd$ go run . --dnsseed devnet-dnsseed.daglabs.com --devnet
Error creating a default config file: open /tmp/go-build417271827/b001/exe/sample-btcd.conf: no such file or directory
2019-09-18 15:26:36.485 [INF] CNFG: RPC service is disabled
2019-09-18 15:26:36.485 [WRN] CNFG: open /home/roni/.btcd/btcd.conf: no such file or directory
2019-09-18 15:26:36.485 [INF] BTCD: Version 0.12.0-beta
2019-09-18 15:26:36.485 [INF] BTCD: Loading block database from '/home/roni/.btcd/data/devnet/blocks_ffldb'
2019-09-18 15:26:36.497 [INF] BTCD: Block database loaded
2019-09-18 15:26:36.498 [INF] CHAN: Loading block index...
2019-09-18 15:26:37.208 [INF] CHAN: Loading UTXO set...
2019-09-18 15:26:46.239 [INF] CHAN: DAG state (chain height 110635, hash 00001c0bd807e66ad7907fb2cc73ae3e1aabce2e20a4d03737b1d0ea718f0344)
2019-09-18 15:26:46.248 [INF] ADXR: Loaded 6 addresses from file '/home/roni/.btcd/data/devnet/peers.json'
2019-09-18 15:26:46.248 [INF] CMGR: Server listening on 0.0.0.0:18333
2019-09-18 15:26:46.248 [INF] CMGR: Server listening on [::]:18333
2019-09-18 15:26:46.373 [INF] CMGR: 2 addresses found from DNS seed n.devnet-dnsseed.daglabs.com
```
{% endcode-tabs-item %}
{% endcode-tabs %}

How cool is that?

