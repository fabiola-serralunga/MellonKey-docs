# 🛠️ MellonKey◉fs◉  — Portfolio Project

> MellonKey◉fs◉, inspired by the gate of Moria.

v1.0.0 *MellonKeySimple* - Documentation

Original document written in spanish by https://github.com/fabiola-serralunga. Translated into english from spanish by a large language model (LLM).

🌐 https://testpages-simple.pages.dev -- website of *MellonKeySimple*

🔗 https://testpages-simple.pages.dev/deliver -- delivers the digital asset via Soulbound Token (SBT) - displays the Certificate of Acquisition - shows contract status

🔗 https://base-sepolia.blockscout.com/address/0x9DF53bB4F3A56b7927560C2638074f55F5DDD20E?tab=tokens_nfts -- to view the SBTs minted in the project in its test environment. 

📜⛓️🔷 `TestnetSBTGaladrielNenya.sol` -- An abstract name was avoided despite being a base contract. This allows for immediate visual traceability that unambiguously links the version, the environment, and the concrete implementation of the contract for this specific case, which is a portfolio project.

**MellonKey◉fs◉** is a development ecosystem created as a technical portfolio. It brings together the Proofs of Concept (PoC) and the results of an iterative design process originated in the ·sarocha· Project. During the research phase, the latter evolved toward an architecture that more efficiently fit its final objectives and scope, allowing MellonKey◉fs◉ to remain as a valuable repository of solutions, resolved dilemmas, and use cases transferable to other projects.

In this version 1.0.0, the main focus is to develop an end-to-end flow for the delivery of a digital asset, guaranteeing immutable records of provenance, authorship, and a certificate of acquisition through Blockchain technology.

To evaluate the integration of the components in a controlled environment and analyze how they interact in real time, the initial collection *The Three Elven Rings* was structured with three digital assets responding to the theme, where this serves as a conceptual framework to validate the system.

In order to validate the base behavior of the system before scaling the infrastructure, the deployment was executed through an individual ERC-721 smart contract for each asset. At this early stage of development, the use of complex patterns such as Factory or Registry was deliberately omitted.


| | |
|---|---|
| **Project** | MellonKey◉fs◉  (v.1.0.0 *MellonKeySimple*). |
| **Gallery** | The Three Elven Rings |
| **What it is** | Soulbound Tokens (SBT) that certify the acquisition of a digital asset and act as an access key to its original file |
| **Network** | Base Sepolia Testnet (chainId 84532 / `0x14a34`) |
| **Token standard** | ERC-721, open edition, non-transferable (soulbound) |
| **Base contract** | `TestnetSBTGaladrielNenya` — Solidity ^0.8.20 + OpenZeppelin v5 |
| **Web infrastructure** | Cloudflare Pages (static site + Pages Function on the Workers runtime) + Cloudflare R2 |
| **Metadata** | JSON hosted on Arweave (IPFS-compatible) |
| **Status** | v1.0.0 on testnet — Active editions: *Galadriel and Nenya* and *Mithrandir and Narya*. Edition established by design as "coming soon" with *Elrond and Vilya* |

---

## Index

