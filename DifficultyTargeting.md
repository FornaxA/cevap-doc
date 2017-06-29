

# Difficulty targeting

[back to main page](README.md)

When addressing the time warp attack, Ioncoin adopted a difficulty targeting algo that was readily available, easy to implement and worked well on the testnet.
For the next step in the development, we'll investigate adopting a difficulty targeting algo with a more smooth pattern.

## Midas shortcomings

Pro:
- More stable than original Ioncoin difficulty algorithm
- Enables addressing timewarp attack
- Calender function enables approaching average block time as specified in whitepaper

Con:
- Large swings in difficulty, resulting in block times between a few seconds and 35 minutes

## Discussions in other coins

- [Zcash github discussion on difficulty adjustment algorithms](https://github.com/zcash/zcash/issues/147)
- [Auroracoin, timewarp exploit and Kimoto's Gravity Well](https://bitcointalk.org/index.php?topic=552895)
- [Dash and Dark Gravity Wave implementation on github](https://github.com/dashpay/dash/blob/master/src/pow.cpp#L82)

Overview:
[MPOS overview](https://github.com/MPOS/php-mpos/issues/2403#issuecomment-93710766)

## Time Warp Attack explained

[Litcoin's intro in the Time Warp bug](https://litecoin.info/Time_warp_attack)

## Ioncoin specific characteristics

The selection, adjustment or creation of a difficulty adjustment algorithm for Ioncoin should take into account Ioncoin specific characteristics.
Discussions in other coins address:
- Is the coin PoS or Pow?
- Are there multipools?
- Does the hash rate change often?
- Fast or slow block times?

Ioncoin is a PoS coin, where the majority of the staking coins are in a small number of large and stable wallets.
The combination of masternodes and staking rewards promotes keeping coins in wallets.
Network stake weight is not expected to change rapidly - at least not in the short term.

That means that in theory low changes in difficulty should be needed.

During the coming months, the chain will slowly catch up with the specified average block time. After that, an algo should be adopted that matches the above characteristics.