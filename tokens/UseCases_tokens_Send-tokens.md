# Use Cases, Scenario’s, Walkthroughts - for tokens

[back to main page](README.md)

## Case "Send tokens"

**Scenario**: Perform basic operations with tokens

**Feature**: Sending tokens

**Actor**: Any user

**Summary**:

When sending tokens, a user needs to specify the tokengroup ID, the recipient address and the amount of tokens to send.
(At the moment, coin control isn't implemented.)

- While the precision of regular ION transactions is 8 decimals after the decimal separator (which is the same as with Bitcoin), a token
creator can specify how many decimals after the decimal separator he wants for his token group. DarkMatter for example knows a 13
decimal precision after the decimal separator.
- The tokengroup ID is a unique identifier that is linked to a token group. For more information on linking tokengroup ID's to token names
and token tickers, see the case "[View information related to a token group](UseCases_tokens_View-token-information.md)".

The command to use is: `token send [TOKENGROUPID] [IONADDRESS] [AMOUNT]`

**Preconditions**: 

- The user must have tokens to send

**Steps**: 

If you do not know the tokengroup ID of the token you want to send, find the tokengroup ID:
```
$ tokeninfo ticker XDM
[
  {
    "groupIdentifier": "ionrt1z08suycj85usle25z7c8fy0pvqg82759dkant22edca55z5c8q0qc3czz0g",
    "txid": "43fb5d0a4c7cdf2f1993bdfcf2d964ee00a44b8e7fd3ffab6b9aa7abf4693109",
    "ticker": "XDM",
    "name": "DarkMatter",
    "decimalPos": 13,
    "URL": "https://www.darkmatter.info/",
    "documentHash": "0000000000000000000000000000000000000000000000000000000000000000"
  }
]
```

Send the token:
```
# Send 0.1234567890123 XDM to address g5LvnLSdmGJW97fSNYBTu5GgZySY6v9rVv
$ token send ionrt1z08suycj85usle25z7c8fy0pvqg82759dkant22edca55z5c8q0qc3czz0g g5LvnLSdmGJW97fSNYBTu5GgZySY6v9rVv 0.1234567890123
f12de18eda9d9764152f884bb0d32fd14607b2baa86ddda29679b91c3bc7118c
```

**Postconditions**:

- The tokens have been send to the recipient
- When needed, fees are paid automatically

**Related use cases**:

- Case "[View information related to a token group](UseCases_tokens_View-token-information.md)"