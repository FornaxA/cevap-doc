# Use Cases, Scenario’s, Walkthroughts - for tokens

[back to main page](README.md)

## Case "Key rotation with token authorities"

**Scenario**: Token group management

**Feature**: Giving token authorities to third parties and dropping token authorities

**Actor**: Token authority owners

**Summary**:

Token authorities are transaction outputs with a special value (which denote the tokengroup ID and token authority flags),
and as such, they can be send to any ION address.

While there is no command to directly *send* a token authority to another address, there are to commands that together enable a token
authority owner to send a token authority to another address:
- `token createauthority [TOKENGROUPID] [IONADDRESS] (authorityflag1) (authorityflag2) (authorityflag..)`: create a new token authority
for the specified token group, and send it to the specified ION address. Either create a token authority with all possible authorities,
or use the authorities specified on the command line.
- `token dropauthorities [TOKENGROUPID] [TRANSACTIONID] [OUTPUTNR] [authorityflag1] (authorityflag2) (authorityflag..)`: drop the specified authorities from the authority output of the specified tokengroup located at the specified transaction output.

By first creating a new token authority and then dropping one's own token authority, one can send a token authority to a new ION address.

- When calling `token createauthority`, the client 1) spends his own token authority (because the new transaction spends a token authority output in the user's wallet), 2) creates a new transaction output with the specified permissions for the recipient ION address that is specified, and 3) creates a new token authority with the original permissions that is send to the user's own wallet using a fresh change address.
- The format of the `token dropauthorities` command is structured to avoid dropping more or different authorities than intended. This is why the transaction ID and output number need to be specified, and this is why the needed permission flags need to be specified. They can be: `mint`, `melt`, `child`, `rescript`, `subgroup` or `all`.

*The current case is written for the DarkMatter management tokens, but can be generalized to user tokens.*

**Preconditions**: 

- The token group has been created and the needed tokens have been minted.

**Steps**:

The steps for a succesful key rotation are:
- Look up the token group ID of the token for which you'll do authority key rotation
- Create a new token authority and send it to an address provided by the new authority owner
- Identify coins in your wallet that hold token authorities for that token group
- Remove (drop) all your token authority coins for that token group

_Look up the token group ID of DarkMatter_

```
$ tokeninfo name DarkMatter
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

DarkMatter tokens on testnet and regtest are specified by using the token group ID
`ionrt1zd4eqkzw580qqftdax8qaq5spdce6ds9zhe7567epd5r3a53r9sqc9tz0tw`.

_Create a new token authority and send it to an address provided by the new authority owner_

The command to create a new token authority is: 
`token createauthority [TOKENGROUPID] [IONADDRESS] (authorityflag1) (authorityflag2) (authorityflag..)`. When no authority flags are specified, all authority permission flags (mint melt child rescript subgroup) are selected. When the user does not have the needed authority, the command fails.

To send a new token authority with all permission flags set to address `gB4gzoFi82SHEFAfZ5cUPLWfCNUbPbDDZk`, use the following command:

```
$ token createauthority ionrt1zd4eqkzw580qqftdax8qaq5spdce6ds9zhe7567epd5r3a53r9sqc9tz0tw gB4gzoFi82SHEFAfZ5cUPLWfCNUbPbDDZk
e69197cf4ce7b536ffde4964080a376b7379f6b48841bfed7bb892fd6ffe45bb
```

To send a new token authority with specific permission flags set (e.g., send a non-renewable mint token authority), specify the needed authority flags on the command line:

```
$ token createauthority ionrt1zd4eqkzw580qqftdax8qaq5spdce6ds9zhe7567epd5r3a53r9sqc9tz0tw gB4gzoFi82SHEFAfZ5cUPLWfCNUbPbDDZk mint
609d41229687a595bd6123b4b874caf4bf46de18db11a743c9ca5c1a55d94485
```

Identify coins in your wallet that hold token authorities_

To drop token authorities, you need to specify the coin that holds the authorities you want to drop through the transaction ID and transaction output, as follows:

```
$ token listauthorities ionrt1zd4eqkzw580qqftdax8qaq5spdce6ds9zhe7567epd5r3a53r9sqc9tz0tw
[
  {
    "groupIdentifier": "ionrt1zd4eqkzw580qqftdax8qaq5spdce6ds9zhe7567epd5r3a53r9sqc9tz0tw",
    "txid": "e69197cf4ce7b536ffde4964080a376b7379f6b48841bfed7bb892fd6ffe45bb",
    "vout": 1,
    "address": "gB4gzoFi82SHEFAfZ5cUPLWfCNUbPbDDZk",
    "token_authorities": "mint melt child rescript subgroup"
  }
]
```

This token authority is specified through providing `e69197cf4ce7b536ffde4964080a376b7379f6b48841bfed7bb892fd6ffe45bb:1`.

_Remove a token authority coin_

To remove a token authority from your wallet, you need to call `token dropauthorities` with `all` token authority flags from each token authority coin that you hold for a specific token group.

```
$ token dropauthorities ionrt1zd4eqkzw580qqftdax8qaq5spdce6ds9zhe7567epd5r3a53r9sqc9tz0tw e69197cf4ce7b536ffde4964080a376b7379f6b48841bfed7bb892fd6ffe45bb 1 all

{
  "groupIdentifier": "ionrt1zd4eqkzw580qqftdax8qaq5spdce6ds9zhe7567epd5r3a53r9sqc9tz0tw",
  "transaction": "e69197cf4ce7b536ffde4964080a376b7379f6b48841bfed7bb892fd6ffe45bb",
  "vout": 1,
  "coin": "COutput(e69197cf4ce7b536ffde4964080a376b7379f6b48841bfed7bb892fd6ffe45bb, 1, 1) [0.00000001]",
  "script": "6b90584ea1de00256de98e0e82900b719d360515f3ea6bd90b6838f69119600c 00000000000000fc OP_GROUP OP_DROP OP_DROP OP_DUP OP_HASH160 5ad06a810f34986bc552d18402d3de3db62491b9 OP_EQUALVERIFY OP_CHECKSIG",
  "destination": "gB4gzoFi82SHEFAfZ5cUPLWfCNUbPbDDZk",
  "authorities_former": "mint melt child rescript subgroup",
  "authorities_new": "none",
  "status": "Dropping all authorities"
}
```

For more information on dropping token authorities (e.g., dropping specific token authority flags while keeping others), see [Drop mint capability](UseCases_tokens_Drop-token-mint-capability.md).

_Verify transfer of authority_

To verify that the token authority is no longer in our wallet, and that it's still available on the blockchain, use the `scantokens` command
and the `token listauthorities` command in conjunction.

For more information on double checking which address holds which token authorities, see [Find token authorities](UseCases_tokens_Find-token-authorities.md).


**Postconditions**:

- Wallet Y contains a new token authority coin for token group A
- Wallet X no longer contains a token authority coin for token group A

**Related use cases**:

- Case "[Drop mint capability](UseCases_tokens_Drop-token-mint-capability.md)"
- Case "[Find token authorities](UseCases_tokens_Find-token-authorities.md)"
- Case "[View information related to a token group](UseCases_tokens_View-token-information.md)"