# Token  based data and identity management

One of the strengths of blockchain based systems is its capacity to provide transparancy and to manage trust trough encryption.

We will showcase two different means to demonstrate ownership: 1) control over a private key belonging to a blockchain address and 2) control over a GPG private key belonging to a public key. This is combined in an ecosystem comprising of a game studio, a game fan site and the blockchain, where a gamer can proof on a fan site (e.g. as part of a discussion forum) that he is a specific gamer (e.g., the winner of that week's tournament).

## Context

In two workshops, ProvenStack and students went through the basics of:
- Manual encoding, decoding and writing data on blockchain families based on the bitcoin core
- Setup and use a full developer environment for bitcoin core development
- Adding remote procedure calls to their blockchain clients
- Compile their modified clients

During this workshop, we will:
- Create and share a gpg key for encrypting, decrypting and signing messages
- Alpha test an utxo based token system for bitcoin families
- Link web technology to the blockchain
- Combine tokens with on-chain data to create new functionality
- Collaborate within a team using git and github

## Case description

Game studio BlackDiamond runs a game called Shooting Stars. The game knows a weekly contest, where each week the top 10 players with the highest scores win a price. Their winnings are paid in the ION currency, and the leaderboard (names and scores of the top 10 players) is published to the ION blockchain - while only the current top 10 is published on their website.

Shooting Stars fan site Perseid.org hosts a forum where gamers and other fans can discuss the game, and show off their achievements. Perseid.org offers encrypted communication to their users, and uses GPG to enable this because GPG is a well-established technology for creating [trusted communication channels](https://blog.mailfence.com/openpgp-public-key/) and to [make your fingerprint know publicly](https://jacob.hoffman-andrews.com/README/the-safe-way-to-put-a-pgp-key-in-your-twitter-bio/). There is no official link between BlackDiamond and Perseid, so the forum members have (until now) no means to validate the claims that other forum members make about their position.

During this workshop, we will implement a system where:
1) the game studio pays the top 10 prices and publishes the top 10 to the blockchain using a web interface
2) a forum member can submit a claim that he is the gamer who achieved a specific high score
3) the forum can publicly send a challenge to that gamer, which he can only solve if he is both the owner of the forum account and the recipient of the in-game price

### System design criteria

The system should be designed in a way that the blockchain offers transparency
- The chain stores the following information on the Leaderboard items: event identifier, player's rank, player's name, player's score.
- The chain stores the challenges that the forum sent to its forum members, and open challenges are easy to find.
- The chain stores proof when a challenge has been met successfully.

The impact on the blockchain should be minimized, which means that some data stays off-chain, new core functionality is avoided when possible, and available funtionality is used to its fullest extent. More specifically:
- The fansite stores the GPG public keys of the users; the public key is not written to the blockchain and there is no encrypted or signed data written to the blockchain.
- Whenever possible, we will use tokens and token subgroups to tag data stored on the blockchain because that makes the data easy and cheap to retrieve.
- 

## Prerequisites

## Input material

## Exercise description
