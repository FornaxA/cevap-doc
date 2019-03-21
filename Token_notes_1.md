# Tokens - Notes

[back to main page](README.md)

## Regtest commands

### Ensure access to the token management key  - method 1
```
setgenerate true 20
listunspent 18
```

Get the address of the transaction that has 18 confirmations. In my case:
`gAQQQjA4DCT2EZDVK6Jae4mFfB217V43Nt`
And add this to chainparams.cpp - the value of strTokenManagementKey in the class class CRegTestParams.

```
        strTokenManagementKey = "gAQQQjA4DCT2EZDVK6Jae4mFfB217V43Nt";
```
Shut down the client, compile it again, and restart the client.

### Ensure access to the token management key  - method 1
```
setgenerate true 18
importprivkey cUnScAFQYLW8J8V9bWr57yj2AopudqTd266s6QuWGMMfMix3Hff4
sendtoaddress gAQQQjA4DCT2EZDVK6Jae4mFfB217V43Nt 20
setgenerate true 18
```

### Create tokens

```
managementtoken new MAGIC MagicToken 4 https://www.google.nl 2
setgenerate true 1
getnewaddress
```

Use the groupIdentifier returned by the `managementtoken new` command (rtion1z0ukpsak22ge6t24g958ycsdad5z5thaahvwvnuwg7qre2l328wscu45hqz) and the generated address (gGnF2fuGxLQcBwDnxm44gr6uEEJ3XcRUbZ) to mint the tokens:
```
token mint rtion1z0ukpsak22ge6t24g958ycsdad5z5thaahvwvnuwg7qre2l328wscu45hqz gGnF2fuGxLQcBwDnxm44gr6uEEJ3XcRUbZ 500
setgenerate true 1
```

And check out the result:
```
token balance
```
