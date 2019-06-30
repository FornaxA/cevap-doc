# Use Cases, Scenario’s, Walkthroughts - for tokens

[back to main page](README.md)

## Case "View information related to a token group"

**Scenario**: Perform basic operations with tokens

**Feature**: Viewing token information

**Actor**: Any user

**Summary**:

Tokens are referred to by their tokengroup identification number: a unique number that is a bech32 encoded representation 
of data that uniquely defines the token creation parameters. These parameters are:
- The token group creation transaction (the token root)
- The token group description (taken from the token creation transaction)
- The token group flags (currenlty only the flag 'management tokens' is used)

The commands to see the parameters that define the token, use the command `tokeninfo`:
- `tokeninfo all`: displays a full list of tokengroups.
- `tokeninfo stats`: display statistics about tokengroups.
- `tokeninfo ticker [TICKER]`: display all information on the token identified by this ticker.
- `tokeninfo name [TOKENNAME]`: display all information on the token identified by this token name.
- `tokeninfo groupid [TOKENGROUPID]`: display all information on the token identified by tokengroup ID.

**Preconditions**: 

No preconditions to use this command.

**Steps**: 

_Look up the token group information of XDM_

```
$ tokeninfo all
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
    "groupIdentifier": "ionrt1zd4eqkzw580qqftdax8qaq5spdce6ds9zhe7567epd5r3a53r9sqc9tz0tw",
    "txid": "18251f19739a5013681d3becc9c523ebee374f8b0410d3cc8d073078a6d56b68",
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
  },
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

$ tokeninfo name darkmatter
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

$ tokeninfo groupid ionrt1zd4eqkzw580qqftdax8qaq5spdce6ds9zhe7567epd5r3a53r9sqc9tz0tw
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

- The `tokeninfo all` command also returns the special case of the ION native token.
- Tickers and token names provided through the command line are case insensitive.

**Postconditions**:

- The requested information is displayed.

**Related use cases**:

- Case "[Create Management Tokens](UseCases_tokens_Create-Management-Tokens.md)"
- Case "[Find token authorities](UseCases_tokens_Find-token-authorities.md)"
- Case "[View token balance](UseCases_tokens_Token-balance.md)"