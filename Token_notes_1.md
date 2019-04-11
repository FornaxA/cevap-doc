# Tokens - testing

[back to main page](README.md)

## Download the client
For now, an alpha release can be downloaded here:
https://github.com/FornaxA/ion/commits/tokengroups

## Creating and sending tokens (using regtest)

First, ensure that the token debug logging is enabled through adding the following line to ioncoin.conf:
```
debug=token
```

Then, start the ion client in regtest mode using:
```
ion-qt -regtest=1
```

You'll run the following commands either from the Qt debug console or using ion-cli.

### Ensure access to the token management key
```
importprivkey cUnScAFQYLW8J8V9bWr57yj2AopudqTd266s6QuWGMMfMix3Hff4
generate 220
sendtoaddress gAQQQjA4DCT2EZDVK6Jae4mFfB217V43Nt 1
sendtoaddress gAQQQjA4DCT2EZDVK6Jae4mFfB217V43Nt 1
sendtoaddress gAQQQjA4DCT2EZDVK6Jae4mFfB217V43Nt 1
generate 6
```

### Create the new management tokens
The amount format that ION (and Bitcoin) uses, allows for a maximum of 18 decimals to be stored (see `utilstrencodings.cpp:594`). With the need to start with 71.000 DarkMatter (5 decimals), we'll have 13 decimals left for the fractional part (mantissa).
Atoms are undivisable, so they'll have 0 decimals after the decimal divisor.
The token new command contains a document URL. For the current examples, we'll use dummy data. When adopting these tokens on mainnet, the URL should refer to a JSON file in a common format. 
The last number is the document hash of the provided URL. Clients can use the hash to verify that the document has not changed since the token creation transaction. We'll again provide dummy data for now.

The managementtoken command can only be used successfully by the team that has access to the hardcoded address - and perhaps later the team that has access to the MAGIC token.

```
managementtoken new MAGIC MagicToken 4 https://wiki.lspace.org/mediawiki/Magic/ 0
generate 1
managementtoken new XDM DarkMatter 13 https://www.darkmatter.info/ 0
generate 1
managementtoken new ATOM Atom 0 https://www.ionomy.com/ 0
generate 1
```

### View which tokens exist
You can retrieve the list of tokens that live on the ION network using the `tokeninfo` command:
```
tokeninfo list
```
(You'll see that the native token `ION` has been added too.)

### Mint the management tokens
We'll send the tokens and the token authorities to a locally generated new address. Note that the tokens and token authorities can be sent to any address when they are minted.
```
getnewaddress
```

Use the groupIdentifier returned by the `managementtoken new` command (ionrt1z0ujxl2dxswjh5yeykqmm9qzzq8p6xfxrjfd0xfz0e4yt29wtnesccvnfup for MAGIC in this example) and the generated address (gGnF2fuGxLQcBwDnxm44gr6uEEJ3XcRUbZ for this example) to mint the tokens.
```
token mint ionrt1z0ujxl2dxswjh5yeykqmm9qzzq8p6xfxrjfd0xfz0e4yt29wtnesccvnfup gGnF2fuGxLQcBwDnxm44gr6uEEJ3XcRUbZ 500
generate 1
```
Do the same for DarkMatter and Atoms. Note that there will be 71000 DarkMatter and 100000 Atoms.

And check out the result:
```
token balance
```

Do the same for the other two tokens.

### Create and mint regular tokens
Regular tokens are created using the token command (not the management token command). 
Creating a new regular token costs 5x the XDM transaction fee.
Minting regular tokens also costs 5x the XDM transaction fee.
This can only be done after creating (or receiving) XDM.

```
token new CST CustomToken 8 https://www.google.com/ 0
generate 1
token mint ionrt1zd9gl4qp6jqpqp6u380u0eqgwu5rn0z2rhjq5mfvrxwlgygp4mtqq5qatxn gNRx7ujxwYhRXfBaV9436XCVKdCWmoaiup 3000000
generate 1
```

### Sending tokens
Sending tokens is done by specifying the groupIdentifier, the recipient address and the token amount.

```
token send ionrt1zvvc47dhn9ntwr45cnp9ap2jq5n96ceuw06d357lg07g2ret675sccwdc9u gH4EH72NBTcen8YUKCABkQBkzcXYABNRHM 5
generate 1
```

### Inspecting which addresses hold XDM (or any other token)
There is an RPC command that wills can the blockchain and return the addresses that hold a specific token - and their token amount.
By specifying the DarkMatter groupIdentifier, you can verify that DarkMatter fees have been sent to the address specified in chainparams.cpp. 20% of those fees will be distributed over Atom holders and Masternode owners weekly - and the other part will be burned (melted).
```
scantokens start ionrt1zvvc47dhn9ntwr45cnp9ap2jq5n96ceuw06d357lg07g2ret675sccwdc9u
```
generate 1

(Note that XDM fees are send to the Token Management Key for now, so a wallet that imports this key
will most likely send the collected fees when trying to send DarkMatter to an address. This wallet must only send DarkMatter
when distributing fees over MN owners and Atom holders).