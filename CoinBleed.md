# CoinBleed analysis and fix

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
### Stratis
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

