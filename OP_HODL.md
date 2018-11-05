# Time locked addresses

To create a Time Locked Address, do the following:

## Go to the ION browser wallet; pick one from:
- https://github.com/cevap/ion-browser-wallet/
- https://wallet.ioncore.xyz
- http://ionomy.i2p/

## Create the Time Locked Address:
2. New -> Time Locked Address.
3. Enter your address public key (the one you require to sign the transaction and be able to spend the coins);
4. Enter the date-time or blockheight you want to release the coins.
5. Submit and save the Redeem Script (don't lose that or you won't be able to spend your coins in the future);
6. Send the coins you want to keep locked to the Address generated.

## After the chosen period, you will be able to spend your coins.

To test, have a look at the following time locked transaction walk-through

recipient=gP5pjMUB38MqPAwKitALTHRGCsFAugGfeY
redeemScript=03003007b17521038033b990150700f9358af48ca0d56c39f0679086bd35df9f151a0b3dde958a02ac
txid=38fb502efd68792d1f45881e8028834d8c617902f38cb0be701d24751c076391
vout=0
scriptPubKey=a9149637a186987c6e27b1cdea289d5d61df6dbe58bb87
privateKey=cNkSJuBSv6TesmuYSw4gk5Ju6sn4v7trsr4yA7WQnK1CxTXL59cV


`./ion-cli getrawtransaction 38fb502efd68792d1f45881e8028834d8c617902f38cb0be701d24751c076391`

`./ion-cli decoderawtransaction 01000000000000004de9...30c088ac00000000`

```
...
"vout": [
    {
      "value": 5.00000000,
      "n": 0,
      "scriptPubKey": {
        "asm": "OP_HASH160 9637a186987c6e27b1cdea289d5d61df6dbe58bb OP_EQUAL",
        "hex": "a9149637a186987c6e27b1cdea289d5d61df6dbe58bb87",
        "reqSigs": 1,
        "type": "scripthash",
        "addresses": [
          "2N6wVzN9PoB37gmnbtoQu5nKifZbFRruXiA"
        ]
      }
    },
...
```

`./ion-cli createrawtransaction "[{\"txid\":\"38fb502efd68792d1f45881e8028834d8c617902f38cb0be701d24751c076391\",\"vout\":0}]" "{\"gP5pjMUB38MqPAwKitALTHRGCsFAugGfeY\":4.96}"`

```
0100000000000000019163071c75241d70beb08cf30279618c4d8328801e88451f2d7968fd2e50fb380000000000ffffffff01005c901d000000001976a914dea8cc13e3d070e9f1595a443f0f2ff91c5b11c988ac00000000
```

```
./ion-cli signrawtransaction 0100000000000000019163071c75241d70beb08cf30279618c4d8328801e88451f2d7968fd2e50fb380000000000ffffffff01005c901d000000001976a914dea8cc13e3d070e9f1595a443f0f2ff91c5b11c988ac00000000 "[{\"txid\":\"38fb502efd68792d1f45881e8028834d8c617902f38cb0be701d24751c076391\",\"vout\":0,\"scriptPubKey\":\"a9149637a186987c6e27b1cdea289d5d61df6dbe58bb87\",\"redeemScript\":\"03003007b17521038033b990150700f9358af48ca0d56c39f0679086bd35df9f151a0b3dde958a02ac\"}]" "[\"cNkSJuBSv6TesmuYSw4gk5Ju6sn4v7trsr4yA7WQnK1CxTXL59cV\"]"  "ALL"
```

```
0100000000000000019163071c75241d70beb08cf30279618c4d8328801e88451f2d7968fd2e50fb38000000002a2903003007b17521038033b990150700f9358af48ca0d56c39f0679086bd35df9f151a0b3dde958a02acffffffff01005c901d000000001976a914dea8cc13e3d070e9f1595a443f0f2ff91c5b11c988ac00000000
```
