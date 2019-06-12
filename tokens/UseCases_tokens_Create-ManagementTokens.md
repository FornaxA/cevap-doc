# Use Cases, Scenario’s, Walkthroughts - for tokens

[back to main page](README.md)

## Case "Create the DarkMatter token with a provable max of 71.000 coins"

**Scenario**: The ionomy team wants to create and distribute the XDM token according to the IEO

**Feature**: Management Tokens can be created when the Token Management Key is available

**Actor**: The Token Management Key owner

**Summary**:

Before regular users can create tokens using XDM, the following Management Tokens must be available:
- The Magic token group, which will replace the Token Management Key
- The DarkMatter token group, which users will need to spend as a fee
- The Atom token group, because Atom token owners will receive part of the DarkMatter fees

The creator of these three token groups:
- performs a *token management creation transaction* to create the respective token groups, which gives him token authorities for the three token groups. 
- mints 1000 MAGIC, 71.000 XDM and 100.000 ATOM
- destroys the DarkMatter token mint authority and the ATOM mint authority

The three token groups are *named token groups*: they have a ticker name, a full name and a website. Currently, the wallet only displays token group
names when all fields are correctly specified: 
1) Ticker name (max 8 characters, only letters)
2) Token name (max 32 characters, only letters)
3) Decimal position (between 0 and 16)
4) Token description document URL (max 79 characters, valid URL)
5) Document hash (256 bit hexadecimal string, can be 0)


**Preconditions**: 

- The network (Mainnet, Testnet or Regtest) should be past the TokenStart block height
- The actor has access to funds at the Token Management Key
- The Magic token group, DarkMatter token group and Atom token group are not yet created

**Steps**: 

ION (and other coins in the Bitcoin family) natively support coin amounts with 18 decimals (10 before and 8 after the decimal separator). 
ION tokens support a variable decimal position (determined at token group creation), but the restriction of 18 decimals remain.

- The Magic token group is defined with 4 decimals in the fractional part
- There is a maximum of 71.000 DarkMatter: 5 decimals in the integer part and 13 decimals in the fractional part
- The Atom token group is atomic, so 0 decimals in the fractional part

The command to create a management token group is: `managementtoken new [TICKER] [NAME] [DECIMALPOS] [URL] [DOCUMENTHASH]`

(This command spends ION from the Token Management Address to proof it has access to the Token Management Key. The ION is spent to a change address, so if you run out of ION at this address (`Input tx is not available for spending`), you need to send additional ION to the Token Management Address.)

Enter the following commands on regtest:

```
$ managementtoken new MAGIC MagicToken 4 https://wiki.lspace.org/mediawiki/Magic 0
{
  "groupIdentifier": "ionrt1zw655y77hx0l6m95em7ekcqm78v9v72s04yrsksup43haq0crvtqc9jyfpt",
  "transaction": "fd80ec76be10825eac045fa7999621b44e1e34f7879ffcf55c1b444dd3c77068"
}

$ generate 1
[
  "678d54d4995f162d7003d11f829a7fdbf640cf74d0f65e8f913f1b09ba7bb364"
]

$ managementtoken new XDM DarkMatter 13 https://www.darkmatter.info/ 0
{
  "groupIdentifier": "ionrt1z08suycj85usle25z7c8fy0pvqg82759dkant22edca55z5c8q0qc3czz0g",
  "transaction": "43fb5d0a4c7cdf2f1993bdfcf2d964ee00a44b8e7fd3ffab6b9aa7abf4693109"
}

$ generate 1
[
  "5c40e0377351855b1941043d12da35d894523428d2af539dab7489b2a039098b"
]

$ managementtoken new ATOM Atom 0 https://www.ionomy.com/ 0
{
  "groupIdentifier": "ionrt1z0etg43nh9g7h6p6st89vexuzcz967jy856xtzathunhpz0wn7ascea3s6c",
  "transaction": "b9f55a3485c6ce4261689f0f29ebd46d4dfab56b9e252659cbffe0c4f364e057"
}

$ generate 1
[
  "78a10149e2666a65bf71ce47a1a5dddb26a8abb2d303205a7ac299a8b119e3e3"
]
```

Verify that the token groups indeed have been created:

