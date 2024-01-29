---
# Copyright (c) 2024 Bitwise IO, Inc.
# Copyright (c) 2018, Intel Corporation.
# Licensed under Creative Commons Attribution 4.0 International License
# <https://creativecommons.org/licenses/by/4.0/>
---

# Sawtooth FAQ: Sawtooth in General

## What is Sawtooth?

Sawtooth is a modular enterprise blockchain platform for
building, deploying, and running distributed ledgers. The design
philosophy targets keeping ledgers distributed and making smart
contracts safe, particularly for enterprise use. Sawtooth
includes a novel consensus algorithm, Proof of Elapsed Time (PoET),
which targets large distributed validator populations with minimal
resource consumption. No special hardware is required to run Sawtooth or
PoET.

## What are some useful Sawtooth links?

Sawtooth introduction

:   <https://sawtooth.splinter.dev/docs/1.2/>


GitHub repository for Sawtooth Core

:   <https://github.com/splintercommunity/sawtooth-core>

Sawtooth documentation, with several guides and references, including:

:   <https://sawtooth.splinter.dev/>

Sawtooth Application Developer\'s Guide

:   <https://sawtooth.splinter.dev/docs/1.2/app_developers_guide/>

Sawtooth Architecture

:   <https://sawtooth.splinter.dev/docs/1.2/architecture/>


Sawtooth Discord Channel

:   <https://discord.com/invite/fnUmDv7tSH>


Sawtooth FAQ

:   <https://sawtooth.splinter.dev/faq/>


## What differentiates Sawtooth from other blockchains?

This includes:

-   State agreement, which assures each node has
    cryptographically-verifiable, identical copies of the blockchain
-   novel Byzantine Fault Tolerant (BFT) consensus, through PoET
-   Unpluggable consensus on-the-fly (without restarting)
-   Multi-language SDK support (Python, Go, Javascript, Rust, with more
    being added)
-   Parallel transaction processing

## Should I use Sawtooth or other blockchain software for my application?

You should look for existing blockchain platforms that will fit your use
case, sort them out by features, maturity (are they production ready?),
and community support. We hope Sawtooth fits your needs.

## How does a blockchain differ from a database?

-   A database has one master copy. A blockchain has multiple
    authoritative copies
-   A database can be changed after a commit. A blockchain\'s records
    are immutable and cannot be undone after a commit
-   A database must have a trusted central authority

## What does an immutable blockchain mean?

It means that blocks already committed cannot be \"undone\" or deleted.
The block\'s transactions are in the blockchain forever. The only way to
undo a transaction is to add another transaction to reverse a previous
transaction. So if the value of `a=1` and a transaction sets `a=2`, the
only way to undo it is to set `a=1` again. However regardless of what
the current value of `a` is, all three of those transactions are
permanently a part of the blockchain. The record of them will never be
lost, and in fact you could rewind state to what it was in previous
blocks if you needed.

This is different from immutable variables. The difference is that with
blockchain *transactions* are immutable. With some programming languages
(such as Rust), *variables* are immutable.

## How do I tell what version of Sawtooth is running?

```sh
$ sawtooth --version
sawtooth-cli (Sawtooth) version 1.1.2
```

<h2 id="diff-between-saw-cli"> What's the difference between the
`sawtooth`, `sawadm`, `sawnet`, and `sawset` commands?</h2>

`sawadm`

:   Administration tasks such as creating the genesis batch file or
    validator key generation

`sawnet`

:   Interact with Sawtooth network, such as comparing chains across
    nodes

`sawset`

:   Change genesis block settings or views, create, and vote on new
    block proposals

`sawtooth`

:   Interact with a Sawtooth validator, such as batches, blocks,
    identity, keygen, peers, settings, state, and transaction
    information

For more information, see the Sawtooth CLI Command Reference at
<https://sawtooth.splinter.dev/docs/1.2/cli/sawtooth.html>

## Must software developed with Sawtooth be open source?

IANAL; however, Sawtooth is released under the Apache 2 license, a
permissive license, and so should be able to be used in both open and
closed source applications.

## Can I copy a Sawtooth Core source file to include with my project?

Yes, if you follow the Apache 2 license terms, which include requiring
preserving copyright and license notices. Sawtooth depends on other
runtime software that has separate terms.

## I get a usage error running `sawnet peers` or `sawnet list-blocks`

These commands were added after the Sawtooth 1.0.5 release and are not
available in earlier releases.

## How do I detect a forked blockchain in a Sawtooth network?

