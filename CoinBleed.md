# CoinBleed analysis and fix

[back to main page](README.md)

## Analysis
ION is a PoS coin with masternodes, based on Peercoin, Transfercoin and various other coins. 
At date [date], unusual patterns in stake production where observed, which was soon identified as an attack with the following characteristics:

 - Blocks produced in the past were accepted
 - Difficulty dropped
 - Several blocks were submitted and accepted from low-coin and low-coinage wallets

## Compare to other coins 
Various other coins include fixes to parts of the block validation process that haven't been included in ION Coin. Such as:
- Orbitcoin allows a smaller window for blocks from the past
- Stratis allows a smaller window for blocks from the future
- The MIDAS algorithm is less susceptible to difficulty manipulation than the ION Coin difficulty algo.

Blocks from the future
### Consensus time
The clients accepted blocks both too far in the future and too far in the past. There are various checks possible on blocks and on transactions to prevent such blocks to be included. Although some of these checks were implemented, there are additional checks possible, and the checks can be implemented more effectively. 
To prevent clients from lying about the time a transaction is offered, or the time a block has been submitted, the submitted time should be compared to the actual time.

The challenge then is to determine:
- How much a client's time is allowed to deviate from the actual time
- What is defined as the actual time

Ioncoin used to allow large deviations from the actual time; in fact, the bitcoin standard values are used. However, bitcoin has a blocktime of 10 minutes, and a very robust miner pool, while Ioncoin has a blocktime of 64 seconds and a developing network of nodes: if there's a good reason, the time window for accepting block can be reconsidered.
This drift window is addressed in Stratis' fix below.

Additionally, the block (or transaction) should not be in the past. The median time of the last 11 reported blocks will from now on be used to check if the block isn't too far from the past. The check for this time takes place in the [CheckBlock() function](https://github.com/cevap/ion/blob/midas-algo/src/main.cpp#L2675-L2678).

An introduction to the underlying [GetMedianPastTime()](https://github.com/cevap/ion/blob/midas-algo/src/main.h#L1039-L1051) function can be found in the bitcoin developers mailing list for the corresponding [BIP: Using Mediam time-past as endpoint for locktime calculations](https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2015-August/010348.html). Note that we are not using this function to verify if transactions should be accepted, but to verify if blocks can be accepted.

### Stratis' fixes
We will incorporate fixes from the Stratis code that prevent blocks from the future to be submitted:

- [Commit "leave old fork behind"](https://github.com/stratisproject/stratisX/commit/b2bacb4929b76b87fc2543b57155229cbd350096)
- [Commit "hardfork w/ better syncing"](https://github.com/stratisproject/stratisX/commit/b80d3ea6442d26d9ccae5174b99f52f64af97d8f)
- [Commit "hardfork"](https://github.com/stratisproject/stratisX/commit/d2c9e5f1bfe91e0b7fa1aa1d8d1de8f831c7318f)
- [Commit "Set hardfork date"](https://github.com/stratisproject/stratisX/commit/271c5a10732abc3568464c0861e2fe3a75262a5b)

In Stratis, the following files are updated for a hardfork that addressed a comparable hack:
- src/main.cpp
    - Better comparison of GetBlockTime() to FutureDrift() 
        - [here](https://github.com/stratisproject/stratisX/blob/master/src/main.cpp#L1960)
        - [here](https://github.com/stratisproject/stratisX/blob/master/src/main.cpp#L2055)
        - [here](https://github.com/stratisproject/stratisX/blob/master/src/main.cpp#L2066)
- src/main.h
    - Updated FutureDrift to be dependent on both nTime (timestamp of the node receiving the block) and nHeight (current block height) [here](https://github.com/stratisproject/stratisX/blob/master/src/main.h#L62)
- src/checkpoints.cpp
    - Added a checkpoint to mark the block of the fork: [here](https://github.com/stratisproject/stratisX/blob/master/src/checkpoints.cpp#L28)
    - Later, added a checkpoint to mark the blocks after the fork [here](https://github.com/stratisproject/stratisX/blob/master/src/checkpoints.cpp#L38)

Additionally, Stratis incorporates checks to make sure some transactions are not propagated; in particular, transactions that cannot be included in the next block should not be propagated to prevent specific double-spend and DoS attacks; this code can be found [here](https://github.com/stratisproject/stratisX/blob/master/src/main.cpp#L276)

### Discussion
FutureDrift () ideally selects the strictest conditions based on blockheight and blocktime: if one of both is after the forking point, the recent, more strict condition should apply.
The Stratis implementation seems to select the least strict condition: if nHeight is manipulated to be before the fork, the 'old' drift values are selected; and if the nHeight is after the fork, but the nTime is manipulated to be low, the 'old' drift values are selected.

### Adapting Stratis' fixes
In Stratis, Testnet was restarted with the updated FutureDriftV2(), while in ION Coin, we used Testnet to simulate the fork where we choose a block height (78800) to switch from ProtocolV1 (up to and including block Fork1Height) to ProtocolV2 (after block Fork1Height)
This enables us to simplify the FutureDrift() functions.

The updated FutureDrift() functions are [here](https://github.com/cevap/ion/blob/midas-algo/src/main.h#L63-71):

```
inline bool IsProtocolV1(int nHeight){ return nHeight <= Params().Fork1Height(); }
inline bool IsProtocolV2(int nHeight){ return nHeight > Params().Fork1Height(); }  

inline bool IsDriftReduced(int64_t nTime) { nTime > Params().Fork1Time; } // Drifting Bug Fix, hardfork on Thursday, June 15, 2017 3:41:20 PM GMT

inline int64_t FutureDriftV1(int64_t nTime) { return nTime + 16200; }
inline int64_t FutureDriftV2(int64_t nTime) { return nTime + 15; }

inline int64_t FutureDrift(int64_t nTime, int nHeight) { return IsProtocolV2(nHeight) ? FutureDriftV2(nTime) : FutureDriftV1(nTime); }
```

To turn this into more strict checking, the Stratis based approach has been updated to the following:
```
inline bool IsProtocolV1(int nHeight){ return nHeight <= Params().Fork1Height(); }
inline bool IsProtocolV2(int nHeight){ return nHeight > Params().Fork1Height(); }  

inline bool IsDriftReduced(int64_t nTime) { return nTime > Params().Fork1Time(); } // Drifting Bug Fix, hardfork on Thursday, June 15, 2017 3:41:20 PM GMT

inline int64_t FutureDriftV2(int64_t nTime) { return nTime + 15; }
inline int64_t FutureDriftV1(int64_t nTime) { return IsDriftReduced(nTime) ? nTime + 16200 : FutureDriftV2(nTime); }

inline int64_t FutureDrift(int64_t nTime, int nHeight) { return IsProtocolV1(nHeight) ? FutureDriftV1(nTime) : FutureDriftV2(nTime); }
```

In main.cpp, the following changes need to be made:
- [x] Update calls to FutureDrift to include nHeight (in addition to nTime)
- [x] Change the FutureDrift() function to return the 'old' drift values only if both the block time and the block height are in the past.
- [ ]  Figure out what to do with GetPastTImeLimit()
    - ```if (GetBlockTime() <= pindexPrev->GetPastTimeLimit() || FutureDrift(GetBlockTime(), nHeight) < pindexPrev->GetBlockTime())```
        

# TODO
- [ ] Replace the links to the master repo with links to the lines in the commits? Find better way of showing the source?
- [ ] Document implementation of the Midas algo
- [ ] Document implementation of past block checks from Orbitcoin
- [ ] Assess if GetMedianTimePast() should also be used to verify if transactions can be included in a block.