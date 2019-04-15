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

## Prerequisites

This exercise depends on the completion of all steps in the workshops on:
- Encoding and decoding data manually: [Workshop Windesheim 14 November 2018
]](https://provenstack.atlassian.net/wiki/spaces/EDUCATION/pages/753801/Workshop+Windesheim+14+November+2018)
- Compiling and extending bitcoin-based blockchains: [Workshop Windesheim 7 March 2019](https://provenstack.atlassian.net/wiki/spaces/EDUCATION/pages/27656232/Workshop+Windesheim+7+March+2019)

Specifically, ensure the following:
- 

## Workshop preparations

The workshop will take place at Windesheim on Wednesday 24 April 2019. ProvenStack will host a conference call to discuss the workshop preparations and to discuss questions regarding the case description. During the workhop itself, the students will work in teams on the different aspects of the system. Each self-assigned team will deliver a part of the system, and upload the resulting source code to Github.
In preparation of this collaborative effort, students are advised to take the following actions:

1) Ensure you have access to a Githib account that you can use during this workshop: either create a new account or confirm you have access to an existing account.
2) Create a new SSH key
    Connect the public key to your Github account
    Send the public key to the Workshop leaders for access to the web server
3) Create a new GPG key
    Upload the GPG public key to the [Ubuntu key server](https://keyserver.ubuntu.com/)
    Share the GPG public key with the Workshop leaders so they can prepare the forum web page
4) Fork ION's community edition Github repository: https://github.com/cevap/ion to your own Github account
5) Submit your GPG public key to the branch `windesheim-ws3` on cevap's ion repository, as follows:
   1) Clone your fork and checkout its `windesheim-ws3` branch
   2) Copy your GPG public key to the subfolder `/contrib/gpg` in the source folder.
   3) Add, commit and push the GPG public key either using the command line or a GUI such as gitkraken
   4) Create a pull request from your fork to the cevap/ion repository (branch `windesheim-ws3`)
6) Install VSCode Live Share for easy real-time collaboration (VSCode screen sharing, chat) during the workshop

(TODO: add links with manuals for the above steps)

## Exercise description

## Exercise steps

### Compile your fork
and `checkout` the `windesheim-testnet` branch
    Compile the source code; see [Workshop Windesheim 7 March 2019](https://provenstack.atlassian.net/wiki/spaces/EDUCATION/pages/27656232/Workshop+Windesheim+7+March+2019).

## Input material

