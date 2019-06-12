# Use Cases, Scenario’s, Walkthroughts - for regtest

[back to main page](README.md)

## Case "Start Regtest"

**Scenario**: Testing a new feature or a new on-chain application

**Feature**: The Regtest network allows users to mine blocks on demand

**Actor**: A regtest network user

**Summary**:

The Regtest network allows a user to create its own local network. Nodes that are started in regtest mode do not automatically connect to
other nodes. Nodes also do not automatically create new blocks. Instead, blocks are generated (both in the PoW and in the PoS phase) using
the command `generate [nr_of_blocks]`.
The ION Regtest network is configured to start with 250 blocks in the PoW phase and then switch to PoS.

**Preconditions**: 

- The ion client is installed
- The ion client is configured

**Steps**: 

To start a node in regtest mode, the node needs to be started with the flag `-regtest=1`. Also, a node cannot both have `-regtest=1` and `testnet=1`,
so one needs to be sure that `ioncoin.conf` does not have `testnet=1` set.
Additionally, when using `ion-cli`, the same parameter `-regtest=1` needs to be passed.
This parameter can be passed either on the command line or through editing `ioncoin.conf`.

The following commands should start a daemon on the Regtest network:

```
# start daemon in regtest modus, ensure the chain and transaction history are reset, enable debug logging, enable background mode
$ iond -regtest=1 -resync=1 -zapwallettxes=1 -debug=1 -daemon=1
ION server starting

# view the chain status
$ ion-cli -regtest=1 getinfo
{
  "version": 3029900,
  "protocolversion": 95704,
  "services": "NETWORK/BLOOM/",
  "walletversion": 61000,
  "balance": 0.00000000,
  "zerocoinbalance": 0.00000000,
  "blocks": 0,
  "timeoffset": 0,
  "connections": 4,
  "proxy": "",
  "difficulty": 0.000244140625,
  "testnet": true,
  "moneysupply": 0.00000000,
  "xIONsupply": {
    "1": 0.00000000,
    "5": 0.00000000,
    "10": 0.00000000,
    "50": 0.00000000,
    "100": 0.00000000,
    "500": 0.00000000,
    "1000": 0.00000000,
    "5000": 0.00000000,
    "total": 0.00000000
  },
  "keypoololdest": 1545340622,
  "keypoolsize": 1001,
  "paytxfee": 0.00000000,
  "relayfee": 0.00010000,
  "staking status": "Staking Not Active",
  "errors": "This is a pre-release test build - use at your own risk - do not use for staking or merchant applications!"
}

# stop the ion regtest daemon
$ ion-cli -regtest=1 stop
ION server stopping

# start daemon in regtest modus, use existing chain if it exists, enable debug logging, enable background mode
$ iond -regtest=1 -debug=1 -daemon=1
ION server starting

# Mine 260 blocks on Regtest, bringing the chain to the PoS phase.
$ ion-cli -regtest=1 generate 260

```

* The same can be achieved by running the ion Qt wallet.
* The parameter `-regtest=1` can be moved to ioncoin.conf as `regtest=1`.

**Postconditions**:

* A running Regtest node.
* A chain with 260 blocks.
  * Entered PoS phase
  * Moved past the TokenStart block height 

**Related use cases**:

- Case "Install and configure new client"