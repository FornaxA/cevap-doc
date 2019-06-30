# Use Cases, Scenario’s, Walkthroughts - for tokens

[back to main page](README.md)

## Case "Drop token mint capability"

**Scenario**: Create and distribute the XDM token according to the IEO

**Feature**: Dropping token authorities

**Actor**: Token authority owners

**Summary**:

Unlike with the ION native tokens (ION and xION), user tokens cannot be created through staking. However, when one has the correct
authority (a `mint authority` for that specific token), a user can create new user tokens through direct minting using the `token mint` command.

When creating a new token group (see e.g. [Create management tokens](UseCases_tokens_Create-Management-Tokens.md)), the token creator receives
two token coins:
- A token authority coin
- A token mint coin

The token authority coin initially has all current authorities enabled for that specific token group:
- **Mint**: create new tokens
- **Melt**: destroy tokens
- **Child**: create new authority coins
- **Rescript**: (reserved) allow changing scripts (contract) connected to token coins
- **Subgroup** create token subgroups

The token mint coin is an output of a transaction that has more tokens coming out of a transaction than going into the transaction: 
the transaction creates new tokens. A user is only able to create a token mint transaction when they own a token mint authority for
that specific token group.

This use case describes scanning the blockchain for token mint authorities for a specific coin, dropping token authorities
(here: dropping the token mint authority) and verifying that the chain no longer holds a mint authority for a specific coin.

The command to use is:
- `token dropauthorities [TOKENGROUPID] [TRANSACTIONID] [OUTPUTNR] [authorityflag1] (authorityflag2) (authorityflag..)`: drop the specified authorities from the authority output of the specified tokengroup located at the specified transaction output.

To drop the mint authority, `mint` should be specified in the list of authority flag parameters.

*The current case is written for dropping the mint authority for DarkMatter, but can be generalized to dropping any authority for any token group.*

**Preconditions**: 

- The token group has been created and the needed tokens have been minted.

**Steps**: 

DarkMatter is a token that should have a provable max of 71000 XDM. To set and verify this limit, follow the steps below.

_Look up the token group ID of DarkMatter_

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
      "token_authorities": "mint melt child rescript subgroup",
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
  "token_authorities": "mint melt child rescript subgroup"
}
```

The `scantokens` command returns:
- All coins that are tokens
- All coins that are token authorities
- The total amount of tokens
- The aggregated set of token authorities

The above resuls show that there is 1 coin that is a token authority
(`285994e794a299013a55b3797d7296c5de68f46dbcd605218a16d2983dba0a17:0`), and this coin has all possible authoritities (mint, melt, child, rescript, subgroup).

_Identify coins in our wallet that hold DarkMatter token authorities_

The command to list coins in our wallet that hold token authorities is: 
`token listauthorities (TOKENGROUPID) (IONADDRESS)`. List all DarkMatter authorities that we own using the command:

```
$ token listauthorities ionrt1zd4eqkzw580qqftdax8qaq5spdce6ds9zhe7567epd5r3a53r9sqc9tz0tw
[
  {
    "groupIdentifier": "ionrt1zd4eqkzw580qqftdax8qaq5spdce6ds9zhe7567epd5r3a53r9sqc9tz0tw",
    "txid": "285994e794a299013a55b3797d7296c5de68f46dbcd605218a16d2983dba0a17",
    "vout": 1,
    "address": "g9cD8fBPBvECUL2s7FSiKkbwS9SjGGH1Pn",
    "token_authorities": "mint melt child rescript subgroup"
  }
]
```

This shows that the only token authority on the blockchain is in our wallet.

_Explicitly drop authorities from a token authority coin_

Call the `token dropauthorities` command on the tokengroup ID for DarkMatter, and explicitly provide the transactionhash and transaction
output number of the token authority that you want to alter. Also provide which token authority flags to drop; when specifying all existing
flags or when specifying `all`, the complete authority will be removed.

In the case of DarkMatter, we want to remove the `mint` authority.

```
$ token dropauthorities ionrt1zd4eqkzw580qqftdax8qaq5spdce6ds9zhe7567epd5r3a53r9sqc9tz0tw 285994e794a299013a55b3797d7296c5de68f46dbcd605218a16d2983dba0a17 1 mint

{
  "groupIdentifier": "ionrt1zd4eqkzw580qqftdax8qaq5spdce6ds9zhe7567epd5r3a53r9sqc9tz0tw",
  "transaction": "285994e794a299013a55b3797d7296c5de68f46dbcd605218a16d2983dba0a17",
  "vout": 1,
  "coin": "COutput(285994e794a299013a55b3797d7296c5de68f46dbcd605218a16d2983dba0a17, 1, 1) [0.00000001]",
  "script": "6b90584ea1de00256de98e0e82900b719d360515f3ea6bd90b6838f69119600c 00000000000000fc OP_GROUP OP_DROP OP_DROP OP_DUP OP_HASH160 c4e69bf63f95a9e7845a63f6c29d9875dfc52675 OP_EQUALVERIFY OP_CHECKSIG",
  "destination": "gLjdD9DcjwGRhkRNnUuM6euSobFMNPdpLf",
  "token_authorities": "mint melt child rescript subgroup",
  "authorities_to_keep": "melt child rescript subgroup"
}
```

_Verify the new authorities_
To verify that there are no token mint authorities for DarkMatter on the blockchain, run the scan command again:
```
$ scantokens start ionrt1zd4eqkzw580qqftdax8qaq5spdce6ds9zhe7567epd5r3a53r9sqc9tz0tw
{
  "success": 1,
  "searched_items": 553,
  "unspents": [
    {
      "txid": "be87f64520882eb81cb8d7fef551489e20e0f54a2864eb5f830dba362329dfa2",
      "vout": 0,
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
In this case, there is 1 token authority for DarkMatter, and it does not include the mint authority.
Note that the resulting mint authority is stored in a new address (in this case, `gPQrewF6LJdno63tmTXwjmqDVrzMsPhdxj`).


**Postconditions**:

- The only DarkMatter authority on the blockchain does not have a mint authority

**Related use cases**:

- Case "[Create Management Tokens](UseCases_tokens_Create-Management-Tokens.md)"
- Case "[Find token authorities](UseCases_tokens_Find-token-authorities.md)"
- Case "[View information related to a token group](UseCases_tokens_View-token-information.md)"