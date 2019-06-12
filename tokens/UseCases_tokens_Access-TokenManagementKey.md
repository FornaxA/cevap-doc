# Use Cases, Scenario’s, Walkthroughts - for tokens

[back to main page](README.md)

## Case "Access the TokenManagementKey"

**Scenario**: The ionomy team wants to use Token Management functions

**Feature**: The TokenManagementKey unlocks Token Management functions

**Actor**: A token group manager

**Summary**:

The Token Managment public key is defined in src/chainparams.cpp. Like the spork key, this key is different for Mainnet, Testnet and Regtest.
(Ultimately, this hard coded key is replaced by a limited token (MagicToken), which is easier to secure by using multi signature addresses and key rotation.)
Token Management Transactions demonstrate that they are performed by somebody with access to the Token Management Key by spending ION from the Token Management Address.
- The actor needs to import the Token Management Private Key.
- The actor needs to ensure the Token Management Address has funds (ION).

**Preconditions**: 

- A running node (See e.g. [Case "Start Regtest"](UseCases_regtest_Start-Regtest.md))
- The network (Mainnet, Testnet or Regtest) should be past the TokenStart block height
- The network has not yet switched to the use of the Magic Token

**Steps**: 

Regtest's Token Management Public Key is `gAQQQjA4DCT2EZDVK6Jae4mFfB217V43Nt` (as defined in `src/chainparams.cpp`), with the corresponding Token Management Private Key `cUnScAFQYLW8J8V9bWr57yj2AopudqTd266s6QuWGMMfMix3Hff4`. The Token Management Private Keys for Mainnet and Testnet are not publicly available. 

```
# Get access to the Token Management Key by importing the Token Management Private Key
importprivkey cUnScAFQYLW8J8V9bWr57yj2AopudqTd266s6QuWGMMfMix3Hff4

# Move funds into the Token Management Address
sendtoaddress gAQQQjA4DCT2EZDVK6Jae4mFfB217V43Nt 1
sendtoaddress gAQQQjA4DCT2EZDVK6Jae4mFfB217V43Nt 1
sendtoaddress gAQQQjA4DCT2EZDVK6Jae4mFfB217V43Nt 1

# Mine 6 Regtest blocks to ensure the funds in the Token Management Address are available for spending
generate 6
```

**Postconditions**:

- The actor now has a wallet.dat that has access to the Token Management Key
- The Token Management Address holds funds (ION) needed to create Token Management Transactions

**Related use cases**:

- Case "Create DarkMatter"