- [🛠️ MellonKey◉fs◉  — Portfolio Project](#️-mellonkeyfs---portfolio-project)
  - [1. What is MellonKey◉fs◉?](#1-what-is-mellonkeyfs)
  - [2. Design principles and rationale](#2-design-principles-and-rationale)
  - [3. General architecture: Smart Contracts and Flow Validation](#3-general-architecture-smart-contracts-and-flow-validation)
    - [3.1. Test Environment](#31-test-environment)
    - [3.2. Technical Challenges and Research (PoC)](#32-technical-challenges-and-research-poc)
  - [4. The base contract: `TestnetSBTGaladrielNenya`](#4-the-base-contract-testnetsbtgaladrielnenya)
    - [4.1 What it is and how it is used](#41-what-it-is-and-how-it-is-used)
    - [4.2 ERC-721 - When the certificate warrants a single owner](#42-erc-721---when-the-certificate-warrants-a-single-owner)
    - [4.3 Contract features](#43-contract-features)
    - [4.4 The data fixed at deployment (constructor)](#44-the-data-fixed-at-deployment-constructor)
    - [4.5 The record left by each mint](#45-the-record-left-by-each-mint)
    - [4.6 The minimal interface the site consumes](#46-the-minimal-interface-the-site-consumes)
  - [5. Edition metadata (JSON on Arweave)](#5-edition-metadata-json-on-arweave)
    - [5.1 What it contains](#51-what-it-contains)
    - [5.2 Arweave (and IPFS too)](#52-arweave-and-ipfs-too)
    - [5.3 A single JSON for the entire open edition](#53-a-single-json-for-the-entire-open-edition)
  - [6. `editions.json`](#6-editionsjson)
    - [6.1 The problem it solves](#61-the-problem-it-solves)
    - [6.2 Who reads what](#62-who-reads-what)
    - [6.3 The complete cycle to publish a new edition](#63-the-complete-cycle-to-publish-a-new-edition)
  - [7. The minting page: `index.html`](#7-the-minting-page-indexhtml)
    - [7.1 Minting happens from the web page, without a server](#71-minting-happens-from-the-web-page-without-a-server)
    - [7.2 How it works internally](#72-how-it-works-internally)
    - [7.3 The claim flow, step by step](#73-the-claim-flow-step-by-step)
  - [8. SBT usage: `deliver.html` and file delivery](#8-sbt-usage-deliverhtml-and-file-delivery)
    - [8.1 Minting does not require a server; delivery does](#81-minting-does-not-require-a-server-delivery-does)
    - [8.2 Delivery authorization](#82-delivery-authorization)
    - [8.3 Current file status: R2 unencrypted (and the next stage)](#83-current-file-status-r2-unencrypted-and-the-next-stage)
    - [8.4 "Certificate of Acquisition" panel](#84-certificate-of-acquisition-panel)
    - [8.5 Public panel on `deliver.html`](#85-public-panel-on-deliverhtml)
  - [9. Test network and faucet funding](#9-test-network-and-faucet-funding)
  - [10. Full visualization](#10-full-visualization)
    - [10.1 What was observed](#101-what-was-observed)
    - [10.2 Blockscout Explorer - Base Sepolia Chain](#102-blockscout-explorer---base-sepolia-chain)
    - [10.3 Marketplaces](#103-marketplaces)
  - [11. Decision table: what was chosen and why](#11-decision-table-what-was-chosen-and-why)
  - [12. Current status and roadmap](#12-current-status-and-roadmap)
    - [**Current status (v1.0.0, testnet):**](#current-status-v100-testnet)
    - [**Roadmap:**](#roadmap)
  - [13. Repository structure](#13-repository-structure)
  - [14. Minimal glossary](#14-minimal-glossary)

---

## 1. What is MellonKey◉fs◉?

MellonKey◉fs◉  is a system that registers on blockchain the delivery of a **certificate of acquisition** for a digital asset, granting access to it through a token. Each digital asset is published as an open edition, and each acquired copy receives a certificate with its data — artist, artwork, description, token id, edition, date — and a single owner. Whoever acquires a MellonKey SBT receives an on-chain, non-transferable certificate proving that this person acquired a digital asset from a specific creator, and that also functions physically as a key to download the original file. 

Let us keep in mind the idea stated plainly: in this case, the MellonKey SBT *is not the digital asset*, but its certificate of acquisition and the way to access it. In another ongoing development, it is being coded so that the token is the digital asset itself, with its corresponding certificate of acquisition and unique identification for the holder. 

Each digital asset has its own **Soulbound Token** (SBT): an ERC-721 token that is minted through a public `claim()`, is bound forever to the wallet that claimed it, and cannot be sold or transferred. It is, strictly speaking, a personal property credential. The project name sums it up: *mellon* means "friend" in Quenya, the word that opened the gate of Moria in *The Lord of the Rings*. 

This testnet has completed the proof of concept with the *The Three Elven Rings* series, consisting of *Galadriel and Nenya*, *Mithrandir and Narya*, and *Elrond and Vilya*, where **each edition is an independent ERC-721 contract deployed from the same base contract**, and where the original file of each artwork lives in a private Cloudflare R2 bucket, delivered only after verifying on-chain that the requesting wallet holds the token for that edition.

A soulbound token was chosen because the goal is to certify acquisition and grant access, not to create a secondary market. If the token can be sold, the key becomes separated from the original buyer and the certificate ceases to be a credential of the person who acquired the digital asset. The non-transferable nature keeps the creator → collector relationship intact and simplifies the entire authorization layer: the wallet that owns the token is, by definition, the wallet that minted it.

## 2. Design principles and rationale

**2.1 — One base contract, one deployment per edition.** MellonKey takes **screen printing** as its mental model: each digital asset is published as an edition, an open run of copies, and each acquired copy receives its certificate of acquisition, with its data and a single owner. Technically, this logic is materialized with **a base contract in Solidity** (`TestnetSBTGaladrielNenya`) that is **deployed once per edition**, and each deployment produces a distinct contract address with its own name, symbol, metadata, and price — the public matrix of that edition. The tokens it issues are the numbered certificates of the acquired copies. The artwork is thus completely isolated: pausing one edition does not affect the others, the price is independent, the explorer history is clean, and the contract address becomes the public "identity" of the edition. The cost of this design is trivial (deploying the same code N times) and the benefit is enormous in simplicity: the contract does not need to know that other editions exist, and the certificate does not need to know that other artworks exist.

**2.2 — The blockchain is the database.** The record of "who acquired what and when" lives in the contract: the ERC-721 `ownerOf` mapping records the wallet that owns each tokenId, the `Claimed` event emits the mint with its timestamp, and the `mintedAt(tokenId)` function exposes the minting date. The authorization to download the digital asset is resolved by querying the contract. 

**2.3 — Configuration (network, contracts, R2, prices, status) lives in `editions.json`.** The frontend and the delivery function read it from there. The addresses of each smart contract are hardcoded.

**2.4 — Serverless mint, delivery with a minimal server.** Minting is a direct transaction between the user's wallet and the contract: no project-owned piece participates in the transaction, so the mint **requires no server**. File delivery, on the other hand, **does require a server**: 

**2.5 — The entire cycle was validated on Base Sepolia with ETH from public faucets.** The same flow applies on mainnet.

**2.6 - The metadata is published on Arweave,** permanent, decentralized storage with perpetual payment. The smart contract stores a URI. The same metadata.JSON hosted on Arweave would work identically pointing to IPFS.

## 3. General architecture: Smart Contracts and Flow Validation

In order to validate the base behavior of the system before scaling the infrastructure, the deployment was executed through an individual ERC-721 smart contract for each asset.

This simplified design decision made it possible to isolate variables, accelerate the Web3 learning curve, and consolidate the foundations of the business logic before moving toward more advanced architectural automation.

### 3.1. Test Environment

* Network / Funding: Base Sepolia Testnet (Sepolia ETH Faucets).
* Standard: ERC-721 (configured as Soulbound Token / Non-transferable).
* Initial Contract: `TestnetSBTGaladrielNenya`.

### 3.2. Technical Challenges and Research (PoC)

To validate the architecture before implementing complex automations (such as Factory or Registry patterns), an iterative design approach was chosen. This made it possible to isolate variables, accelerate the Web3 learning curve, and audit the contract behavior directly in production.

During the deployment of the base contract and minting from the test interface, the following scenario was identified and documented:

* Behavior in Explorers (Blockscout): Both the digital asset metadata and the global functions of the contract are indexed and displayed correctly and successfully.
* Client-Side Incidence (Web3 Wallet Extensions): A visual bug recognized by the industry was experienced, where certain browser extensions do not render the public methods `name()` and `symbol()`.
* Analysis of the Incidence: This behavior does not affect the integrity or business logic of the smart contract. It is associated with the way the wallet provider (client RPC/cache) parses individual contracts on test networks, which confirms the importance of performing these integration tests in staging environments to map the user experience (UX).
* Nevertheless, after reviewing the `TestnetSBTGaladrielNenya` code, I made a minor change and proceeded to deploy it by making only one modification — using quotes for strings — when filling in the `name`, `symbol`, `tokenURI`, and `mintPrice` fields in Remix IDE, I minted the SBT again and the result was identical. 
* To isolate other variables, the next digital asset was minted from `TestnetSBTMithrandirNarya`. 
* At this point I had enough valuable information that is capitalized in **MellonKey◉fs◉ v2.0.0**.

To view the experimental tokens minted for this project (and others), you can visit 
https://base-sepolia.blockscout.com/address/0x9DF53bB4F3A56b7927560C2638074f55F5DDD20E?tab=tokens_nfts

```
              ┌──────────────────────────────────────────────────┐
              │             Base Sepolia                         │
              │                                                  │
              │   TestnetSBTMellonKey @ 0xc76B…e1B   (Galadriel) │
              │   TestnetSBTMellonKey @ 0x…          (Mithrandir)│
              │   TestnetSBTMellonKey @ 0x…          (Elrond)    │  
              │   • claim() payable      • ownerOf()             │
              │   • mintPrice()          • mintedAt()            │
              │   • balanceOf()          • tokenURI()            │
              └────────────▲───────────────▲─────────────────────┘
                                     │               │
              (1) claim + ETH        │               │  (3) ownerOf(tid)
              wallet ↔ contract      │               │      verification
                                     │               │
┌────────────────────┐   (2) reads    │               │
│   editions.json    │◀──────────────┴───────────────┘
│                    │      ┌──────────────────────────────┐
│  network · contracts│─────▶│  Cloudflare Pages            │
│  r2File · prices   │      │  index.html   (mint / claim) │
└────────────────────┘      │  deliver.html (SBT usage)   │
                            │  functions/api/download.js   │
                            │  (Pages Function / Workers)  │
                            └───────┬───────────────┬──────┘
                                    │               │
                       (4) reads r2File│              │ (5) tokenURI()
                                    ▼               ▼
                            ┌──────────────┐   ┌──────────────────┐
                            │ Cloudflare   │   │ Arweave          │
                            │ R2 (private) │   │ metadata JSON    │
                            │ *digitalasset│   │                  │
                            └──────────────┘   └──────────────────┘
```

**How the parts integrate, in real order of use:**

1. **The content creator deploys** an instance of `TestnetSBTGaladrielNenya` per edition (in this version Remix IDE was used), passing to the constructor: collection name, symbol, URI of the metadata JSON on Arweave, and price in wei. The resulting smart contract address is pasted into `editions.json`.
2. **The visitor mints from the web**: `index.html` reads the configuration from `editions.json`, queries `mintPrice()` on-chain, and sends `claim()` with the attached ETH from MetaMask. No server is involved.
3. **The collector uses the SBT**: on `deliver.html` they select the edition, enter their tokenId, sign a message with their wallet, and the page sends everything to the delivery function.
4. **The function verifies and delivers**: `functions/api/download.js` validates the signature, queries `ownerOf(tokenId)` against the contract for that edition, and if the owning wallet matches, serves the `r2File` for that edition from R2. The delivery function uses the object key in R2 to resolve the download. It is not a full URL.
5. **The certificate of acquisition is public: it is recorded on-chain and, depending on the marketplace, may also be visible in its interface.**: the contract's `tokenURI()` points to the JSON on Arweave that explorers (Basescan, Blockscout) and marketplaces (OpenSea and others) read to display its name, image, description, and attributes.

Each piece consumes only what it needs. The contract is autonomous once deployed: it does not read project files. The data source in `editions.json` is not read by the blockchain. The JSON metadata on Arweave is not read by the site: it is resolved by explorers and marketplaces through the contract. That decoupling is what allows adding editions without touching code. 

---

## 4. The base contract: `TestnetSBTGaladrielNenya`

### 4.1 What it is and how it is used

`TestnetSBTGaladrielNenya` is **a single base contract** written in Solidity (^0.8.20) on top of OpenZeppelin v5. When **an instance of this contract is deployed per digital asset edition**, each instance lives at its own contract address on the blockchain with its own data. The model is that of screen printing: the base contract is the **matrix** (unique, reusable, audited only once), each deployment is the **edition** stamped with that matrix (with its name, symbol, metadata, and price), and each token it issues is a **signed and numbered copy** of that edition — a certificate of acquisition with its data and a single owner:

name_: `Galadriel and Nenya (Testnet SBT)` <br>
symbol: `GNENYA` <br>
MetadataURI_: JSON from Arweave for this artwork <br>
mintPrice. `0.02 ETH` in wei <br>

A contract-per-edition approach was chosen because (a) the deployment is the event that "publishes" the artwork, with its name and symbol immutable in the explorer; (b) the price and metadata are its own and require no conditional logic; (c) a problem in one edition (for example, pausing it indefinitely) does not affect the others; (d) the `Claimed` event history of each contract is exactly the acquisition history of that artwork — nothing needs to be filtered. And since the mold is unique, auditing once is enough for all editions.

In this version 1.0.0, no way of grouping these independent contracts at the blockchain and indexing level has yet been implemented. Beyond the website, there is still nothing that allows a user observing the editions in explorers and marketplaces to understand that they maintain a collective relationship with each other. 

In version 2.0.0, as a preliminary solution before implementing a Factory or Registry, the grouping of the three editions within the same collection will be resolved at the on-chain metadata level through the `name()` function encoded in each contract. Again, the goal is to evaluate the reach of this solution in explorers and marketplaces, given that there is consensus that indexing happens anyway. In practice, the following will be done: although each contract will keep its own address and independence on the network, a common prefix will be used in the `name` field to observe whether this identification is also indexed semantically in explorers and applications. To make it clear: `HolderNarya Gil-galad`, `HolderNarya Cirdan`, `HolderNarya Mithrandir`.

The question is whether this convention in `name` guarantees immediate visual traceability and brand consistency in wallets and explorers — or not. Nevertheless, the definitive integration of the Registry/Factory is the technical solution to formally group collections under a single on-chain registry, automate their deployment, and validate the legitimacy of each contract; also toward unified indexing, contract discovery, and centralized collection governance; and the path toward effective interoperability with dApps and marketplaces.

### 4.2 ERC-721 - When the certificate warrants a single owner

The screen printing metaphor also defines the standard. A certificate of acquisition must be able to answer, unambiguously and without consulting anyone else: **which artwork is it?** (the edition's metadata), **which copy is it?** (a number per acquisition), **whose is it?** (a single owner), and **when was it acquired?** (a date per copy). ERC-721 answers all four: each token is a unique unit (`tokenId`), `ownerOf(tokenId)` returns *one* owning wallet, and the contract adds `mintedAt(tokenId)` as the minting date.

When evaluating how to resolve digital file delivery, the ERC-1155 standard was also considered. By contrast, this is a **semi-fungible standard that thinks in balances**: the same wallet can hold several units of the same `id` (`balanceOf(wallet, id) = 5`), there is no `ownerOf` per unit, the units are interchangeable with each other, and there is no issuance or date per copy. Whoever holds "3 units of id 1" under ERC-1155 is not the owner of three numbered certificates: they are a holder of balance 3. For fungible items (in-game ammo, equivalent tickets) it is perfect; for a certificate of acquisition it is the wrong unit — limiting it to 1 per wallet would be a patch over a standard designed for the opposite.

This is how it closes in practice: when someone claims their copy, the contract mints the next number, stores the date, and registers the owner; when someone requests the original file, the web server requests their **signature** and asks the contract **who is the owner of that number**. The certificate of acquisition is guaranteed by the standard.

### 4.3 Contract features

ERC-721 (OpenZeppelin v5)

Universal standard: the token is visible in wallets, explorers, and marketplaces with no extra work.

Soulbound (non-transferable)

- Override of `_update` that only allows mint (`from = 0x0`) and burn (`to = 0x0`).
- No one can sell, gift, or move the token: SBT, transfer not allowed.
- It is a personal credential and a key that cannot be lent.

Open edition with `claim()`

- `claim() external payable`, 1 token per wallet (`balanceOf(msg.sender) == 0`).
- Anyone who pays the price claims their certificate; there is no fixed quota or allowlist.
- One wallet = one certificate = one key.

Pausable

- OpenZeppelin `Pausable`, `whenNotPaused` on `claim()`, `pause()`/`unpause()` owner-only.
- The creator can stop new minting at any time (for edition closure, review, or emergency).
- Already-minted tokens retain their access: pausing does not break existing keys.

ReentrancyGuard

- OpenZeppelin `ReentrancyGuard`, `nonReentrant` on `claim()` and `withdraw()`.
- Explicit protection against reentrancy in the two functions that move ETH.
- Complemented with the checks-effects-interactions pattern (the refund goes at the end).

Automatic refund

- If `msg.value > mintPrice`, the excess is returned.
- Sending extra ETH never over-pays: the contract returns the difference in the same transaction.

On-chain minting date

- Mapping `_mintedAt[tokenId]` with `block.timestamp`.
- Public getter `mintedAt(uint256)`.
- Event `Claimed(to, tokenId, mintedAt)`.
- The certificate registers when it was acquired, queryable by anyone forever and emitted in logs that explorers index.

Metadata by URI

- `tokenURI()` returns the fixed URI stored in the constructor (Arweave; would be identical with IPFS).
- All tokens in the edition share the same metadata JSON (see §5), consistent with an open edition.

Owner administration

- `pause()`/`unpause()`, `setMintPrice()`, `setMetadataURI()`, `withdraw(to)`.
- Reasonable and bounded control: adjust price, correct/update metadata, pause, and clear manual funds.
- In a new contract, automatic withdrawal will be set: when `claim()` is executed, the payment will be sent directly to the owner's wallet. The `withdraw(to)` function will remain as a safety backup in case any ETH gets stuck (for example, if someone sends funds to the contract without using mint).
- None of what has already been minted is affected by these functions.

Ownable(msg.sender)

- The deployer remains as the owner.
- In this version 1.0.0 the owner is the developer's wallet.


### 4.4 The data fixed at deployment (constructor)

The constructor receives **four parameters**, which are typed by hand at deployment and are fixed in the contract storage:

```solidity
constructor(
        string memory name_,         
        string memory symbol_,       
        string memory metadataURI_, 
        uint256 mintPrice_,          // price in wei, e.g.: 20000000000000000 (= 0.02 ETH)            
    ) ERC721(name_, symbol_) Ownable(msg.sender) {
        _metadataURI = metadataURI_;
        mintPrice = mintPrice_;
    }
```

Important points about these parameters within MellonKey◉fs◉:

- **`name` and `symbol` belong to the contract (on-chain)**: They are defined through the `name()` and `symbol()` functions of the ERC-721 standard and cannot be overwritten by external metadata. Their function is to establish the identity of each edition on the blockchain. There is also a **`name` field in the JSON uploaded to Arweave that represents the individual asset (off-chain)**; and which names the token and coexists in parallel with the contract's identity without replacing it (more on this in §10).
- **`metadataURI` is a URI, not the content.** The contract only stores the pointer. In this case the metadata is on Arweave because permanence, decentralization, and perpetual payment are chosen; pointing to an IPFS CID or equivalent gateway would work exactly the same — the contract serves it via `tokenURI()`.
- **`mintPrice` is in wei.** 0.02 ETH = `20000000000000000` wei (18 decimals). The mint page does not hardcode this value: it reads it on-chain with `mintPrice()` at the moment of minting, so a price change via `setMintPrice()` is reflected on the web without touching anything else.
- **The deployer is the owner.** `Ownable(msg.sender)` makes the deploying wallet the owner of the contract, the only one authorized to pause, change the price, change the metadata URI, and withdraw funds.

In version 2.0.0, the new contract will record `author` (text) expressing a semantic identity/attribution. It will also record the **verifiable cryptographic identities of `creator`, `crafter`, and `artist`** (wallet addresses) at the time of deployment. Thanks to **`setAttribution()`**, the owner will have the flexibility to correct or update these addresses in the future without needing to do a new deploy.


### 4.5 The record left by each mint

By calling `claim()` with the corresponding ETH, the contract executes and records:

```text
require(msg.value >= mintPrice)      → if not: "SBT: insufficient payment"
require(balanceOf(msg.sender) == 0)  → if not: "SBT: already minted" (1 per wallet)
tokenId = _nextTokenId++             → sequential numbering starting from 1
_safeMint(msg.sender, tokenId)       → ownerOf mapping: tokenId → wallet  ★ the key record
_mintedAt[tokenId] = block.timestamp → on-chain minting date
emit Claimed(to, tokenId, mintedAt)  → event log (indexable by explorers)
emit Transfer(0x0 → wallet)          → standard ERC-721 mint event
refund of the excess (if any)        → interaction at the end (CEI pattern)
```

The ★ marks the piece that holds up the entire delivery system: `ownerOf(tokenId)` is the only "user table" MellonKey has, and it is immutable, public, and verifiable by anyone.

### 4.6 The minimal interface the site consumes

The site uses a minimal human-readable ABI, declared inline in the pages and in the delivery function — no separate ABI file is needed:

```text
claim()                payable                        → mint page
mintPrice()            view returns (uint256)         → mint page
balanceOf(address)     view returns (uint256)         → mint page (card status)
ownerOf(uint256)       view returns (address)         → delivery function (authorization)
mintedAt(uint256)      view returns (uint64)          → certificate panel on deliver.html
```

---

## 5. Edition metadata (JSON on Arweave)

### 5.1 What it contains

Each edition has **a metadata JSON** following the ERC-721 Metadata standard, hosted permanently and with perpetual payment on Arweave. It is the JSON that explorers and marketplaces read via `tokenURI()` to display the certificate. The one for the first edition (summarized) is:

```json
{
    "attributes": [
        {
            "trait_type": "Artist",
            "value": "MellonKey◉fs◉"
        },
        {
            "trait_type": "Collection",
            "value": "Testnet The Three Elven Rings"
        },
        {
            "trait_type": "Artwork",
            "value": "Testnet Mithrandir and Narya"
        },
        {
            "trait_type": "Edition",
            "value": "Open Edition (Testnet)"
        }
    ],
    "description": "Mithrandir, known in Middle-earth as Gandalf, was one of the Istari sent by the Valar to aid the free peoples against Sauron. Among his most treasured secrets was Narya, the Ring of Fire, one of the Three Elven Rings forged by Celebrimbor. Círdan, the Shipwright, guarded it for centuries and, seeing Gandalf arrive, knew he must give it to him. Narya granted no power of domination, but the ability to kindle hearts, inspire courage, and reawaken hope in times of darkness. With it, Mithrandir inspired Men, Elves, and Hobbits to resist the Shadow, to fight when all seemed lost. The ring, set with a ruby, symbolized the inner fire that does not yield. Gandalf wore it hidden, without boast, and its influence was felt in the Shire, in Rohan, in Gondor, and in the last battle before the Black Gate. After the destruction of the One Ring, the power of the Three faded, and Mithrandir departed into the West bearing Narya, the ring that kindled the flame of resistance.",
    "image": "https://arweave.net/adHMMYCWVpyA8vxyN_cZW5soQzSSbYpvF4Xsa-Rw5fE",
    "name": "Test Mithrandir and Narya by MellonKey◉fs◉ "
}
```

In the implementation of version 2.0.0, it will be taken into account that since certain platforms do not display `name()` or `symbol()`, `description` will be leveraged to display that information. 

```json
{
  "description": "CERTIFICATE OF ACQUISITION — Soulbound Token (Testnet). Artist: MellonKey◉fs◉. Collection: Testnet The Three Elven Rings. Artwork: Testnet Galadriel and Nenya. This non-transferable ERC-721 SBT certifies the acquisition of the artwork and unlocks the download of its exclusive file on the MellonKey project page. […] Nenya, the Ring of Water: Galadriel's Ring of Preservation and the Three Elven Rings. […full lore of Nenya…]"
}
```

**The `attributes` of the JSON are the critical part of the design.** MellonKey's SBTs are certificates of acquisition and, accordingly, the creator's name (`Creator`), the collection, the artwork, and the edition type are encoded as structured attributes (`trait_type` / `value`). This is the format that Basescan, Blockscout, OpenSea, and the rest of the explorers and marketplaces index and display as "attributes" or "properties" of the token. Wallets do not usually display them. 

### 5.2 Arweave (and IPFS too)

- **Arweave** offers permanent storage with a single payment: the JSON URI does not expire, does not depend on the creator maintaining hosting, and the certificate is replicated on the permaweb. For a certificate of acquisition that is expected to last decades, this permanence is a good solution. 
- **IPFS would be valid**, although the performance is different. The contract stores a text string and serves it via `tokenURI()`; it does not know whether it points to Arweave, an IPFS gateway, or anywhere else. Publishing on IPFS and passing `https://ipfs.io/ipfs/<CID>` to the constructor produces exactly the same behavior. The choice of Arweave was a policy of permanence, not of compatibility.
- **The image also lives on Arweave** (`image` in the JSON), so the complete certificate (image + data) is independent of the gallery's infrastructure.
- Finally, it remains to be considered that in this version **Cloudflare's centralized web2 server** has been chosen since everything runs from the same website and the project is a portfolio. 

### 5.3 A single JSON for the entire open edition

In an open edition all tokens are equal to each other (same artwork, same certificate, different number), so **`tokenURI()` returns the same URI for any tokenId**: 

- **Economy:** there is no need to publish (and pay for) a JSON per token or store URIs per tokenId on-chain.
- **Coherence:** Certificate N° 3 and N° 300 declare exactly the same digital asset and the same attributes; but each certificate is unique with respect to the holder, which is why the tokenId is updated as on-chain data.
- **Contrast with other flows:** in projects with per-token metadata (where each token is different), a service that generates the JSON on demand is needed. In MellonKey, being an open edition with an identical certificate: a single JSON on Arweave is enough.

> **Note:** if in the future an edition needed distinct metadata per token, the contract already exposes `setMetadataURI()` (owner) to repoint the edition's URI; and for the per-token case, the variant of storing a base URI + suffix per tokenId would exist. It was not implemented because the open edition does not need it.
---

## 6. `editions.json` 

### 6.1 The problem it solves

MellonKey centralizes **everything** that changes between editions in a single JSON read by the three pieces of the site:

```json
{
  "chain": {
    "name": "Base Sepolia Testnet",
    "chainIdHex": "0x14a34",
    "chainIdDec": 84532,
    "rpcUrls": ["https://sepolia.base.org"],
    "blockExplorerUrls": ["https://sepolia.basescan.org"],
    "alchemyUrlTemplate": "https://base-sepolia.g.alchemy.com/v2/{ALCHEMY_API_KEY}"
  },
  "editions": [
    {
      "id": "galadriel-nenya",
      "title": "Galadriel and Nenya",
      "author": "Portfolio Project MellonKey",
      "creator": "MellonKey◉fs◉",
      "image": "https://arweave.net/RW6F24scwbOfgL_6cwnBqqPB93tOMtWoxOpPbSOZJxo",
      "priceLabel": "0.02 ETH",
      "contract": "0x...",
      "active": true,
      "r2File": "galadriel-nenya.png",
      "downloadName": "galadriel-nenya.png"
    },
    {
      "id": "mithrandir-narya",
      "title": "Mithrandir and Narya",
      "author": "Portfolio Project MellonKey",
      "creator": "MellonKey◉fs◉",
      ...
      ...
    }
  ]
}
```

### 6.2 Who reads what

| Consumer | What it uses from `editions.json` |
|---|---|
| `index.html` (mint) | list of editions to render cards, `contract` to interact, `chain` to configure the network in the wallet, `priceLabel`/`image`/`title` for the UI |
| `deliver.html` (download) | active editions for the selector, `artist` for the certificate panel, `contract` to read `mintedAt` on-chain |
| `functions/api/download.js` (delivery) | resolves the edition by `id`, validates `active` and `contract`, looks up `r2File` in R2, builds the RPC URI with `alchemyUrlTemplate` |

No contract address is hardcoded in the code. The JSON is read by the backend **from the static assets of the Cloudflare deploy itself** (`env.ASSETS.fetch`), so the deploy and the configuration travel together and are always consistent.

### 6.3 The complete cycle to publish a new edition

```text
1. Deploy an instance of `TestnetSBTGaladrielNenya` with `name`, `symbol`, `metadataURI` (Arweave JSON of the digital asset), `mintPrice` in wei.
2. Add the contract address in the new entry of `editions.json`.
3. Upload the artwork's original file to R2 with the exact name of "r2File".
4. Set "active": true.
5. Redeploy the website on Cloudflare. 
```

---

## 7. The minting page: `index.html`

### 7.1 Minting happens from the web page, without a server

The minting of each Soulbound Token on the *The Three Elven Rings* gallery site designed by MellonKey is a `claim()` — a claim, not a purchase mediated by anyone. The visitor opens the gallery, connects their wallet, and claims their certificate of acquisition in a transaction that goes **directly from their wallet to the contract**. No server of our own touches the transaction: there is no cart, no checkout API, no "payment processor". The ETH attached to the transaction is the price, and the price is not decided by the page: the page **reads `mintPrice()` from the on-chain contract** and sends exactly that value as the transaction's `value`. If the creator/owner updated the mint price in the contract, the website reflects it automatically on the next load.

### 7.2 How it works internally

- **Dynamic cards from `editions.json`.** Editions are not written in the HTML: they are rendered by reading the JSON. An edition without a deployed contract or with `active: false` is shown as *Coming soon*.
- **Correct network without friction.** Before operating, `ensureNetwork()` verifies the wallet's `chainId` against the one in `editions.json`; if it differs, it requests the switch with `wallet_switchEthereumChain` (and offers `wallet_addEthereumChain` if the network is not added), without reloading the page.
- **Shared minimal ABI.** The cards need `claim()`, `mintPrice()`, and `balanceOf(address)` — declared as inline human-readable ABI. 
- **Real status per wallet.** After connecting, `balanceOf` is queried on each contract to render the true state of each card (*Mint* / *Owned*).
- **A single delegated listener.** Click events are handled with delegation on the card container, so adding editions to the JSON does not add JS code.

### 7.3 The claim flow, step by step

```text
[User]                      [index.html]                   [SBT Contract]
    │ connects wallet                  │                              │
    │───────────────────────────────▶│                              │
    │                     ensureNetwork() + balanceOf()              │
    │ click "Mint"                    │                              │
    │ ─────────────────────────────▶ │ mintPrice()  (read)           │
    │                                 │─────────────────────────────▶│
    │                                 │ claim({ value: price })      │
    │ signs/sends tx in wallet        │─────────────────────────────▶│
    │                                 │        Claimed(to, id, ts)   │
    │◀───────────────────────────────│ tx confirmed                 │
    │ SBT in wallet + "Owned" status                                │
    │ click "Reveal Here"  ──▶  deliver.html?edition=<id>            │
```

That last step is the bridge between the two halves of the product: the *certificate* half (mint, visible in wallet and explorers) and the *key* half (download of the original file). 

---

## 8. SBT usage: `deliver.html` and file delivery

### 8.1 Minting does not require a server; delivery does

Minting (*mint*) is completed without a server since the transaction is the record: there is nothing to process beyond what the blockchain already processes. File delivery, on the other hand, needs a server because the original file requires authorization for its download. A **Cloudflare Pages Function** was implemented (`functions/api/download.js`) — which runs on the same Cloudflare Workers runtime and delivers the file from R2.

### 8.2 Delivery authorization

In all her current projects, the creator and developer maintains the same initiative: the immutable, public, and easily checkable record that blockchain offers for the provenance, authorship, and acquisition of each digital asset. The advantage or bonus of adopting a token registered on the blockchain is that it also solves the secure delivery of the original file as many times as the buyer requires, without depending on a database of buyers. The proof of ownership is built on the client and verified against the chain:

```text
[deliver.html — client]                          [functions/api/download.js — server]
1. User selects acquired edition 
2. `.html` builds the message:
   "Download 'digital asset' with tokenId 3
    at Gallery - 1758…(timestamp in ms)"
3. User confirms personal_sign of the message (in the wallet)
4. POST /api/download
   { wallet, signature, message, tokenId, edition }
                                    ─────────────▶ 5. Validates that the message is recent
                                                       (TTL of n minutes — anti-replay)
                                                   6. verifyMessage(message, signature)
                                                       → recovers the signing wallet
                                                   7. ownerOf(tokenId) on the CONTRACT
                                                       of that edition (via RPC)
                                                   8. ownerOf == signing wallet?
                                                       NO → 403
                                                       YES → 9
                                                   9. bucket.get(edition.r2File)
                                                       → serves the file with
                                                         Content-Disposition: downloadName
[client]
10. Receives the blob and triggers the download
```

- **The signature binds everything in a single string:** edition + tokenId + wallet + moment. An old signature cannot be reused (TTL of `n` minutes), the signature of one edition cannot be used for another, and you cannot sign with a wallet other than the owning one.
- **`ownerOf` is the source.** Even if someone forged the frontend, the server verifies the signature cryptographically and then queries the contract for the owner of that tokenId **in the contract of that edition**. The "buyer registry" is the ERC-721 mapping.
- **The file is resolved by edition, not by token.** `r2File` is declared once per edition in `editions.json`: all tokens in the open edition download the same original file, with the download name (`downloadName`) also defined per edition.
- **The Alchemy secret stays on the server.** The frontend uses the public Base Sepolia RPC; the delivery function builds its RPC URI with `ALCHEMY_API_KEY` from environment secrets (`alchemyUrlTemplate` from the JSON). 

### 8.3 Current file status: R2 unencrypted (and the next stage)

At this stage, MellonKey v1.0.0 (*MellonKeySimple*), the artwork's original file is in R2 **unencrypted**: the asset's protection is the access control of the delivery function — R2 is not public, there is no direct URL to the object, and the only gateway is `POST /api/download` with on-chain verification. The file is always transported over HTTPS and its location is never exposed.

**The next stage, already defined as roadmap, is encryption of the file at rest:** the object will be stored encrypted (for example, AES-256 per edition) so that the content is useless without the key, and delivery will consist of decrypting on the fly for verified wallets — or, later on, schemes where SBT ownership participates in key derivation. The change is orthogonal to the rest of the system: `editions.json` already declares the file per edition, and the function is the only point through which the bytes pass, so encrypting there does not touch the contract, the frontend, or existing flows.

### 8.4 "Certificate of Acquisition" panel

Since SBTs are certificates of acquisition, `deliver.html` includes a panel that **repeats and completes the certificate data on the surface where the project has full control**: creator, title, collection, and contract (from `editions.json`), tokenId, holder, and minting date read on-chain with a read-only provider that requires connecting the wallet. This requirement makes the certificate of acquisition behave as a personal document on the web. This panel exists precisely because wallets do not guarantee the display of that data (see §10): the certificate cannot depend on a third-party renderer to show what is essential.
Although all the data is traceable on the blockchain — and in fact the panel also links to the token's page on Basescan — on the web it is visualized and recognized as a certificate of acquisition. 


### 8.5 Public panel on `deliver.html`

A **public panel** on `deliver.html` shows the edition title as declared on the project's website, but below it appears data obtained from the blockchain: the contract's `name` and `symbol`; and then, the edition's status (`totalMinted`, last certificate, last issuance). Without connecting any wallet, any visitor can read this information. And the data listing closes with an invitation to acquire the digital asset. 


## 9. Test network and faucet funding

The entire cycle of the **MellonKey** project was developed, deployed, and tested on **Base Sepolia**, the Base test network (chainId 84532), for reasons that remain valid also as project policy:

- **Zero iteration cost.** The contract was redeployed several times during development (each deploy produces a new address that is pasted into `editions.json`). On mainnet each iteration would have cost real gas; on testnet, nothing.
- **The flow is identical to production.** Base Sepolia is a real EVM network with real blocks and real confirmations: the claim, message signing, `ownerOf` reading, and delivery from R2 work exactly the same as on Base mainnet. Migrating means changing the `chain` block of `editions.json` and deploying the contract on the new network.
- **The ETH came from public faucets.** The gas used for deploying and for test mints was obtained from **ETH faucets for Base Sepolia** (publicly available, which give away testnet ETH by pasting the wallet address). No real money is at stake at this stage, and yet the economic behavior of the system (price, excess refund, `withdraw`) is fully exercisable: faucet ETH is "real" ETH for the test network.
- **The test price is 0.02 ETH** per certificate, configured in wei in the constructor (`20000000000000000`). When moving to mainnet, the final price is decided and a new instance of the contract is deployed — the price is not "migrated", it is declared at deployment.

Practical note: testnet SBTs **do not appear on OpenSea or most marketplaces** (which index mainnet), and it is normal that wallets do not show them with full metadata. To verify them, the test network explorers are used — see the next section, which documents the actual observed behavior.

## 10. Full visualization

### 10.1 What was observed

Previously, in other projects, the following was observed: at least in the MetaMask wallet browser extension, the display of contract data and JSON data differs in fundamental fields within the same platform and compared to other platforms. From a development standpoint, this is not a minor issue. 

- NFT, standard ERC-721, 1:1 edition, Base Mainnet: display of `name`, `description`, and `image` from the metadata JSON and of address, token ID, `name`, and `symbol` of the contract.   
- NFT, standard ERC-1155, Base Mainnet: display of `name`, `description`, and `image` from the metadata JSON; and of address, token ID, contract standard (no `name` or `symbol`).   
- NFT and SBT, standard ERC-721, Base Sepolia: display of `name`, `description`, and `image` from the metadata JSON; and of address, token ID, contract standard (no `name` or `symbol`).    

In this project, when minting from the web, at least the MetaMask wallet in its web browser extension shows: the `name`, `description`, and `image` from the Arweave JSON, along with the contract address, the tokenId, and the standard (ERC-721). It does not show: `attributes` from metadata.JSON, `name` and `symbol` of the contract address, nor the mint date established as a contract function. This is not a bug in the project or the metadata — it is a known limitation of the MetaMask renderer in web browsers, equally observable in NFTs minted on mainnet (yes, surprising but confirmed that it does not always fetch the complete information)-. In fact, the wallet does not resolve the `tokenURI` written in the contract address but rather queries data from marketplaces and explorers: it shows a basic payload, does not render the array of attributes, nor the `name` and `symbol` functions, let alone custom ones like `mintedAt()`.


### 10.2 Blockscout Explorer - Base Sepolia Chain  

The Soulbound Token minted on Testnet can be viewed with **its full metadata — including all `attributes`** — by copying the contract address from the wallet and searching for it in an explorer like Blockscout on the Base Sepolia Network/Chain:

**https://base-sepolia.blockscout.com/**

From there you can also see all the tokens acquired on Testnet (with the NFT denomination) of each wallet. 

For tokens of the ERC-721 standard, Blockscout resolves the three fields of the contract constructor when viewing the Soulbound Token in question (*View the Collection*): `name`, `symbol`, and `tokenURI` (full fields), and shows all that information plus the token ID. The JSON metadata hosted on Arweave can also be queried separately from there. 

Basescan (`sepolia.basescan.org`) also shows the attributes on the token's page, and `deliver.html` links directly to that page from the certificate panel.

It should be noted that for other standards such as ERC-1155, the visualization in explorers is by the contract's `name` and `symbol`, and in marketplaces by JSON metadata.

### 10.3 Marketplaces

Blockscout and marketplaces read **the same fields** from the contract address and from there to the JSON metadata hosted on Arweave. Blockscout renders the full standard, something some wallets do not do. 

> **If the token is fully visible on Base Sepolia's Blockscout, it will be fully visible on OpenSea and other Ethereum network marketplaces** when the edition is published on mainnet — because they all consume the data from the contract address via the same on-chain path. This sets up the immutable record of SBT ownership. 

However, MellonKey has chosen not to depend on third-party renderers to display the Certificate of Acquisition, and the project secures it through **three simultaneous surfaces**:

| Surface | What it shows | Controlled by |
|---|---|---|
| `description` of the JSON (plain text) | The certificate data as text: artist, collection, artwork + attribute note | Wallets render the `name` and `description` fields of the JSON |
| Structured `attributes` | Artist / Collection / Artwork / Edition as properties | Explorers and marketplaces (Blockscout, Basescan, OpenSea) |
| "Certificate of Acquisition" panel on `deliver.html` | Artist, artwork, tokenId, contract, and **on-chain minting date** | The site itself — always complete, without depending on wallets |

---

## 11. Decision table: what was chosen and why

| # | Decision | Discarded alternative | Rationale |
|---|---|---|---|
| 1 | **One base contract, one instance per edition (ERC-721)** | A single multi-edition contract  | Each artwork is a digital screen print: numbered certificate, with its data and a single owner (`ownerOf`). Additionally: per-artwork isolation; own name/symbol/price/metadata; auditing once serves for all |
| 2 | **Soulbound (non-transferable)** | Transferable NFT with secondary market | The token is a certificate of acquisition and a personal key; sale would break the artist→collector relationship and separate the key from the buyer |
| 3 | **Open edition with public `claim()`** | Limited edition with allowlist | Open and simple access: pay and claim; 1 per wallet prevents hoarding; pausable if the edition needs to be closed |
| 4 | **Pausable + ReentrancyGuard** | Minimal contract without defenses | Editorial control (pause without breaking already issued keys) and standard security over ETH in the functions that move funds |
| 5 | **Metadata on Arweave** | Self-hosting / IPFS only | Permanence with single payment; the certificate does not depend on live hosting. IPFS remains a non-equivalent option given the need for nodes to keep files alive |
| 6 | **A single JSON shared by the open edition** | One JSON per tokenId | Identical tokens except for the number; enormous publication savings; no metadata worker |
| 7 | **`editions.json` as the single source** | Hardcoded addresses / per-environment configuration | Adding an edition = edit JSON + upload file to R2 + redeploy the website; frontend and backend read the same config in the same deploy |
| 8 | **Serverless mint** | Checkout with own backend | The transaction is the record; zero infrastructure on the critical payment path |
| 9 | **Delivery with Pages Function (Workers runtime)** | Separate Worker / traditional server | Same repo and deploy as the site; on-chain verification + R2 in a single point; no database |
| 10 | **Authorization by signature + `ownerOf`** | Buyer database / session tokens | The blockchain IS the registry; the signature proves identity and freshness; `ownerOf` proves ownership; nothing to sync |
| 11 | **File per edition in R2 (`r2File`)** | One file per tokenId | All tokens in the open edition grant access to the same original; the individual tokenId matters for the certificate, not for the file |
| 12 | **Signature TTL of `n` min** | Signature valid forever | Anti-replay: an intercepted signature expires quickly; re-signing is free for the user |
| 13 | **Base Sepolia + faucets first** | Going straight to mainnet | Cost-free iteration with a flow identical to production; migration = redeploy + config |
| 14 | **Certificate panel on the site itself** | Trusting only wallets/marketplaces | The certificate shows artist and minting date where the project controls the render, without depending on platform quirks |

## 12. Current status and roadmap

### **Current status (v1.0.0, testnet):**

- Base contract `TestnetSBTGaladrielNenya` deployed on Base Sepolia — instance of the three editions configured in `editions.json`.
- Edition metadata published on Arweave, with certificate of acquisition in `description` and structured attributes.
- Complete site on Cloudflare Pages: mint via `claim()` serverless and file delivery with on-chain verification via Pages Function + R2.
- *Certificate of Acquisition* panel operational, with minting date read on-chain, only visible to holders.
- Public panel on `deliver.html` 
- Funding of gas and test mints with ETH from public Base Sepolia faucets.

### **Roadmap:**

**Encryption of the file in R2** — the original object will be stored encrypted; delivery will decrypt on the fly after on-chain verification. It is the natural evolution of the current access control (defined in §8.3).

**Deploy a new base contract `TestnetSBTMellonKey.sol`** with modifications: 
- The contract's `name()` will semantically unify the three editions. Review indexing in explorers and marketplaces.
- `_nextTokenId` set equal to 1, to prevent the first ID from being 0; a state that forces new code in `deliver.html`. 
- it will record `author` (text) expressing a semantic identity/attribution. It will also record the **verifiable cryptographic identities of `creator`, `crafter`, and `artist`** (wallet addresses) at the time of deployment. Thanks to **`setAttribution()`**, the owner will have the flexibility to correct or update these addresses in the future without needing to do a new deploy.
- automatic withdrawal will be set: when `claim()` is executed, the payment will be sent directly to the owner's wallet. The `withdraw(to)` function will remain as a safety backup in case any ETH gets stuck (for example, if someone sends funds to the contract without using mint).

**New data in `description`** of the metadata.JSON of each digital asset uploaded to Arweave: information already present in `attributes` will be added so that it is recorded on platforms where they are not displayed.

> Outside this portfolio project and already in development for the ·sarocha· project: the original file is the soulbound token, it is hosted encrypted on Arweave, and is decrypted through a cryptographic envelope.

## 13. Repository structure

```text
testpages-simple/
├── index.html                  # Minting page: dynamic cards + claim() serverless
├── deliver.html                # SBT usage: signature + download + public panel + certificate of acquisition panel
├── editions.json               # SINGLE SOURCE: network, contracts, r2File, prices, status
└── functions/
    └── api/
        └── download.js         # Pages Function (Workers runtime): verifies and delivers from R2
└── css/
    └── styles-deliver.css
    └── styles.css

```

## 14. Minimal glossary

| Term | Meaning in **MellonKey◉fs◉** |
|---|---|
| **SBT (Soulbound Token)** | Non-transferable ERC-721: certifies acquisition and acts as a key; cannot be sold or moved from the wallet |
| **ERC-1155** | Semi-fungible standard: the same wallet can hold several units of the same id (a balance). Discarded for certificates: without per-unit `ownerOf` there is no copy number or single owner — a balance holder is not a collector of a certificate |
| **Open edition** | Edition without a fixed quota: anyone who pays the price claims their certificate (1 per wallet) |
| **`claim()`** | Mint as a claim: payable direct wallet→contract transaction, with automatic refund of the excess |
| **wei** | Smallest unit of ETH (10⁻¹⁸). 0.02 ETH = `20000000000000000` wei |
| **Pausable** | Owner's switch that stops new `claim()`s without affecting already issued tokens |
| **ReentrancyGuard** | OpenZeppelin shield against reentrancy attacks on `claim()` and `withdraw()` |
| **`ownerOf`** | Standard function that says which wallet owns a tokenId — the system's "buyer registry" |
| **`mintedAt`** | Contract's own getter: minting date of each token, in unix seconds |
| **`tokenURI`** | URI pointing to the edition's metadata JSON (Arweave at this stage; IPFS would be equivalent) |
| **Arweave / IPFS** | Decentralized storage. We chose Arweave for permanence with a single payment; the contract is transport-agnostic |
| **Cloudflare R2** | S3-compatible object bucket where each artwork's original file lives (private, no public URL) |
| **Pages Function / Workers** | Cloudflare's serverless runtime that executes `functions/api/download.js`: verifies the signature, queries `ownerOf`, and serves the file |
| **Base Sepolia** | Base's test network (chainId 84532) where this stage was developed and deployed, with ETH from faucets |
| **Faucet** | Public service that gives away testnet ETH to pay for gas during development |
| **Blockscout** | Block explorer (base-sepolia.blockscout.com) that displays the token with its full metadata, including attributes |

♾️ *Stay human* <br>
◉fs◉