Use [`sawnet compare-chains`]({% link docs/1.2/cli/sawnet.md %}#sawnet-compare-chains)
and look for a different set of
block(s) at the head of the chains. This is distinct from the case where
one node has a blockchain that\'s not up-to-date, but has conflicting
heads (\"forked\"). Forking can occur if the Sawtooth network is
partitioned and cannot fully communicate. It can also be the result of a
bug in transaction processing (for example, transactions don\'t
serialize in a deterministic way).

<h2 id="failed-to-reach-common-ancestor"> What does `Failed to reach common
ancestor` mean from `sawnet compare-chains`?</h2>

It means the blockchains have no blocks in common, including the genesis
block. This usually happens when a second node is added with its own
genesis node. Only the first node in a Sawtooth network should be
created with a genesis block.

## How do I report a bug?

Please submit a GitHub issue at <https://github.com/splintercommunity/sawtooth-core/issues>

## What encryption algorithms are used by Sawtooth?

-   Transaction signing with ECDSA 256-bit key using curve secp256k1
    (same as Bitcoin)
-   ZeroMQ (ZMQ or 0MQ) used for communications. ZMQ uses CurveZMQ for
    encryption and authentication, which uses ECDH 256-bit key with
    curve Curve25519 for key agreement.
-   PoET uses AES-GCM to encrypt its monotonic counter
-   Names are hashed with SHA-512 or SHA-256

## Can you explain Global State with an example?

Global state is where sawtooth and TPs read/write blockchain data.
Examples are a-plenty if you look at the github repo examples (intkey,
XO, etc.) The \"state\" is implemented as a Radix Merkle Trie over the
LMDB database, where the \'keys\' are 35 bytes (70 characters) and the
scheme for the keys is up to the TP developer. The first 3 bytes (6
chars) of the key identifies a unique TP namespace and it is recommended
to avoid colliding with other TP namespaces. To enable your TP to
read/write (or in context parlance \"get/set\") data at addresses, you
need to specify those addresses *a priori* in the Transaction
inputs/outputs. Otherwise you will get Authorization errors. The
addresses your TP will read or write to need to be deterministic.

Using the SimpleWallet application as an example (see example
application links above), the blockchain will contain transactions
showing deposits, withdrawals and transfers between accounts. The global
state will contain the balance in the different accounts corresponding
at the current point in time, after all transactions in the chain have
been processed.

## What is the difference between the Merkle Radix Trie and the blockchain?

The blockchain itself just stores transactions, not state, so reading
the data in the last block does not say much by itself. Data in the
blockchain is also immutable and can never change (except by adding new
blocks). The radix trie is a different data structure that is used to
make fast queries to the state. The root of the Merkle Trie is a hash.
One can easily identify if something changed when the root hash changes.
The Merkle Trie addressing allows quick retrieval at an address and
partial queries of address prefixes.

## Are 32-byte IDs within a transaction family large enough to avoid collisions?

Yes. If they are being generated with a random distribution, the chances
are vanishingly rare. A UUID is only 16-bytes and if you generated a
billion per second, it would take 100 years before you would expect 50%
odds of a collision.

## Why is Sawtooth capable of supporting large network populations of nodes?

One of the reasons is the homogeneous nature of Sawtooth Nodes. You
don\'t have different nodes with specialized functions, so it\'s easy to
setup and manage many nodes. Secondly, and more importantly, the PoET
consensus mechanism has been designed for large networks. It\'s not very
efficient in small networks and you\'ll likely get much better
performance with other mechanisms in a small network, but PoET handles
large populations easily.

## Are there any examples of Sawtooth permissions?

-   off-chain permissioning is in `/etc/sawtooth/validator.toml` (see
    `validator.toml.example` )
-   on-chaining permissioning is recorded on-chain. See block 0 for
    examples, such as `sawtooth.settings.vote.authorized_keys`
-   transaction key permissioning controls what clients can submit
    transactions, based on signing keys
    (`transactor.transaction_signer`,
    `transaction.transaction_signer.<name of TP>`,
    `transactor.batch_signer` )
-   validation key permissioning controls what nodes are allowed to
    connect to the Sawtooth network
-   transaction family permissioning controls what TFs are supported by
    this Sawtooth network, `sawtooth.validator.transaction_families`
-   then there are policies and roles from the optional Sawtooth
    Identity Transaction Processor, documented at
    <https://sawtooth.splinter.dev/docs/1.2/transaction_family_specifications/identity_transaction_family.html>

<h2 id="does-restore-state-peer">Does Sawtooth restore state when a peer
restarts or when a peer is åout-of-sync with the network?</h2>

Yes.

<h2 id="when-content-changed-merkle"> When content at an address is changed
several times by the transactions in a block, what appears in the
state (Merkle Tree)?</h2>

The only thing that hits state is the aggregate (final) set of address
changes due to the transactions in the block. If multiple transactions
in a single block modify an address, there will only be one \'set\'. You
could see the transaction level changes in the receipts if you needed
to.

<h2 id="create-app-clone-repo"> In order to create a Sawtooth application, do I
need to clone and modify  the entire `sawtooth-core` repository?</h2>

No. It can be done that way, but it\'s not recommended. All you need to
write is the client application and the Transaction Processor. The core
Sawtooth functionality should be installed as packages instead of being
built from source and integrated with your application.

## What is Sawtooth *global state agreement*?

Sawtooth writes state to a verifiable structure called a *Radix Merkle
Trie* and the verification part (the root hash) is included in the
consensus process. That means that agreement is not just on the ordering
of transactions but also on the resulting contents of the entire
database.

This guards against a variety of possible failures during the
application of a transaction (e.g. different library version installed,
a write failure, a local database corruption, numerical representation
differences).

Of course the feature is mainly targeted at protecting the integrity of
a production network, but it is also helpful during development. Running
applications over test networks can help identify nondeterminism and
that will only be apparent if you form consensus over state.

## How can CPU vulnerabilities such as Spectre and Meltdown impact Sawtooth?

Sawtooth is a CPU-agnostic blockchain platform. It includes an optional
TEE/SGX feature which enhances BFT protections for PoET. PoET is
designed following a defense-in-depth approach. There are three or so
mechanisms that work in different aspects of the protocol independently
from the TEE. This includes three tests performed by PoET:

-   c-test: A node must wait c blocks after admission before its blocks
    will be accepted - this is to prevent trying to game identities and
    some obscure corner scenarios.
-   K-test: The node can publish at most K blocks before its peers
    require it to recertify itself.
-   z-test: And perhaps most importantly a node may not publish at
    frequency greater than z

Finally, should a node run a compromised consensus protocol, the main
characteristic at risk would be *fairness*. It would not be able to
impact *correctness* network-wide. That is, it cannot publish invalid
transactions. If it does the other nodes will just reject those
transactions and the associated block(s) and they will not commit
network-wide.

## Are Docker containers required to run Sawtooth?

Docker is a quick and easy way to get Sawtooth up and running. However, Sawtooth
does not require Docker.

Follow the instructions to run on Ubuntu at
https://sawtooth.splinter.dev/docs/1.2/app_developers_guide/creating_sawtooth_network.html#using-ubuntu-for-a-sawtooth-test-network>
For specific apps, you can run without docker by manually running
commands in a `Dockerfile` as follows:

-   Install Sawtooth on an Ubuntu following the instructions in the
    *Sawtooth Applications Developer\'s Guide*
-   Create the Genesis Block. See Guide in previous step
-   Install required packages listed under the RUN line in the
    `Dockerfile` for each container
-   Install your application\'s transaction processor and client.
-   Make sure your client app connects to the REST API at
    `http://localhost:8008` instead of `http://rest-api:8008`
-   Make sure your transaction processor connects to
    `tcp://localhost:4004` instead of `tcp://validator:4004`
-   Start the Validator, REST API, and Settings TP:
    `sudo -u sawtooth sawtooth-validator -vv &`
    `sudo -u sawtooth sawtooth-rest-api -vvv &`
    `sudo -u sawtooth settings-tp -vv &`
-   Start your application-specific transaction processor(s). See the
    `CMD` line in the `Dockerfile` for your TP
-   Start your application client (see `CMD` in your client
    `Dockerfile`)

## What cloud services support Sawtooth Blockchain?

AWS offers Sawtooth, and other cloud providers plan to offer Sawtooth on
their cloud service.

## Does Sawtooth use blockchain mining?

No. There is no inherent need to incentivize miners in a
private/permissioned blockchain. Part of the permissioned model is that
everyone involved has a personal stake in the verifying the data, so you
do not need to pay them. This contrasts with a public deployment where
you are asking strangers to verify the data for you. In that case you
probably do need to incentivize them somehow, and a currency is a common
way to do so.

## What is the \"head node\" or \"master node\" in Sawtooth?

Sawtooth has no concept of a \"head node\" or \"master node\". Once
multiple nodes are up and running, each node has the same genesis block
(block 0) and treats all other nodes as peers. The first validator node
on the network has no special meaning, other than being the node that
created the genesis block.

## What is a Sawtooth Role?

A Role is a set if permissions. Identities could be assigned one or more
roles. A role is a convenient shorthand because role(s) can be assigned
to several identities rather than tediously assigning individual
permissions to each identity. See
<https://sawtooth.splinter.dev/docs/1.2/sysadmin_guide/configuring_permissions.html>

## What Sawtooth Roles are defined?

transactor

:   who can sign transactions and batches

transactor.batch_signer

:   who can sign batches

transactor.transaction_signer

:   who can sign transactions

transaction.transaction_signer.\<transaction processor name>

:   who can sign transactions for a specific TP

network

:   nodes authorized to make peer requests

network.consensus

:   nodes authorized to broadcast new blocks with Gossip
