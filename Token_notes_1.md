# Tokens - testing

[back to main page](README.md)

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
setgenerate true 19
sendtoaddress gAQQQjA4DCT2EZDVK6Jae4mFfB217V43Nt 1
sendtoaddress gAQQQjA4DCT2EZDVK6Jae4mFfB217V43Nt 1
sendtoaddress gAQQQjA4DCT2EZDVK6Jae4mFfB217V43Nt 1
setgenerate true 6
```

### Create the new management tokens
The amount format that ION (and Bitcoin) uses, allows for a maximum of 18 decimals to be stored (see `utilstrencodings.cpp:594`). With the need to start with 71.000 DarkMatter (5 decimals), we'll have 13 decimals left for the fractional part (mantissa).
Atoms are undivisable, so they'll have 0 decimals after the decimal divisor.
The token new command contains a document URL. For the current examples, we'll use dummy data. When adopting these tokens on mainnet, the URL should refer to a JSON file in a common format. 
The last number is the document hash of the provided URL. Clients can use the hash to verify that the document has not changed since the token creation transaction. We'll again provide dummy data for now.

The managementtoken command can only be used successfully by the team that has access to the hardcoded address - and perhaps later the team that has access to the MAGIC token.

```
managementtoken new MAGIC MagicToken 4 https://wiki.lspace.org/mediawiki/Magic/ 0
setgenerate true 1
managementtoken new XDM DarkMatter 13 https://www.darkmatter.info/ 0
setgenerate true 1
managementtoken new ATOM Atom 0 https://www.ionomy.com/ 0
setgenerate true 1
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

Use the groupIdentifier returned by the `managementtoken new` command (rtion1z0ukpsak22ge6t24g958ycsdad5z5thaahvwvnuwg7qre2l328wscu45hqz for MAGIC in this example) and the generated address (gGnF2fuGxLQcBwDnxm44gr6uEEJ3XcRUbZ for this example) to mint the tokens.
```
token mint rtion1z0ukpsak22ge6t24g958ycsdad5z5thaahvwvnuwg7qre2l328wscu45hqz gGnF2fuGxLQcBwDnxm44gr6uEEJ3XcRUbZ 500
setgenerate true 1
```
Do the same for DarkMatter and Atoms. Note that there will be 71.000 DarkMatter and 100.000 Atoms.

And check out the result:
```
token balance
```

Do the same for the other two tokens.

### Sending tokens
```
token send rtion1z0lxvaj2xl9qerhssxwscrjk95g6p69vhq57ajl0m5wraykz7a3qc0t47gs gH4EH72NBTcen8YUKCABkQBkzcXYABNRHM 5
```

### Inspecting who has XDM

```
scantokens start rtion1z0lxvaj2xl9qerhssxwscrjk95g6p69vhq57ajl0m5wraykz7a3qc0t47gs
```