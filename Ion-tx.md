
# Creating and mutating transactions with ion-tx

[back to main page](README.md)

## ION Core tools
ION Core currently knows the following binaries:
iond - the regular daemon, which includes server and wallet
ion-qt - the graphical client, which also includes server and wallet functionality
ion-cli - the command line interface, which is used to control the server functionality provided by iond and ion-qt
ion-tx - a tool to create and mutate ion transactions

## Transaction creation
Typically, users first learn to create transactions using the wallet functionality provided by iond/ion-cli or by ion-qt. When creating a new transaction, the wallet determines or helps to determine which unspent outputs to use, how much fee to send to the network and to which addresses to send the ion coins.

Raw transcations can also be created and inspected by both client. 

## Raw transaction example

Here is an example raw testnettransaction:

```
./ion-cli gettransaction e8c01abcbfbebc062a1121ec13c3281128e69f7b7b4229ba5afcaf05efd213e3
{
    "txid" : "e8c01abcbfbebc062a1121ec13c3281128e69f7b7b4229ba5afcaf05efd213e3",
    "version" : 1,
    "time" : 1508765842,
    "locktime" : 0,
    "vin" : [
        {
            "txid" : "30ff305f2e00b16a9c672ab32bdf465b8c3dc3500a47ea5d7eb7f2e5c0d89286",
            "vout" : 1,
            "scriptSig" : {
                "asm" : "304402201fd75bebcf6d1f2c6d0f1cbefb2bc297e368741e21457a34677f4dda3b607c5c02206eb05c063e06c1ef0d9d560c90476f6716c8b467db34efe56b421462e11189f001 03931c6f8dfc0088038130a351b6b9e4f53c1db39cf33d6ca06f025831a840092b",
                "hex" : "47304402201fd75bebcf6d1f2c6d0f1cbefb2bc297e368741e21457a34677f4dda3b607c5c02206eb05c063e06c1ef0d9d560c90476f6716c8b467db34efe56b421462e11189f0012103931c6f8dfc0088038130a351b6b9e4f53c1db39cf33d6ca06f025831a840092b"
            },
            "sequence" : 4294967295
        }
    ],
    "vout" : [
        {
            "value" : 0.12890000,
            "n" : 0,
            "scriptPubKey" : {
                "asm" : "OP_DUP OP_HASH160 4cf2ed4585148f8a246a59fda07abb7d2aab44d4 OP_EQUALVERIFY OP_CHECKSIG",
                "hex" : "76a9144cf2ed4585148f8a246a59fda07abb7d2aab44d488ac",
                "reqSigs" : 1,
                "type" : "pubkeyhash",
                "addresses" : [
                    "g9oNsxyCpf9BaUTcRQuM2c9ZFc5xorajqf"
                ]
            }
        },
        {
            "value" : 0.52810000,
            "n" : 1,
            "scriptPubKey" : {
                "asm" : "OP_DUP OP_HASH160 98375d2d20b74710be6ced90167d89bab91137e6 OP_EQUALVERIFY OP_CHECKSIG",
                "hex" : "76a91498375d2d20b74710be6ced90167d89bab91137e688ac",
                "reqSigs" : 1,
                "type" : "pubkeyhash",
                "addresses" : [
                    "gGfMYSwcCbRrHGNcN3JJvYHpfwgYoibEoH"
                ]
            }
        }
    ],
    "amount" : -0.12890000,
    "fee" : -0.00100000,
    "confirmations" : 0,
    "bcconfirmations" : 0,
    "txid" : "e8c01abcbfbebc062a1121ec13c3281128e69f7b7b4229ba5afcaf05efd213e3",
    "walletconflicts" : [
    ],
    "time" : 1508765842,
    "timereceived" : 1508765842,
    "details" : [
        {
            "account" : "",
            "address" : "g9oNsxyCpf9BaUTcRQuM2c9ZFc5xorajqf",
            "category" : "send",
            "amount" : -0.12890000,
            "fee" : -0.00100000
        }
    ]
}
```