```
$ tokeninfo list
[
  {
    "groupIdentifier": "ionrt1zudc0feh",
    "txid": "0000000000000000000000000000000000000000000000000000000000000000",
    "ticker": "ION",
    "name": "Ion",
    "decimalPos": 8,
    "URL": "https://www.ionomy.com",
    "documentHash": "0000000000000000000000000000000000000000000000000000000000000000"
  },
  {
    "groupIdentifier": "ionrt1zw655y77hx0l6m95em7ekcqm78v9v72s04yrsksup43haq0crvtqc9jyfpt",
    "txid": "fd80ec76be10825eac045fa7999621b44e1e34f7879ffcf55c1b444dd3c77068",
    "ticker": "MAGIC",
    "name": "MagicToken",
    "decimalPos": 4,
    "URL": "https://wiki.lspace.org/mediawiki/Magic/",
    "documentHash": "0000000000000000000000000000000000000000000000000000000000000000"
  },
  {
    "groupIdentifier": "ionrt1z08suycj85usle25z7c8fy0pvqg82759dkant22edca55z5c8q0qc3czz0g",
    "txid": "43fb5d0a4c7cdf2f1993bdfcf2d964ee00a44b8e7fd3ffab6b9aa7abf4693109",
    "ticker": "XDM",
    "name": "DarkMatter",
    "decimalPos": 13,
    "URL": "https://www.darkmatter.info/",
    "documentHash": "0000000000000000000000000000000000000000000000000000000000000000"
  },
  {
    "groupIdentifier": "ionrt1z0etg43nh9g7h6p6st89vexuzcz967jy856xtzathunhpz0wn7ascea3s6c",
    "txid": "b9f55a3485c6ce4261689f0f29ebd46d4dfab56b9e252659cbffe0c4f364e057",
    "ticker": "ATOM",
    "name": "Atom",
    "decimalPos": 0,
    "URL": "https://www.ionomy.com/",
    "documentHash": "0000000000000000000000000000000000000000000000000000000000000000"
  }
]
```
(You'll see that the native token ION has been added too.)

Next, you need to mint the actual management tokens.

We'll send the tokens and the token authorities to a locally generated new address. Note that the tokens and token authorities can be sent to any address when they are minted.
You'll need to use the `groupIdentifier` returned by the `managementtoken new` command or the `tokeninfo list` command (e.g., `ionrt1z0ujxl2dxswjh5yeykqmm9qzzq8p6xfxrjfd0xfz0e4yt29wtnesccvnfup`), and you'll need a recipient ION address (the output of the `getnewaddress` command, e.g. `gGnF2fuGxLQcBwDnxm44gr6uEEJ3XcRUbZ`) to mint the tokens.

```
$ getnewaddress
gFBsCcQQvvYJ6BQUmTZzLLfS9AaeLNqZhz

# Mint 500 Magic tokens
$ token mint ionrt1zw655y77hx0l6m95em7ekcqm78v9v72s04yrsksup43haq0crvtqc9jyfpt gFBsCcQQvvYJ6BQUmTZzLLfS9AaeLNqZhz 5000
0aa5dbf903187783d4fb4d970796f1b3697b938e1474a3bbaddea7c57e3d4227

# Generate a new regtest block
$ generate 1
[
  "8a165888a27e3d2b2be8813f23afb27bd83bcac059685fd674b481e6a54c85d8"
]

# Mint 71.000 DarkMatter tokens
$ token mint ionrt1z08suycj85usle25z7c8fy0pvqg82759dkant22edca55z5c8q0qc3czz0g gFBsCcQQvvYJ6BQUmTZzLLfS9AaeLNqZhz 71000
bf2abcd5cc6fa7f7f43f25ff5a5b26a401fe66d2cf753406186e751f15529cb1

# Generate a new regtest block
$ generate 1
[
  "46c13278d58d845c648dd8e6764ca75a50aab90e2e70e296e420b33e3cd6f910"
]

# Mint 100.000 Atom tokens
$ token mint ionrt1z0etg43nh9g7h6p6st89vexuzcz967jy856xtzathunhpz0wn7ascea3s6c gFBsCcQQvvYJ6BQUmTZzLLfS9AaeLNqZhz 100000
0fbd901254cd61eeb6241795ae6ce0f120bb3119a77f0b61d87dfaa4ddbf9772

# Generate a new regtest block
$ generate 1
[
  "27e4fe2b378cbd43e1c184a7a21dd8bca62133f0ee658ee8d0a6fa5d167d7336"
]
```

And finally, verify that the correct amount of tokens have been created:
```
$ token balance
{
  "ionrt1z08suycj85usle25z7c8fy0pvqg82759dkant22edca55z5c8q0qc3czz0g": 71000.0000000000000,
  "ionrt1zw655y77hx0l6m95em7ekcqm78v9v72s04yrsksup43haq0crvtqc9jyfpt": 5000.0000,
  "ionrt1z0etg43nh9g7h6p6st89vexuzcz967jy856xtzathunhpz0wn7ascea3s6c": 100000
}
```

**Postconditions**:

- The Magic (MAGIC) token group is created
- The DarkMatter (XDM) token group is created
- The Atom (ATOM) token group is created
- 5000 MAGIC are minted
- 71.000 XDM are minted
- 100.000 ATOM are minted
- The above token amounts have been verified

**Related use cases**:

- Case "Drop token mint capability - provable token max"
- Case "Send a token"
- Case "View your token balance"
- Case "View information related to a token group"
- Case "Create a list with: addresses that hold a specific token, how many tokens they hold, and when the tokens were deposited to that address"
- Case "Create a list with: addresses that receive masternode payouts and the related MN's uptime"