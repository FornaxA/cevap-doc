# Use Cases, Scenario’s, Walkthroughts - for tokens

[back to main page](README.md)

## Case "Find token authorities"

**Scenario**: Create and distribute the XDM token according to the IEO

**Feature**: Scanning the UTXO set and retrieving wallet information on token authorities

**Actor**: Token authority owners

**Summary**:

By scanning spendable coins, one can create a list of all token authorities in the current state of the blockchain.
There are two options:
- Scanning the full public blockchain to find all currently existing token authorities
- Scanning one's own wallet to find the token authorities one can currently use

*The current case is written for the DarkMatter token, but can be generalized to any other token.*

**Preconditions**: 

- The token group has been created and the needed tokens have been minted. (See e.g. Case [Create Management Tokens](UseCases_tokens_Create-Management-Tokens.md)).

**Steps**: 

A token creator can find the currently available token authorities using the following commands:
- `scantokens start [TOKENGROUPID]`: scan the full public blockchain for all currently available token coins, including both spendable tokens and token authorities
- `token listauthorities (TOKENGROUPID)`: show the wallet's unspend transaction outputs that are token authorities. The scan can be limited by tokengroup.
- `token balance (TOKENGROUPID) (IONADDRESS)`: show the wallet's per-token balance, including the total of the token authorities available in the wallet. The scan can be limited by tokengroup or by tokengroup and ION address.

_Look up the token group ID of DarkMatter_
Scantokens needs the ID of a tokengroup as a parameter, and the other two commands may have the ID of a tokengroup as a parameter.
See [View information related to a token group](UseCases_tokens_View-token-information.md) for detailed information on how to retrieve the tokengroup ID.

```
$ tokeninfo ticker XDM
[
  {
    "groupIdentifier": "ionrt1zd4eqkzw580qqftdax8qaq5spdce6ds9zhe7567epd5r3a53r9sqc9tz0tw",
    "txid": "18251f19739a5013681d3becc9c523ebee374f8b0410d3cc8d073078a6d56b68",
    "ticker": "XDM",
    "name": "DarkMatter",
    "decimalPos": 13,
    "URL": "https://www.darkmatter.info/",
    "documentHash": "0000000000000000000000000000000000000000000000000000000000000000"
  }
]
```

_Scan the blockchain for token authorities for DarkMatter_
The scan command walks through each item in the UTXO set, returning those UTXO's that take the form of an unspent token.

Token authorities are a specific type of token: they do not specify an amount, but they specify a set of authorities instead.

The tokengroup ID and the token authorities are specified in the scriptPubKey, and if the transaction output holds token authorities, they are presented in readable form in the list of unspent items.

After the set of unspent items, the command returns the total amount of tokens on the blockchain and the aggregated set of token authorities that are encountered in the scan. 

```
$ scantokens start ionrt1zd4eqkzw580qqftdax8qaq5spdce6ds9zhe7567epd5r3a53r9sqc9tz0tw
{
  "success": 1,
  "searched_items": 553,
  "unspents": [
    {
      "txid": "285994e794a299013a55b3797d7296c5de68f46dbcd605218a16d2983dba0a17",
      "vout": 1,
      "address": "g9cD8fBPBvECUL2s7FSiKkbwS9SjGGH1Pn",
      "scriptPubKey": "206b90584ea1de00256de98e0e82900b719d360515f3ea6bd90b6838f69119600c0800000000000000fcb6757576a9144ad6371453d3daeb613a202266219fe66949f23788ac",
      "ION_amount": 0.00000001,
      "token_authorities": "melt child rescript subgroup",
      "height": 355
    },
    {
      "txid": "98fee7bb8c9c3e903203cd412e9dd6b5949d61d9e5647c2d6441d805819531fd",
      "vout": 0,
      "address": "gPQrewF6LJdno63tmTXwjmqDVrzMsPhdxj",
      "scriptPubKey": "206b90584ea1de00256de98e0e82900b719d360515f3ea6bd90b6838f69119600c08000087fe3c6dda09b6757576a914e2424e6f68bcee00715ef7257479c5a68267914388ac",
      "ION_amount": 0.00000001,
      "token_amount": 71000.0000000000000,
      "height": 355
    }
  ],
  "total_amount": 71000.0000000000000,
  "token_authorities": "melt child rescript subgroup"
}
```

The current scan shows a very limited number of token transactions for the DarkMatter token:
- One output that is a token authority
(`285994e794a299013a55b3797d7296c5de68f46dbcd605218a16d2983dba0a17:0`), and this coin has all possible authoritities except the mint authority.
- One output that holds 71.000 DarkMatter tokens.

_Identify coins in our wallet that hold DarkMatter token authorities_

The command to list coins in our wallet that hold token authorities is: 
`token listauthorities (TOKENGROUPID) (IONADDRESS)`. To list all DarkMatter authorities that you own, use the following command:

```
$ token listauthorities ionrt1zd4eqkzw580qqftdax8qaq5spdce6ds9zhe7567epd5r3a53r9sqc9tz0tw
[
  {
    "groupIdentifier": "ionrt1zd4eqkzw580qqftdax8qaq5spdce6ds9zhe7567epd5r3a53r9sqc9tz0tw",
    "txid": "285994e794a299013a55b3797d7296c5de68f46dbcd605218a16d2983dba0a17",
    "vout": 1,
    "address": "g9cD8fBPBvECUL2s7FSiKkbwS9SjGGH1Pn",
    "token_authorities": "melt child rescript subgroup"
  }
]
```

_List the token authorities that are available in the wallet_

The command to show the total available tokens and token authorities in the wallet is `token balance (TOKENGROUPID) (IONADDRESS)`.

```
$ token balance
[
  {
    "groupIdentifier": "ionrt1z08suycj85usle25z7c8fy0pvqg82759dkant22edca55z5c8q0qc3czz0g",
    "balance": 71000.0000000000000,
    "authorities": "melt child rescript subgroup"
  },
  {
    "groupIdentifier": "ionrt1zw655y77hx0l6m95em7ekcqm78v9v72s04yrsksup43haq0crvtqc9jyfpt",
    "balance": 5000.0000,
    "authorities": "mint melt child rescript subgroup"
  },
  {
    "groupIdentifier": "ionrt1z0etg43nh9g7h6p6st89vexuzcz967jy856xtzathunhpz0wn7ascea3s6c",
    "balance": 100000,
    "authorities": "melt child rescript subgroup"
  }
]
```

The command shows us that our wallet has authorities for 3 different token groups, and that we do not hold mint authorities for the 2 of those
token groups. (Use the `scantokens` command to find out if there are other wallets that hold more token authorities).

**Postconditions**:

- The blockchain was scanned for token amounts and token authorities
- The token balances in the wallet were retrieved (both amounts and authorities)
- The list of  token authority outputs available in the wallet was generated

**Related use cases**:

- Case "[View information related to a token group](UseCases_tokens_View-token-information.md)"
- Case "[Drop mint capability](UseCases_tokens_Drop-token-mint-capability.md)"
- Case "[Key rotation with token authorities](UseCases_tokens_Key-rotation-with-token-authorities.md)"