We will create a transaction that spends another 0.0871 ION from vout:1 from this transaction (which currently holds 0.5281 ION).

### Find unspent transactions using ion-cli

A transaction takes as input the unspent output of another transaction. Let's find the unspent outpouts of transaction e8c01abcbfbebc062a1121ec13c3281128e69f7b7b4229ba5afcaf05efd213e3.

```
./ion-cli listunspent 1 10000 "gGfMYSwcCbRrHGNcN3JJvYHpfwgYoibEoH"
error: Error parsing JSON:gGfMYSwcCbRrHGNcN3JJvYHpfwgYoibEoH
```

This does not work; it tells us that ion-cli expects a JSON encoded string. We'll need to put brackets and double quotes around the address, and 'escape' the quotes in that JSON string as follows:

```
./ion-cli listunspent 1 10000 "[\"gGfMYSwcCbRrHGNcN3JJvYHpfwgYoibEoH\"]"
[
    {
        "txid" : "e8c01abcbfbebc062a1121ec13c3281128e69f7b7b4229ba5afcaf05efd213e3",
        "vout" : 1,
        "address" : "gGfMYSwcCbRrHGNcN3JJvYHpfwgYoibEoH",
        "scriptPubKey" : "76a91498375d2d20b74710be6ced90167d89bab91137e688ac",
        "amount" : 0.52810000,
        "confirmations" : 10,
        "spendable" : true
    }
]
```

### Transaction creation using ion-cli

```
./ion-cli createrawtransaction "[{\"txid\":\"e8c01abcbfbebc062a1121ec13c3281128e69f7b7b4229ba5afcaf05efd213e3\",\"vout\":1}]" "{\"g9oNsxyCpf9BaUTcRQuM2c9ZFc5xorajqf\":0.5279}"
010000008c34ee5901e313d2ef05affc5aba29427b7b9fe6281128c313ec21112a06bcbebfbc1ac0e80100000000ffffffff01f0822503000000001976a9144cf2ed4585148f8a246a59fda07abb7d2aab44d488ac00000000
```

And then:
```
./ion-cli decoderawtransaction 010000008c34ee5901e313d2ef05affc5aba29427b7b9fe6281128c313ec21112a06bcbebfbc1ac0e80100000000ffffffff01f0822503000000001976a9144cf2ed4585148f8a246a59fda07abb7d2aab44d488ac00000000
{
    "txid" : "f9d1ac01b749b9b29560d383318bb8d9a47157f7c7d2c5cc6fe5028b81410a06",
    "version" : 1,
    "time" : 1508783244,
    "locktime" : 0,
    "vin" : [
        {
            "txid" : "e8c01abcbfbebc062a1121ec13c3281128e69f7b7b4229ba5afcaf05efd213e3",
            "vout" : 1,
            "scriptSig" : {
                "asm" : "",
                "hex" : ""
            },
            "sequence" : 4294967295
        }
    ],
    "vout" : [
        {
            "value" : 0.52790000,
            "n" : 0,
            "scriptPubKey" : {
                "asm" : "OP_DUP OP_HASH160 4cf2ed4585148f8a246a59fda07abb7d2aab44d4 OP_EQUALVERIFY OP_CHECKSIG",
                "hex" : "76a9144cf2ed4585148f8a246a59fda07abb7d2aab44d488ac",
                "reqSigs" : 1,
                "type" : "pubkeyhash",
                "addresses" : [
                    "g9oNsxyCpf9BaUTcRQuM2c9ZFc5xorajqf"
                ]
            }
        }
    ]
}
```

### Transaction creation, hex encoding and json output using ion-tx

To create a new, empty transaction, use the -create option to ion-tx:
```
./ion-tx -create
010000009800ee59000000000000
```

This is a hex encoded transaction, with the following information:

```
./ion-tx -testnet -json 010000009800ee59000000000000
{
    "txid": "9f6e10a949f0a14ed63cfdc8b0cd16b545fca47ad5655c61f24b90de86af8a99",
    "version": 1,
    "time": 1508769944,
    "locktime": 0,
    "vin": [
    ],
    "vout": [
    ],
    "hex": "010000009800ee59000000000000"
}
```

