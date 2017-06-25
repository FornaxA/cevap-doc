# Initial block download sometimes gets stuck

[back to main page](README.md)

When start a new instance of the ioncoin client, it needs to perform an initial block download: synchronizing the chain. During sync, certain functionality is unavailable (such as masternode functions). Also, the aim of this process is to get the synchronization finished as quickly as possible.

Several users have reported the sync process to get stalled at random block heights. The context has not been narrowed down yet.

## Steps to reproduce

1. Set up a clean datadir
2. Run iond or ion-qt
3. Wait for sync to either finish or stall

The sync is more likely to stall when multiple clients are running simultaneously. It is possible but not certain that this indicates:
- memory limitations (in code)
- multithreading issues
- higher chance of issues because the sync takes longer

## Log files

Log files can be located on the community repo: 
- [Logfile 1](https://github.com/cevap/ion/blob/midas-algo/bin/debug.log-errors/debug-stuck-with-addnode.tar.xz.gpg)
- [Logfile 2](https://github.com/cevap/ion/blob/midas-algo/bin/debug.log-errors/debug.tar.xz.gpg)

The log files indicate correlation between the syncing problem and the logging of supposed ORPHAN blocks. The client processes an increasing number of blocks as orphans, until the process stalls. The blocks are not actually orphans: they are blocks that have been received early.

More specifically, the log files indicate two types of orphans:
1. Blocks that should only be processed after the inital sync has been completed (so, actually NEW blocks that have been produced after the initial download started)
2. Blocks several thousand blocks ahead of the last block that has been processed by the sync process

The sync process stalls when it arrived at the block height of such an orphan.

### First case

About the sync below:
- the sync started with an empty chain
- the first orphan (with prev height 179213) was received when the sync was at height 12608
- the second orphan (with prev height 15000) was received when the sync was at height 12680
- the sync stalled at block 14999

Log snippets:

```
2017-06-24 13:56:06 SetBestChain: new best=c5cd1e61af02088190d0f755ee277efb8858cb350782446ccbfbbf33c3882d93  height=12608  trust=00000000000000000000000000000000000000000000000
06ccdce96b2f6bbd4  blocktrust=398099662768620  date=02/12/17 22:10:56
2017-06-24 13:56:06 ProcessBlock: ACCEPTED
2017-06-24 13:56:06 ProcessBlock: ORPHAN BLOCK 1, prev=3797cfd4ec2ef299d42271c8b1f4332e40bad7d7fda21ba3ece61663c491002c
2017-06-24 13:56:06 SetBestChain: new best=bac5a2a5205b1fe87af620fdaf0a82253c1f9c57595d2e2f38aa6e0267cce776  height=12609  trust=00000000000000000000000000000000000000000000000
06ccf49e7ff0d4891  blocktrust=417064075824317  date=02/12/17 22:11:12
2017-06-24 13:56:06 ProcessBlock: ACCEPTED
```

And:

```
iond getblock 3797cfd4ec2ef299d42271c8b1f4332e40bad7d7fda21ba3ece61663c491002c
{
    "hash" : "3797cfd4ec2ef299d42271c8b1f4332e40bad7d7fda21ba3ece61663c491002c",
    "confirmations" : 1037,
    "size" : 435,
    "height" : 179213,
    "version" : 7,
    "merkleroot" : "d7cca80aabda8bbbc9d2dd32d9dcd997f38cbc5e5595e913a24cccf6e69fb803",
    "mint" : 17.00000000,
    "time" : 1498312304,
    "nonce" : 0,
    "bits" : "1a0df384",
    "difficulty" : 1202543.18080997,
    "blocktrust" : "12598187cb17ad",
    "chaintrust" : "29760e66cf9fcb2e14",
    "previousblockhash" : "b771d6116d93e4beb8337b0dc794efca8465e42e3e29239e62761e2e7db7e8c5",
    "nextblockhash" : "a503a539052ba92616d0c749d3732165bf544683c788e169a2fe2c968e89bf8a",
    "flags" : "proof-of-stake",
    "proofhash" : "00000dffb8a00d9d3e6ddba43ecbd9f78d7021169eb4af4d771d50168f090357",
    "entropybit" : 0,
    "modifier" : "097394bbca2bb967",
    "modifierv2" : "5bd340423a945711940579c1a0d8060ca9dbca69021a98d9927407eaef139c84",
    "tx" : [
        "4577d4c7045ffea968be6064b53842b30410736a9121ef65932323f63969abe4",
        "bf6ea72867846b764b14d5dc8afbc4566296ff7a38fe286ef7ab93ccf4f2e8ee"
    ],
    "signature" : "3044022041c07c4df241bd3a8edf36e6d1d6de1ed61d3610dcccde49c62c77d61376cb6b02206521a2f9a758e43eaef63c5378e8c248c21ae5b54c2d0bc8c4acfbb72fbac06c"
}
```

And then:

```
2017-06-24 13:56:07 SetBestChain: new best=bdb25a0b8589d5ac986009877d16dc666a9d2c84715bc7bf46c87aeacddb013d  height=12680  trust=00000000000000000000000000000000000000000000000
06d87fab882b3f1a5  blocktrust=797934129899347  date=02/12/17 23:28:48
2017-06-24 13:56:07 ProcessBlock: ACCEPTED
2017-06-24 13:56:07 ProcessBlock: ORPHAN BLOCK 2, prev=b24fc6313bf26e4b465b70c6da9fc1a0d821aa8c06c310bcd5e76b9f1a85053f
2017-06-24 13:56:07 SetBestChain: new best=1de76f4fdbde200a79d98adc7c66b5679baa9c4fa8e7e7ea53e6f91ef98c04e2  height=12681  trust=00000000000000000000000000000000000000000000000
06d8b190261fd8a0d  blocktrust=877727557720168  date=02/12/17 23:34:24
2017-06-24 13:56:07 ProcessBlock: ACCEPTED
```

With:

```
./iond -datadir=/home/dev/.ionomy-fork1 getblock b24fc6313bf26e4b465b70c6da9fc1a0d821aa8c06c310bcd5e76b9f1a85053f
{
    "hash" : "b24fc6313bf26e4b465b70c6da9fc1a0d821aa8c06c310bcd5e76b9f1a85053f",
    "confirmations" : 165275,
    "size" : 478,
    "height" : 15000,
    "version" : 7,
    "merkleroot" : "adf73ed5a3e34a248c566c09eea6cf62f67e96715249cbda52f42d393ad7fd5e",
    "mint" : 23.00000000,
    "time" : 1487104448,
    "nonce" : 0,
    "bits" : "1a1a03df",
    "difficulty" : 644892.62622974,
    "blocktrust" : "9d72677770f28",
    "chaintrust" : "993cef448646f7e5",
    "previousblockhash" : "5aa028ae2fe4ade89a465cd84f7391d66ceabdc62cfa246781ba75956a38aa80",
    "nextblockhash" : "1027e3eed56d445fa0054f8038ac0b0c8c41f2bbc45dd680132f0ff6dc6fbce2",
    "flags" : "proof-of-stake",
    "proofhash" : "003f8c8c5901d1476f2cb4fb44e9743f757564ef409f4a7268d78bfe4bc3fdb1",
    "entropybit" : 1,
    "modifier" : "c610fad27bf1d8db",
    "modifierv2" : "e3283494c1c3d77d2587a0a2569e27bea0562adeac02c42c43bb1910a7ed0ea6",
    "tx" : [
        "40b64450fdb0af2854d7ebe00c9b936ba778a6331836f66807096235c9a9dd2c",
        "6137fdb98a60c9b9374a577145f2837564eb9b80d1063e513fed132ee8cbf0e1"
    ],
    "signature" : "3045022100c0973c0e6315abc41f5e4eb1aaae3ebf45e3a7e222d42b76f0d7b2f2d93c13b902200a12e43f0ef336b27d1401d9f84abaea7c582e069654123c6455b79dc8dab2a4"
}
```

### Second case

The second case is a continuation of the sync above. It follows a similar (but slightly different) pattern:
- New blocks are received and marked as orphans
- Chunks of blocks are received that are far ahead in the sync process; they are marked as orphans
- The sync stalls at the block height of one of the blocks it marked as an orphan earlier

The orphans are received as follows:
- Orphan block (with prev height 179212) after block 5000
- Orphan block (with prev height 179213) after block 12608
- Orphan block (whth prev height 15000) after block 12680
- Orphan block (whth prev height 15001) after block 12681
- Orphan block (whth prev height 15002) after block 12682
- The last block accepted is 45156 (hash 2b9d1e04c42f323c3372158f5a727d6f1f1535238a10ddc712f1f850e97e2096)
- Block 45157 (hash 504152092026f169583da88a70251e0ba8a1f1ceded6ecbd8585563e2daddd39) is never received (also not as orphan)
- Block 45158 (hash 07c33fcd4b5426a12774ef6a9ec442f392fb6bf59976d289b73939ca14dd9b1c) is never received (also not as orphan)

Next steps:
- Extract block heights of orphans, using `grep ORPHAN debug.log| awk -F '=' '{iond getblock $2|grep height}'`