# Use Cases, Scenario’s, Walkthroughts - for tokens

[back to main page](README.md)

## Introduction

As part of the integration of game development functionality and blockchain technology, the ION community chose to adopt a token system as part of
its blockchain core. The [community approved](https://twitter.com/ion_community/status/1017804434724843521) proposal [IIP 0002](https://github.com/ionomy/iips/blob/master/IIP_0002.md) was [put to vote](https://cevap.github.io/doc/IIP_vote_2.html) in July 2018, after which development started. Instead of developing a solution from scratch, the team assessed a number of proposals and implementations that
were currently being worked on for other Bitcoin family coins. Selection criteria were:
- Fully open, with active development
- Emphasis on permissionless transactions
- Efficient in terms of resource consumption
- Simple and elegant underlying principles

The token system implemented is based on the Group Tokenization proposal by Andrew Stone / BU.

References:
- [GROUP Tokenization specification by Andrew Stone](https://docs.google.com/document/d/1X-yrqBJNj6oGPku49krZqTMGNNEWnUJBRFjX7fJXvTs/edit#heading=h.sn65kz74jmuf)
- [GROUP Tokenization reference implementation for Bitcoin Cash](https://github.com/gandrewstone/BitcoinUnlimited/commits/tokengroups)

For the technical principles underlying ION Group Tokenization, the above documentation is used as our standard.

ION developers fine tuned, extended and customized the Group Tokenization implementation. This documentation aims to support the ION
community in:
- Using the ION group token system
- Creating additional tests as part of the development process
- Finding new use cases that need development support

## Scenario's

### Perform basic operations with tokens
- Case "[Send tokens](UseCases_tokens_Send-tokens.md)"
- Case "[Token balance](UseCases_tokens_Token-balance.md)"
- Case "[View information related to a token group](UseCases_tokens_View-token-information.md)"

### Testing tokens on regtest

- Case "[Start Regtest](UseCases_regtest_Start-Regtest.md)"
- Case "[Access Management Tokens](UseCases_regtest_Access-Token-Management-Key.md)

### Token group management
- Case "[Key rotation with token authorities](UseCases_tokens_Key-rotation-with-token-authorities.md)"

### Create and distribute the XDM token according to the IEO

- Case "[Create Management Tokens](UseCases_tokens_Create-Management-Tokens.md)"
- Case "[Drop mint capability](UseCases_tokens_Drop-token-mint-capability.md)"
- Case "[Find token authorities](UseCases_tokens_Find-token-authorities.md)"
