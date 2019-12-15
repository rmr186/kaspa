---
description: Spec > Differences from Bitcoin
---

# Differences from Bitcoin



1. ‌DAG
2. Phantom
3. UTXO-set maintenance, diff, etc.
4. No Built-in Wallet and Wallet-related APIs
5. API-Server
6. The way fees are paid
7. The way confirmations are counted
8. Netsync procedure
9. Schnorr
10. No Segwit
11. Malleability fix through TxID + AcceptedIDMerkleRoot
12. Only P2PKH and P2SH are standard
13. ECMH utxo-commitments
14. Subnetworks, gas, payloads, etc.
15. Coinbase transaction structure
16. Bech32 addresses \(similar to cashaddr\)
17. Changes in SCRIPT:
    1. No multisig dummy
    2. Remove OP\_CODESEPARATOR
    3. Remove FindAndDelete functionality
    4. SigHashSingle errors on wrong index
    5. Minimum-if-policy as consensus rule \(in BTC is only for Segwit txs\)
    6. \[Q: Does the above include requirements for bridge to Layer2?\]
18. Disable chained transactions
19. Difficulty adjustment algorithm
20. Nonce is uint64
21. Timestamp is int64
22. TxOut.Value is uint64
23. Finality
24. Transaction/Block mass
25. Full node
26. Pruning
27. Archival nodes

