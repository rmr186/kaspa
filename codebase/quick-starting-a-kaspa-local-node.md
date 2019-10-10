# Quick Starting a Kaspa Local Node

## Start a local dev network:

`/btcd$ go run ./mining/simulator/... --addresslist=<(printf "`[`127.0.0.1:18334`](http://127.0.0.1:18334)`\n127.0.0.1:18336") --notls/btcd$ sudo chmod + run-dev.sh`

## **Start an node and connect to the network:**

`/btcd$ go run . --dnsseed` [`devnet-dnsseed.daglabs.com`](http://devnet-dnsseed.daglabs.com) `--rpclisten=localhost:18334 --rpcuser=user --rpcpass=pass --devnet --notls --miningaddr=dagtest:qrgufg4qzfqacfzrf6xvx46aqedr5nke3qv6a74fex`

## **Parameters for launching the RPC client:**

 `--dnsseed` [`devnet-dnsseed.daglabs.com`](http://devnet-dnsseed.daglabs.com)  
`--rpclisten=localhost:18334  
--rpcuser=user  
--rpcpass=pass  
--devnet  
--notls--miningaddr=dagtest:qrgufg4qzfqacfzrf6xvx46aqedr5nke3qv6a74fex`

## **Start an RPC client and connect to the network:**

`/btcd/cmd/btcctl$ /btcctl$ go run . --rpcuser=user --rpcpass=pass --rpcserver=localhost:18334 --devnet getPeerInfo  
  
/btcd/cmd/btcctl$ go run ./mining/simulator/... --addresslist=<(printf "\n127.0.0.1:18336") --notls`[`127.0.0.1:18334`](http://127.0.0.1:18334)  
``

`/btcctl$`**`go run . --rpcuser=user --rpcpass=pass --rpcserver=localhost:18334 --devnet -l`**

## Chain Server Commands:

`addManualNode "addr" (onetry=false)  
createRawTransaction [{"txid":"value","vout":n},...] {"address":amount,...} (locktime)  
debugLevel "levelspec"  
decodeRawTransaction "hextx"  
  
---/btcctl$` **`go run . --rpcuser=user --rpcpass=pass --rpcserver=localhost:18334 --devnet getBlockDagInfo`**  
  
`Post` [`https://localhost:18334`](https://localhost:18334)`: http: server gave HTTP response to HTTPS client  
exit status 1---`  
  
  
----------  
`/btcctl$ go run . -h  
Usage:  
  btcctl [OPTIONS]  
  
Application Options:  
  -V, --version       Display version information and exit  
  -l, --listcommands  List all of the supported commands and exit  
  -C, --configfile=   Path to configuration file (default: /home/roni/.btcctl/btcctl.conf)  
  -u, --rpcuser=      RPC username  
  -P, --rpcpass=      RPC password  
  -s, --rpcserver=    RPC server to connect to (default: localhost)  
  -c, --rpccert=      RPC server certificate chain for validation (default: /home/roni/.btcd/rpc.cert)  
      --notls         Disable TLS  
      --proxy=        Connect via SOCKS5 proxy (eg.` [`127.0.0.1:9050`](http://127.0.0.1:9050)``)  
      --proxyuser=    Username for proxy server  
      --proxypass=    Password for proxy server  
      --testnet       Connect to testnet  
      --simnet        Connect to the simulation test network  
      --devnet        Connect to the development test network  
      --skipverify    Do not verify tls certificates (not recommended!)  
  
Help Options:  
  -h, --help          Show this help message  
  
  
The special parameter `-` indicates that a parameter should be read from the  
next unread line from standard input.  
exit status 1``

`----------/btcctl$ go run . --rpcuser=user --rpcpass=pass --rpcserver=localhost:18334 --devnet getPeerInfo  
[]  
  
----------go run . --rpcuser=user --rpcpass=pass --rpcserver=localhost:18334 --devnet getBlockDagInfo  
  
{  
  "dag": "devnet",  
  "blocks": 111636,  
  "headers": 111636,  
  "tipHashes": [  
    "00001c0bd807e66ad7907fb2cc73ae3e1aabce2e20a4d03737b1d0ea718f0344"  
  ],  
  "difficulty": 1.25334631,  
  "medianTime": 1566978717,  
  "utxoCommitment": "6dcda0b0fc8b47e71b1039707f1e07d70297803283cf768b05dd0ca14b084ad7",  
  "pruned": false,  
  "softForks": null,  
  "bip9SoftForks": {  
    "dummy": {  
      "status": "failed",  
      "bit": 28,  
      "startTime": 1199145601,  
      "timeout": 1230767999,  
      "since": 0  
    }  
  }  
}  
  
---------- go run . --rpcuser=user --rpcpass=pass --rpcserver=localhost:18334 --devnet -l  
Chain Server Commands:  
addManualNode "addr" (onetry=false)  
createRawTransaction [{"txid":"value","vout":n},...] {"address":amount,...} (locktime)  
debugLevel "levelspec"  
decodeRawTransaction "hextx"  
decodeScript "hexscript"  
flushDbCache  
generate numblocks  
getAllManualNodesInfo (details=true)  
getBestBlock  
getBestBlockHash  
getBlock "hash" (verbose=true verbosetx=false "subnetwork")  
getBlockCount  
getBlockDagInfo  
getBlockHash index  
getBlockHeader "hash" (verbose=true)  
getBlockTemplate ({"mode":"value","capabilities":["capability",...],"longpollid":"value","sigoplimit":sigoplimit,"masslimit":masslimit,"maxversion":n,"target":"value","data":"value","workid":"value"})  
getCFilter "hash" filtertype  
getCFilterHeader "hash" filtertype  
getChainFromBlock ("starthash" includeblocks)  
getConnectionCount  
getCurrentNet  
getDagTips  
getDifficulty  
getGenerate  
getHashesPerSec  
getHeaders "starthash" "stophash"  
getInfo  
getManualNodeInfo "node" (details=true)  
getMempoolEntry "txid"  
getMempoolInfo  
getMiningInfo  
getNetTotals  
getNetworkHashPs (blocks=120 height=-1)  
getNetworkInfo  
getPeerInfo  
getRawMempool (verbose=false)  
getRawTransaction "txid" (verbose=0)  
getSubnetwork "subnetworkid"  
getTopHeaders ("starthash")  
getTxOut "txid" vout (includemempool=true)  
getTxOutProof ["txid",...] ("blockhash")  
getTxOutSetInfo  
help ("command")  
invalidateBlock "blockhash"  
node "connect|remove|disconnect" "target" ("perm|temp")  
ping  
preciousBlock "blockhash"  
reconsiderBlock "blockhash"  
removeManualNode "addr"  
searchRawTransactions "address" (verbose=true skip=0 count=100 vinextra=false reverse=false ["filteraddr",...])  
sendRawTransaction "hextx" (allowhighfees=false)  
setGenerate generate (genproclimit=-1)  
stop  
submitBlock "hexblock" ({"workid":"value"})  
uptime  
validateAddress "address"  
verifyMessage "address" "signature" "message"  
verifyTxOutProof "proof"  
version`  
  
:~/kaspacoin/btcd$ go run . --dnsseed devnet-dnsseed.daglabs.com --rpclisten=localhost:18334 --rpcuser=user --rpcpass=pass --devnet --notls --miningaddr=dagtest:qrgufg4qzfqacfzrf6xvx46aqedr5nke3qv6a74fex

2019-09-22 11:32:22.553 \[WRN\] CNFG: open /home/roni/.btcd/btcd.conf: no such file or directory 2019-09-22 11:32:22.554 \[INF\] BTCD: Version 0.12.0-beta 

2019-09-22 11:32:22.554 \[INF\] BTCD: Loading block database from '/home/roni/.btcd/data/devnet/blocks\_ffldb' 2019-09-22 11:32:22.567 \[INF\] BTCD: Block database loaded 2019-09-22 11:32:22.569 \[INF\] CHAN: Loading block index... 

2019-09-22 11:32:23.454 \[INF\] CHAN:Start a local dev network:

`/btcd$ go run ./mining/simulator/... --addresslist=<(printf "`[`127.0.0.1:18334`](http://127.0.0.1:18334/)`\n127.0.0.1:18336") --notls/btcd$ sudo chmod + run-dev.sh`

## **Start an node and connect to the network:** <a id="start-an-node-and-connect-to-the-network"></a>

`/btcd$ go run . --dnsseed` [`devnet-dnsseed.daglabs.com`](http://devnet-dnsseed.daglabs.com/) `--rpclisten=localhost:18334 --rpcuser=user --rpcpass=pass --devnet --notls --miningaddr=dagtest:qrgufg4qzfqacfzrf6xvx46aqedr5nke3qv6a74fex`

## **Parameters for launching the RPC client:** <a id="parameters-for-launching-the-rpc-client"></a>

 `--dnsseed` [`devnet-dnsseed.daglabs.com`](http://devnet-dnsseed.daglabs.com/) `--rpclisten=localhost:18334 --rpcuser=user --rpcpass=pass --devnet --notls--miningaddr=dagtest:qrgufg4qzfqacfzrf6xvx46aqedr5nke3qv6a74fex`

## **Start an RPC client and connect to the network:** <a id="start-an-rpc-client-and-connect-to-the-network"></a>

`/btcd/cmd/btcctl$ /btcctl$ go run . --rpcuser=user --rpcpass=pass --rpcserver=localhost:18334 --devnet getPeerInfo /btcd/cmd/btcctl$ go run ./mining/simulator/... --addresslist=<(printf "\n127.0.0.1:18336") --notls`[`127.0.0.1:18334`](http://127.0.0.1:18334/)

`/btcctl$`**`go run . --rpcuser=user --rpcpass=pass --rpcserver=localhost:18334 --devnet -l`**

## Chain Server Commands: <a id="chain-server-commands"></a>

`addManualNode "addr" (onetry=false) createRawTransaction [{"txid":"value","vout":n},...] {"address":amount,...} (locktime) debugLevel "levelspec" decodeRawTransaction "hextx" ---/btcctl$` **`go run . --rpcuser=user --rpcpass=pass --rpcserver=localhost:18334 --devnet getBlockDagInfo`** `Post` [`https://localhost:18334`](https://localhost:18334/)`: http: server gave HTTP response to HTTPS client exit status 1---` ---------- `/btcctl$ go run . -h Usage: btcctl [OPTIONS] Application Options: -V, --version Display version information and exit -l, --listcommands List all of the supported commands and exit -C, --configfile= Path to configuration file (default: /home/roni/.btcctl/btcctl.conf) -u, --rpcuser= RPC username -P, --rpcpass= RPC password -s, --rpcserver= RPC server to connect to (default: localhost) -c, --rpccert= RPC server certificate chain for validation (default: /home/roni/.btcd/rpc.cert) --notls Disable TLS --proxy= Connect via SOCKS5 proxy (eg.` [`127.0.0.1:9050`](http://127.0.0.1:9050/)``) --proxyuser= Username for proxy server --proxypass= Password for proxy server --testnet Connect to testnet --simnet Connect to the simulation test network --devnet Connect to the development test network --skipverify Do not verify tls certificates (not recommended!) Help Options: -h, --help Show this help message The special parameter `-` indicates that a parameter should be read from the next unread line from standard input. exit status 1``

`----------/btcctl$ go run . --rpcuser=user --rpcpass=pass --rpcserver=localhost:18334 --devnet getPeerInfo [] ----------go run . --rpcuser=user --rpcpass=pass --rpcserver=localhost:18334 --devnet getBlockDagInfo { "dag": "devnet", "blocks": 111636, "headers": 111636, "tipHashes": [ "00001c0bd807e66ad7907fb2cc73ae3e1aabce2e20a4d03737b1d0ea718f0344" ], "difficulty": 1.25334631, "medianTime": 1566978717, "utxoCommitment": "6dcda0b0fc8b47e71b1039707f1e07d70297803283cf768b05dd0ca14b084ad7", "pruned": false, "softForks": null, "bip9SoftForks": { "dummy": { "status": "failed", "bit": 28, "startTime": 1199145601, "timeout": 1230767999, "since": 0 } } } ---------- go run . --rpcuser=user --rpcpass=pass --rpcserver=localhost:18334 --devnet -l Chain Server Commands: addManualNode "addr" (onetry=false) createRawTransaction [{"txid":"value","vout":n},...] {"address":amount,...} (locktime) debugLevel "levelspec" decodeRawTransaction "hextx" decodeScript "hexscript" flushDbCache generate numblocks getAllManualNodesInfo (details=true) getBestBlock getBestBlockHash getBlock "hash" (verbose=true verbosetx=false "subnetwork") getBlockCount getBlockDagInfo getBlockHash index getBlockHeader "hash" (verbose=true) getBlockTemplate ({"mode":"value","capabilities":["capability",...],"longpollid":"value","sigoplimit":sigoplimit,"masslimit":masslimit,"maxversion":n,"target":"value","data":"value","workid":"value"}) getCFilter "hash" filtertype getCFilterHeader "hash" filtertype getChainFromBlock ("starthash" includeblocks) getConnectionCount getCurrentNet getDagTips getDifficulty getGenerate getHashesPerSec getHeaders "starthash" "stophash" getInfo getManualNodeInfo "node" (details=true) getMempoolEntry "txid" getMempoolInfo getMiningInfo getNetTotals getNetworkHashPs (blocks=120 height=-1) getNetworkInfo getPeerInfo getRawMempool (verbose=false) getRawTransaction "txid" (verbose=0) getSubnetwork "subnetworkid" getTopHeaders ("starthash") getTxOut "txid" vout (includemempool=true) getTxOutProof ["txid",...] ("blockhash") getTxOutSetInfo help ("command") invalidateBlock "blockhash" node "connect|remove|disconnect" "target" ("perm|temp") ping preciousBlock "blockhash" reconsiderBlock "blockhash" removeManualNode "addr" searchRawTransactions "address" (verbose=true skip=0 count=100 vinextra=false reverse=false ["filteraddr",...]) sendRawTransaction "hextx" (allowhighfees=false) setGenerate generate (genproclimit=-1) stop submitBlock "hexblock" ({"workid":"value"}) uptime validateAddress "address" verifyMessage "address" "signature" "message" verifyTxOutProof "proof" version` :~/kaspacoin/btcd$ go run . --dnsseed devnet-dnsseed.daglabs.com --rpclisten=localhost:18334 --rpcuser=user --rpcpass=pass --devnet --notls --miningaddr=dagtest:qrgufg4qzfqacfzrf6xvx46aqedr5nke3qv6a74fex

2019-09-22 11:32:22.553 \[WRN\] CNFG: open /home/roni/.btcd/btcd.conf: no such file or directory 2019-09-22 11:32:22.554 \[INF\] BTCD: Version 0.12.0-beta

2019-09-22 11:32:22.554 \[INF\] BTCD: Loading block database from '/home/roni/.btcd/data/devnet/blocks\_ffldb' 2019-09-22 11:32:22.567 \[INF\] BTCD: Block database loaded 2019-09-22 11:32:22.569 \[INF\] CHAN: Loading block index...

2019-09-22 11:32:23.454 \[INF\] CHAN: Loading UTXO set... 2019-09-22 11:32:32.739 \[INF\] CHAN: DAG state \(chain height 110635, hash 00001c0bd807e66ad7907fb2cc73ae3e1aabce2e20a4d03737b1d0ea718f0344\)

2019-09-22 11:32:32.745 \[INF\] RPCS: RPC server listening on 127.0.0.1:18334 2019-09-22 11:32:32.748 \[INF\] ADXR: Loaded 11 addresses from file '/home/roni/.btcd/data/devnet/peers.json' 2019-09-22 11:32:32.748 \[INF\] CMGR: Server listening on 0.0.0.0:18333 2019-09-22 11:32:32.748 \[INF\] CMGR: Server listening on \[::\]:18333 2019-09-22 11:32:33.121 \[INF\] CMGR: 3 addresses found from DNS seed n.devnet-dnsseed.daglabs.com

`roni@ori-OptiPlex-7060:~/kaspacoin/btcd$ cd cmd roni@ori-OptiPlex-7060:~/cd btcctl`

`kaspacoin/btcd/cmd$ ls -l total 32 drwxrwxr-x 2 roni roni 4096 Sep 22 11:20 addblock drwxrwxr-x 2 roni roni 4096 Sep 22 11:20 addsubnetwork drwxrwxr-x 2 roni roni 4096 Sep 22 11:20 btcctl drwxrwxr-x 2 roni roni 4096 Sep 22 11:20 findcheckpoint drwxrwxr-x 2 roni roni 4096 Sep 22 11:20 genaddr drwxrwxr-x 2 roni roni 4096 Sep 22 11:20 gencerts drwxrwxr-x 2 roni roni 4096 Sep 22 11:20 genesis drwxrwxr-x 3 roni roni 4096 Sep 22 11:20 txgen roni@ori-OptiPlex-7060:~/kaspacoin/btcd/cmd$` Loading UTXO set... 2019-09-22 11:32:32.739 \[INF\] CHAN: DAG state \(chain height 110635, hash 00001c0bd807e66ad7907fb2cc73ae3e1aabce2e20a4d03737b1d0ea718f0344\) 

2019-09-22 11:32:32.745 \[INF\] RPCS: RPC server listening on 127.0.0.1:18334 2019-09-22 11:32:32.748 \[INF\] ADXR: Loaded 11 addresses from file '/home/roni/.btcd/data/devnet/peers.json' 2019-09-22 11:32:32.748 \[INF\] CMGR: Server listening on 0.0.0.0:18333 2019-09-22 11:32:32.748 \[INF\] CMGR: Server listening on \[::\]:18333 2019-09-22 11:32:33.121 \[INF\] CMGR: 3 addresses found from DNS seed n.devnet-dnsseed.daglabs.com

`roni@ori-OptiPlex-7060:~/kaspacoin/btcd$ cd cmd roni@ori-OptiPlex-7060:~/cd btcctl`

`kaspacoin/btcd/cmd$ ls -l total 32 drwxrwxr-x 2 roni roni 4096 Sep 22 11:20 addblock drwxrwxr-x 2 roni roni 4096 Sep 22 11:20 addsubnetwork drwxrwxr-x 2 roni roni 4096 Sep 22 11:20 btcctl drwxrwxr-x 2 roni roni 4096 Sep 22 11:20 findcheckpoint drwxrwxr-x 2 roni roni 4096 Sep 22 11:20 genaddr drwxrwxr-x 2 roni roni 4096 Sep 22 11:20 gencerts drwxrwxr-x 2 roni roni 4096 Sep 22 11:20 genesis drwxrwxr-x 3 roni roni 4096 Sep 22 11:20 txgen roni@ori-OptiPlex-7060:~/kaspacoin/btcd/cmd$`

  

