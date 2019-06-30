# Use Cases, Scenario’s, Walkthroughts - for tokens

[back to main page](README.md)

## Case "Token balances"

**Scenario**: Perform basic operations with tokens

**Feature**: View token balance

**Actor**: Any user

**Summary**:

The wallet's per-token balance can be retrieved with the command: `token balance (TOKENGROUPID) (IONADDRESS)`. If a wallet contains token authorities, the per-token total set of authorities is included in the output. the total of the token authorities available in the wallet. The list can be limited by tokengroup or by tokengroup and ION address.

**Preconditions**: 

No preconditions.

**Steps**: 

To show all tokens in a wallet, use the following command:
```
$ token balance
[
  {
    "groupIdentifier": "ionrt1z08suycj85usle25z7c8fy0pvqg82759dkant22edca55z5c8q0qc3czz0g",
    "balance": 0.1234567890123
  },
  {
    "groupIdentifier": "ionrt1z0etg43nh9g7h6p6st89vexuzcz967jy856xtzathunhpz0wn7ascea3s6c",
    "balance": 6
  },
  {
    "groupIdentifier": "ionrt1zw2qfu0hfuttvxwszv3980x9tsxsmdh7zmew9e7hecl436da4xfqqfj5ud6",
    "authorities": "melt"
  }
]
```

This wallet holds 0.1234567890123 XDM, 6 ATOM and a melt authority for CST.

If you want to retrieve the token group info connected to the tokengroup ID returned by the token balance command, use the
`tokeninfo groupid` command:

```
$ tokeninfo groupid ionrt1zw2qfu0hfuttvxwszv3980x9tsxsmdh7zmew9e7hecl436da4xfqqfj5ud6
[
  {
    "groupIdentifier": "ionrt1zw2qfu0hfuttvxwszv3980x9tsxsmdh7zmew9e7hecl436da4xfqqfj5ud6",
    "txid": "89b35b2b26c2208db457969da1f7de0bb6b01620410514d83175b718379ce0bc",
    "ticker": "CST",
    "name": "CustomToken",
    "decimalPos": 8,
    "URL": "https://www.google.com/",
    "documentHash": "0000000000000000000000000000000000000000000000000000000000000000"
  }
]
```

To retrieve only the balance of a specific group, specify the tokengroup ID as a parameter as follows:

```
$ token balance ionrt1z08suycj85usle25z7c8fy0pvqg82759dkant22edca55z5c8q0qc3czz0g
[
  {
    "groupIdentifier": "ionrt1z08suycj85usle25z7c8fy0pvqg82759dkant22edca55z5c8q0qc3czz0g",
    "balance": 1.1234567890123
  }
]
```

To retrieve only the balance of a specific group at a specific address, specify both parameters:

```
$ token balance ionrt1z08suycj85usle25z7c8fy0pvqg82759dkant22edca55z5c8q0qc3czz0g gDA68DU2u6So92Nw6uknEV5ieEg1WA2CiR
[
  {
    "groupIdentifier": "ionrt1z08suycj85usle25z7c8fy0pvqg82759dkant22edca55z5c8q0qc3czz0g",
    "balance": 1.0000000000000
  }
]
```

**Postconditions**:

- An overview of available tokens and token authorities is displayed

**Related use cases**:

- Case "[View information related to a token group](UseCases_tokens_View-token-information.md)"