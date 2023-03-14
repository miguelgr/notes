---
title: IPFS
description: Interplanetary File System
topic: ipfs
---
### Evolution to web3

web (wires, network)
web 1 (read-only static)
web 2 (read-write interactive)
Web 3 (read-write trust and verifiable) is a combination of:

- Distributed Web

  - Work offline
  - Faster
  - Smarter
  - No need of central point of coordination
  - Permanent links

- Blockchain

  - Permissionless
  - Incentive alignment
  - Publicly verifiable

- Semantic Web

  - Graph of Knowledge
  - Programmable understanding of meaning


it enables **Open Services**

- Open Source: publicly auditable
- Forkability: in disagreement, part ways and experiment
- Permissionless entry: Anyone can join and contribute
- Programmable Agreements: provide service over time, access control
- CryptoEconomics: Incentive structures to align participants and optimize value

### IPFS - Inter-Planetary File system

> We need a protocol designed fo future proof the web, make it resilient, work distributed, permantent links, faster, smarter (content addressing) and with no central protocol coordination.

IPFS is an open-source project created by [Protocol Labs](https://protocol.ai/?ref=hackernoon.com), an R&D lab for network protocols and former Y Combinator startup. Protocol Labs also develops complementary systems like [IPLD](https://ipld.io/?ref=hackernoon.com) and [Filecoin](https://coincentral.com/filecoin-beginners-guide-largest-ever-ico/?ref=hackernoon.com)

**IPFS is a global, versioned, peer-to-peer filesystem.**

It combines good ideas from [Git](https://git-scm.com/), [BitTorrent](https://en.wikipedia.org/wiki/BitTorrent), [Kademlia](https://en.wikipedia.org/wiki/Kademlia), [SFS](https://en.wikipedia.org/wiki/Self-certifying_File_System), and the Web. It is like a single bit-torrent swarm, exchanging git objects. IPFS provides an interface as simple as the HTTP web, but with permanence built-in.

IPFS is a **protocol**:
- defines a content-addressed file system
- coordinates content delivery
- combines Kademlia + BitTorrent + Git

IPFS is a **filesystem**:
- has directories and files
- mountable filesystem (via FUSE)

IPFS is a **web**:
- can be used to view documents like the web
- files accessible via HTTP at `http://ipfs.io/<path>`
- browsers or extensions can learn to use `ipfs://` directly
- hash-addressed content guarantees the authenticity

IPFS is **p2p**:
- worldwide peer-to-peer file transfers
- completely decentralized architecture
- **no** central point of failure

IPFS is **modular**:
- connection layer over any network protocol
- routing layer
- uses a routing layer DHT (kademlia/coral)
- uses a path-based naming service
- uses BitTorrent-inspired block exchange

IPFS **uses crypto**:
- cryptographic-hash content addressing
- block-level deduplication
- file integrity + versioning
- filesystem-level encryption + signing support

IPFS is a **CDN**:
- add a file to the filesystem locally, and it's now available to the world
- caching-friendly (content-hash naming)
- BitTorrent-based bandwidth distribution

IPFS has a name service:

- IPNS, an SFS inspired name system
- global namespace based on PKI
- serves to build trust chains
- compatible with other NSes
- can map DNS, .onion, .bit, etc to IPNS

### For whom is IPFS:

- Archivists: Storing archival data using **IPFS enables deduplication, clustered persistence, and high performance** — empowering you to store the world's information for future generations.

- Service providers: Providing large amounts of data to users? Storing on IPFS could help you slash bandwidth costs thanks to its use of **secure, peer-to-peer content delivery**.

- Researchers: If you're working with or distributing large datasets, storing that data using IPFS can help speed up performance and **unlock decentralized archiving**.

- Blockchain developers: IPFS content addressing enables you to store large files off-chain and put immutable, permanent links in transactions — timestamping and securing content without having to put the data itself on-chain.

- Content creators: IPFS empowers creators to build and share on the decentralized web — whether that's delivering content free from intermediary control or minting NFTs that stand the test of time.

- Offline users: High-latency networks cause major obstacles for those with poor internet infrastructure. Peer-to-peer IPFS offers **resilient access to data independent of latency or backbone connectivity.**

## Crash course

### Adding a file to IPFS

Go install [ipfs](https://github.com/ipfs/kubo)

```
echo "hello world" > hello.txt
ipfs add hello.txt
# added QmT78zSuBmuS4z925WZfrqQ1qHaJ56DQaTfyMUF7F8ff5o hello.txt
# 12 B / 12 B [===================] 100.00%
ipfs cat  QmT78zSuBmuS4z925WZfrqQ1qHaJ56DQaTfyMUF7F8ff5o
# hello world
```

To replicate content, you must take your node online, join the p2p network, and pin the specific CID from another node.

“The ipfs add command will create a Merkle DAG out of the data following the UnixFS data format. Your content is broken down into blocks using a Chunker, and then arranged in a tree-like structure using 'link nodes' to tie them together. The returned CID is the hash of the root node in the DAG.”


## Content Addressing
### Location Addressing vs Content Addressing

We expect our libp2p logo to be available at https://proto.school/tutorial-assets/T0009L01-libp2p-logo.svg, but what if:

Drawbacks:

- URL points to a single copy in a single location
- If copy dissapears? no way to kwnow where other copies are.
- How do I validate integrity (DNS spoofing, change the content)
- HTTP secures the wire but not the content
- URL encoding

Location addressing issues:

- the server of this website is down?
- the DNS server is down?
- the image was converted to a PNG file and so the new location is T0009L01-libp2p-logo.png?
- you are in a country that has blocked the proto.school domain?

### Content addressing

**Immutable**:  hashed content.
**Secure**: authenticates the content.
**De-duplication**: same data, same address
**Chunking**: avoids dedup, piecewise transfer (retry chunk, instead of whole file), random access (like watching a video of youtube at a certain moment)

## CIDs

**CIDs** (Content Identifiers), the unique labels used to point to data stored on distributed information systems including IPFS, IPLD, libp2p, and Filecoin.

- Deduplication
- Self-certification
- Immutability

The cryptographic algorithm used must generate hashes that have the following characteristics:

- Deterministic: The same input should always produce the same hash.
- Uncorrelated: A small change in the input should generate a completely different hash.
- One-way: It should be infeasible to reconstruct the data from the hash.
- Unique: Only one file can produce one specific hash.

Cryptographic hashing is not unique to IPFS, and there are many hashing algorithms out etc.**IPFS uses sha2-256 by default**, though a CID supports virtually any strong cryptographic hash algorithm. **All of the same size, 256 bits, which equates to 32 bytes**

[QmRBkKi1PnthqaBaiZnXML6fH6PNqCFdpcBxGYXoUQfp6z](https://ipfs.io/ipfs/QmcRD4wkPPi6dig81r5sLj9Zm1gDCL4zgpEj9CfuRrGbzF)

By parsing the QmRBkKi1P…p6z CID, you discover:

- the CID follows **version 0** spec because it starts with **Qm**
- the QmRBkKi1P…p6z string is **encoded** using **base58btc**
- the data "hello IPFS world by Web3Coach. BTW: Ethereum FTW" were encoded as DAG Protobuf under a codec 0x70 before being stored on disk
- the hash code 0x12 signals the data fingerprint obtained using the sha256 hash function, producing a unique 32 byte long digest

<base>``<codec><hash-function><hash-length><hash-digest>

**Multihash**

To be future-proof and enable different hashing algorithms, IPFS created the following standard:

`CODE : SIZE : DIGEST`

```go
type DecodedMultihash struct {
   Code   uint64 // 0x12
   Name   string // sha2-256
   Length int    // 32 bytes
   Digest []byte // Digest holds the raw multihash bytes
}
```

Multihash has many advantages. When computers are more powerful in 5 years, you could use a stronger hash function like sha3-512 as long as you configure the corresponding 0x13 code as the Multihash in the CID prefix – the protocol will be ready to handle it.

**Multicodec**

The Code attribute tells you how data is encoded before being stored on disk, so you know how to decode it back when the user wants to read it. It could be anything CBOR, Protobuf, JSON…

IPFS maintains a public list of all possible codecs. The most common codecs are:

```
raw       | ipld      | 0x55 | raw binary
dag-pb    | ipld      | 0x70 | MerkleDAG protobuf
dag-cbor  | ipld      | 0x71 | MerkleDAG cbor
````

// but you could also encode Ethereum blocks on IPFS!
`eth-block | ipld      | 0x90 | Ethereum Block (RLP)`

**Multibase**

The problem with CID v0 and the **base58btc** encoding is the lack of interoperability between environments. A multibase prefix adds support for different encodings like **base32** to achieve DNS-friendly names.

A table with all Multibase encodings:

- encoding  | code
- base32    | b
- base58btc | z
- base64    | m

[Multicodecs](https://github.com/multiformats/multicodec/blob/master/table.csv)

**Multihashes** follow the TLV pattern (type-length-value). Essentially, the "original hash" is prefixed with the type of hashing algorithm applied and the length of the hash.

- type: identifier of the cryptographic algorithm used to generate the hash (e.g. the identifier of sha2-256 would be 18 - 0x12 in hexadecimal) - see the multicodec table for all the identifiers

- length: the actual length of the hash (using sha2-256 it would be 256 bits, which equates to 32 bytes)

- value: the actual hash value.

**CIDv0  used base58btc encoding**

`<multihash-algorithm><multihash-length><multihash-hash>

What if the encoding is different? v0 -> v1

CIDv1 includes a multicodec prefix that specifies which encoding method was used. (ex: dag-pb)

**the multicodec prefix in an IPFS CID will always be an IPLD codec.**

`<multicodec><multihash-algorithm><multihash-length><multihash-hash>`

**CIDv1**

`<cid-version><multicodec><multihash:<multihash-algorithm><multihash-length><multihash-hash>>`

Encoding allows human readable form of a CID (bytes -> text)

If starts with:

- Qm: base58btc (v0)
- b: base32 (v1)

[CID Inspector](https://cid.ipfs.tech/#QmcRD4wkPPi6dig81r5sLj9Zm1gDCL4zgpEj9CfuRrGbzF)

The CID specification, which originated in IPFS, now lives in [Multiformats](https://github.com/multiformats) and supports a broad range of projects including IPFS, IPLD, libp2p, and Filecoin.

### Merkle DAGs

In honor to Ralph Merkle.

**Directed**: linkages between nodes flow in one direction
**Acycilc**: there are no cycles/loops anywhere in the graph.

> Note that structures made of nodes that embed the CIDs of their children necessarily cannot contain cycles. The cryptographic functions used in the construction of CIDs and therefore our Merkle DAG make it impossible to describe a "self-referential" path through the graph. This is an important security guarantee: if we traverse a Merkle DAG, we can be certain that we won’t end up in an infinite loop.

Merkle DAGS are used in git, IPFS,  Ethereum, Filecoin ... to store and communicate date.`

[IPLD](https://proto.school/merkle-dags/08) defines common formats as formal schemas for structuring and communicating content-addressed data structures. Is a single namespace for all hash-inspired protocols.

IPLD (objects)

-   `Data` — blob of unstructured binary data of size < 256 kB.
-   `Links` — array of Link structures. These are links to other IPFS objects.

A Link structure has three data fields

-   `Name` — name of the Link
-   `Hash` — hash of the linked IPFS object
-   `Size` — cumulative size of linked IPFS object, including following _its_ links

`ipfs object get QmW2WQi7j6c7UgJTarActp7tDNikE4B2qXtFCfLPdsgaTQ | jq`

[Explore DAGS](https://explore.ipld.io/#/explore/QmWNj1pTSjbauDHpdyg5HQ26vYcNWnubg1JehmwAE9NnU9)

### Distributability

Merkle DAGs help us alleviate all of these problems. By distributing the dataset as a content-addressed DAG:

 - Anybody who wants can help distribute the file
- Nodes from all over the world can participate in serving the data
- Each part of the DAG has its own CID that can be distributed independently
- It’s easy to find alternative providers of the same data
- The nodes forming the DAG are small, and can be downloaded in parallel from many different providers
- Larger datasets encompassing the original can simply link the original dataset as a child of a larger DAG

[DAG IPFS](https://dag.ipfs.io/)

### Deduplication

## lip2p

Code implementations of existing P2P systems were really hard to find, and where they did exist, they were often hard to re-use or re-purpose due to:

- Poor or non-existent documentation
- Restrictive licensing or no license to be found
- Very old code with the last update more than a decade ago
- No point of contact (no maintainer available to reach)
- Closed source (private) code
- Deprecated products
- No specifications provided
- No friendly API exposed
- Implementations too tightly coupled with a specific use case
- Not upgradable with future protocols


## Desktop IPFS app

https://github.com/TheDiscordian/native-ipfs-building-blox

Install RUST
Install Tauri
`create-tauri-app`

https://tauri.app/v1/references/architecture/inter-process-communication/



## ML + IPFS

https://winder.ai/do-you-like-dags-implementing-a-graph-executor-for-bacalhau/
[Job Pipelining](https://hackmd.io/@usN-geg4Q_iFcXZ-UCZpoQ/rkW5FE3Mj)

How a bacalhau node processes data import from ipfs?



## Bibliography

- [A beginner guide](https://hackernoon.com/a-beginners-guide-to-ipfs-20673fedd3f)
- [ML + IPFS](https://blog.aquila.network/top-5-reasons-why-ipfs-is-a-powerful-tool-for-machine-learning):  Top 5 reasons why IPFS is a powerful tool for Machine Learning
- [Fleek: IPFS Framework](https://blog.ipfs.io/2022-04-24-fleek/)
- [git-lfs as ipfs](https://github.com/sameer/git-lfs-ipfs)
- [Aquila DB](https://docs.google.com/presentation/d/1ioGh0BMnsPJnYJyZ5W9kglsa72YnZXD_9XsbHuGZzl8/edit#slide=id.g635cff3fde_0_110)
- [protocol labs research courses](https://research.protocol.ai/tutorials/resnetlab-on-tour/)
- [Crash course](https://www.freecodecamp.org/news/technical-guide-to-ipfs-decentralized-storage-of-web3/)
- [Files in IPFS](https://medium.com/textileio/whats-really-happening-when-you-add-a-file-to-ipfs-ae3b8b5e4b0f)

### Media

- [Content addressed distributed data structures-SpeakeasyJS2020](https://www.youtube.com/watch?v=VtzpJU4Cns8)
https://solid.mit.edu/

### Companies

- [Iterative AI](https://iterative.ai/)



[Protocol Labs Notion](https://pl-strflt.notion.site/PL-EngRes-Public-b5086aea86ed4f81bc7d0721c6935e1e)


### Medium synopsis

https://medium.com/0xcode/using-ipfs-for-distributed-file-storage-systems-61226e07a6f

IPFS is more ideal for permanent data storage, like for digital music, works of art and accreditations (e.g. certificates, diplomas, awards, donations). These are data types that don’t need to be changed and putting it on a blockchain based storage system makes more sense. It provides the creator or artist a digital proof that cannot be altered by anyone and provable through a hash based system with key value pairs that is unique only to one item.

IPFS storage is also more public, so confidentiality of data is required. This could be a violation for certain types of data storage which exposes private data (e.g. GDPR Rules). There are also available scalable solutions for data storage on AWS and Azure cloud that meet privacy, security and compliance. In my own opinion I don’t feel the need to distribute personal information the way IPFS stores data. For content that is made publicly available, using IPFS can provide a proof of authenticity to the owner of that content. This can secure a creator’s work like art by proving their ownership which then allows them to collect royalties and prevent others from taking credit for something they did not create.

It seems IPFS delivers fast and secure fault tolerant file storage for content. However, it may not be suitable for financial and personal data that requires strict regulation of how the data is stored and protected. This is also not recommended for storing files that change frequently due to updates, like active log files that continuously record data. The storage of data on the blockchain serves a different purpose, and IPFS provides that solution. As IPFS evolves, it could use a privacy layer that can hide personal data that is also encrypted at rest, so there would be no violations of exposing anything confidential
