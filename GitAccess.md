# Git Access

## Community Git: https://github.com/cevap/ion

With the ION community taking over responsibility of coin development, Ionomy can focus on game development and on the development of the ION related economy.

ION Coin development takes place on Github. Git is a version control system that enables easy and robust collaboration between loosely coupled teams and individuals. 
In addition to enabling teams to develop and share code, Github arranges for a wide selection of features that fit perfectly to development communities, such as:
- Issue tracking (See [ION issue tracking](https://github.com/ionomy/ion/issues) and [CEVAP ION issue tracking](https://github.com/cevap/ion/issues))
- A Wiki (See the [Community Ionomy Wiki](https://github.com/cevap/ion/wiki))
- Simple, static web pages (Such as [this manual itself](https://cevap.github.io/doc/))

While real-time interation with the Ionomy community and the ION community takes place in [Slack](https://ionomy.slack.com/) and at the [ION Community Forum](https://ion.community/), the ION community's development place is the [community github](https://github.com/cevap/ion).

## ION Community Edition Git Primer

An excellent git primer can be found [here](https://dont-be-afraid-to-commit.readthedocs.io/en/latest/).

Simplified workflow of interaction between Ionomy ION git and ION Community Edition (CE) git:
1. ION Community developers develop on the ION CE repository until they agree the branch or feature is ready to be sent to the Ionomy ION repository
2. An ION Community developer subits a pull request (PR) to the Ionomy ION repository
3. An Ionomy representative responds to the PR

A community member can start working on the ION CE git as follows. Contributor X:
- clones ION CE repository if it is the first time he uses the repository
- pulls updates from the git repository
- checks out the relevant branch
- works on his files
- stages files to be uploaded to the repository
- commits these changes and adds a descriptive message
- pushes the changes to the repository

Note that a repository is typically cloned only once; after that, updates are received by pulling them.

It is good to get to know the command line interface for git, and perhaps edit files using plain text editors. However, there are plenty of great tools that simplify and speed up contributing to git repositories, such as:
- Git Desktop
- VSCode
- Tortoise Git

## Getting ION Edition Community git access

Access to the ION CE repository is provided by authenticating with a security key. 
The public part of this key should be made publicly available, with a hash to identify the person and the public key located in the ION CE repository. When a hash and corresponding public key is accepted into the ION CE repository, this is done by a person that has already been validated himself. This creates a chain of trust.

1. Generate a GPG key (see README.md from tor on this), with an email address linked to your github account. 
    - Export this key using `gpg --armor --export <mail address>`
    - Submit the (public) 'ASCII-armored PGP key' to e.g. http://pgp.mit.edu/
    - Send the public key to each place where you need to get access; for example, you could ask a contributor to upload this public key to the ION CE gitian folder.
    - Import GPG keys using the command line: `gpg --import <filemame>`
    - Decrypt GPG messages using the command line: `gpg --decrypt <filename>`
    - Encrypt GPG messages using the command line: `gpg -a --encrypt <filename>`; -a tells gpg to produce ascii-readable output.
2. Generate a RSA key; the private key is stored on the system that needs to acces github, and the public key is stored on github (or at any other system where you need to get access)

For more information on GPG keys and key exchange, see the [gnupg FAQ](https://www.gnupg.org/gph/en/manual/x56.html).

## ION Community Edition git organization

Currently, the following contributors are identified:
- tor: <link to public key>
- aspaas: <link to public key>

The contributor public keys are located at https://github.com/cevap/ion/tree/master/contrib/publickeys

## TODO
- [ ] Tailer and apply the linked Git Primer to the ION Community Git