### Transaction mutation: change transaction time

```
./ion-tx -testnet -json 010000009800ee59000000000000 time=1508783244
{
    "txid": "9fa096558dcecc8f345d54258c2f4b0deccbf24297721bea1b1f52bc26beef1a",
    "version": 1,
    "time": 1508783244,
    "locktime": 0,
    "vin": [
    ],
    "vout": [
    ],
    "hex": "010000008c34ee59000000000000"
}
```

### Transaction mutation: add vin

```
./ion-tx -testnet -json 010000008c34ee59000000000000 in=e8c01abcbfbebc062a1121ec13c3281128e69f7b7b4229ba5afcaf05efd213e3:1
{
    "txid": "be4b2569b3b4d4035beb963d3f5cd2da3093839eda2936af709d2a07d67c8bc4",
    "version": 1,
    "time": 1508783244,
    "locktime": 0,
    "vin": [
        {
            "txid": "e8c01abcbfbebc062a1121ec13c3281128e69f7b7b4229ba5afcaf05efd213e3",
            "vout": 1,
            "scriptSig": {
                "asm": "",
                "hex": ""
            },
            "sequence": 4294967295
        }
    ],
    "vout": [
    ],
    "hex": "010000008c34ee5901e313d2ef05affc5aba29427b7b9fe6281128c313ec21112a06bcbebfbc1ac0e80100000000ffffffff0000000000"
}
```

### Transaction mutation: add vout

```
/ion-tx -testnet -json 010000008c34ee5901e313d2ef05affc5aba29427b7b9fe6281128c313ec21112a06bcbebfbc1ac0e80100000000ffffffff0000000000 outaddr=0.5279:g9oNsxyCpf9BaUTcRQuM2c9ZFc5xorajqf
{
    "txid": "f9d1ac01b749b9b29560d383318bb8d9a47157f7c7d2c5cc6fe5028b81410a06",
    "version": 1,
    "time": 1508783244,
    "locktime": 0,
    "vin": [
        {
            "txid": "e8c01abcbfbebc062a1121ec13c3281128e69f7b7b4229ba5afcaf05efd213e3",
            "vout": 1,
            "scriptSig": {
                "asm": "",
                "hex": ""
            },
            "sequence": 4294967295
        }
    ],
    "vout": [
        {
            "value": 0.5279,
            "n": 0,
            "scriptPubKey": {
                "asm": "OP_DUP OP_HASH160 4cf2ed4585148f8a246a59fda07abb7d2aab44d4 OP_EQUALVERIFY OP_CHECKSIG",
                "hex": "76a9144cf2ed4585148f8a246a59fda07abb7d2aab44d488ac",
                "reqSigs": 1,
                "type": "pubkeyhash",
                "addresses": [
                    "g9oNsxyCpf9BaUTcRQuM2c9ZFc5xorajqf"
                ]
            }
        }
    ],
    "hex": "010000008c34ee5901e313d2ef05affc5aba29427b7b9fe6281128c313ec21112a06bcbebfbc1ac0e80100000000ffffffff01f0822503000000001976a9144cf2ed4585148f8a246a59fda07abb7d2aab44d488ac00000000"
}
```

### Compare transaction ID's to verify ion-tx produces the same tx as ion-cli

Both routes produce the same tx id: f9d1ac01b749b9b29560d383318bb8d9a47157f7c7d2c5cc6fe5028b81410a06

### Sign transaction

```
./ion-cli signrawtransaction 010000008c34ee5901e313d2ef05affc5aba29427b7b9fe6281128c313ec21112a06bcbebfbc1ac0e80100000000ffffffff01f0822503000000001976a9144cf2ed4585148f8a246a59fda07abb7d2aab44d488ac00000000
{
    "hex" : "010000008c34ee5901e313d2ef05affc5aba29427b7b9fe6281128c313ec21112a06bcbebfbc1ac0e8010000006b48304502210087d0d66bc5a76a07441bbb33bac30d1b713100c3484175d6ac016ffacef4961502203ef7a69da48e68e673aefe4dd5007c6ef63a4438f34882b88001c6a76a0dcd6a01210398eefd10371706b588b1e5550da8e2a2fd6864ca22778c827df13acf691d277dffffffff01f0822503000000001976a9144cf2ed4585148f8a246a59fda07abb7d2aab44d488ac00000000",
    "complete" : true
}
```

```
./ion-cli dumpprivkey gGfMYSwcCbRrHGNcN3JJvYHpfwgYoibEoH
cRoBkRoFopsWMHbvoCcT5LRJqX2dHoxfcURzNgVzisuvTFmUQExa
```


```
../ion-tx -testnet -json 010000008c34ee5901e313d2ef05affc5aba29427b7b9fe6281128c313ec21112a06bcbebfbc1ac0e80100000000ffffffff01f0822503000000001976a9144cf2ed4585148f8a246a59fda07abb7d2aab44d488ac00000000 set=privatekeys:"[\"cRoBkRoFopsWMHbvoCcT5LRJqX2dHoxfcURzNgVzisuvTFmUQExa\"]" set=prevtxs:"[{\"txid\":\"e8c01abcbfbebc062a1121ec13c3281128e69f7b7b4229ba5afcaf05efd213e3\",\"vout\":1,\"scriptPubKey\":\"76a91498375d2d20b74710be6ced90167d89bab91137e688ac\"}]" sign
{
    "txid": "dcdecb0f754c7c9f05fb001d96fafa8d71068a9a3035472f1037e932fc1f7285",
    "version": 1,
    "time": 1508783244,
    "locktime": 0,
    "vin": [
        {
            "txid": "e8c01abcbfbebc062a1121ec13c3281128e69f7b7b4229ba5afcaf05efd213e3",
            "vout": 1,
            "scriptSig": {
                "asm": "3045022100a693320598b678e8605f49616d48ae08dc23d6284654894b8fd65c33f9a6b9b002200de3aad989e874762716ae5315d9e6d0147ea2561f6a4dec10c890a9ce4f54e9[ALL] 0398eefd10371706b588b1e5550da8e2a2fd6864ca22778c827df13acf691d277d",
                "hex": "483045022100a693320598b678e8605f49616d48ae08dc23d6284654894b8fd65c33f9a6b9b002200de3aad989e874762716ae5315d9e6d0147ea2561f6a4dec10c890a9ce4f54e901210398eefd10371706b588b1e5550da8e2a2fd6864ca22778c827df13acf691d277d"
            },
            "sequence": 4294967295
        }
    ],
    "vout": [
        {
            "value": 0.5279,
            "n": 0,
            "scriptPubKey": {
                "asm": "OP_DUP OP_HASH160 4cf2ed4585148f8a246a59fda07abb7d2aab44d4 OP_EQUALVERIFY OP_CHECKSIG",
                "hex": "76a9144cf2ed4585148f8a246a59fda07abb7d2aab44d488ac",
                "reqSigs": 1,
                "type": "pubkeyhash",
                "addresses": [
                    "g9oNsxyCpf9BaUTcRQuM2c9ZFc5xorajqf"
                ]
            }
        }
    ],
    "hex": "010000008c34ee5901e313d2ef05affc5aba29427b7b9fe6281128c313ec21112a06bcbebfbc1ac0e8010000006b483045022100a693320598b678e8605f49616d48ae08dc23d6284654894b8fd65c33f9a6b9b002200de3aad989e874762716ae5315d9e6d0147ea2561f6a4dec10c890a9ce4f54e901210398eefd10371706b588b1e5550da8e2a2fd6864ca22778c827df13acf691d277dffffffff01f0822503000000001976a9144cf2ed4585148f8a246a59fda07abb7d2aab44d488ac00000000"
}
```

## Differences

There are minor differences in the signed transaction.
