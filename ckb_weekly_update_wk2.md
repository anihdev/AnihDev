# Builder Track Weekly Status Update -- Week 2

**Name:** Anih Soma (AnihDev)  
**Duration:** st: 25th Dec, 2025 - stp: 1st Jan, 2026.

## Courses Completed This Week

- **NFT Starter Course**  
- **Began L1 Developer Training Course**
- **Nervos Basics & CKB Tokenomics**

## What I Learned This Week

### Understanding CKB NFT Standards

This week began with understanding why NFT standards exist in the first place.

Early NFTs appeared around 2014, but real adoption didn’t occur until CryptoKitties (2017). Its explosive popularity famously congested Ethereum, pushing transaction fees to extreme levels and exposing fundamental scalability and cost issues.

This directly led to ERC721 (2018); single-asset NFT standard and ERC1155; multi-token standard enabling multiple NFT types in one contract. These standards accelerated NFT development by providing reusable templates, but over time, large-scale adoption revealed serious limitations.

### CKB-Based NFT Standards: CoTA vs Spore

* CoTA which stands for "Compact Token Aggregator" optimizes for cost efficiency

* Spore optimizes for data permanence

* Both leverage CKB’s cell model and refundable state rent

**CoTA** addresses NFT cost scalability by using **Sparse Merkle Trees (SMTs)** a **single 32-byte root hash stored on-chain**, structured off-chain data verified with on-chain proofs. Instead of storing one cell per NFT; all NFTs owned by a user are aggregated into one **CoTA cell**, ownership proofs are verified during transfers.

**Spore** on the other hand priotizes **permanence**.mEach Spore NFT lives in its own cell, stores **all content fully on-chain** and pays state rent via locked CKBytes (1 CKB = 1 byte)

Finally i took home that Choice of model top adopt depends entirely on use case, not which is better.

## L1 Developer Training: Foundations

### Cell Model Orientation

The L1 Developer Training Course reinforces that Nervos L1 is **UTXO-inspired**, **cell-based** and fundamentally different from Ethereum’s account model. The design is more flexible and sustainable, but requires deeper understanding of state transitions and explicit storage economics  

## Tools and Language requirments and setup

### JavaScript / TypeScript 
I’m already a bit comfortable with TypeScript, but revisited its relationship with JavaScript specifically in the context of CKB SDKs and off-chain transaction contsruction.

### Rust
I already had Rust installed locally, I verified my Rust packages are updated and ensured the environment is ready for development.

### Nervous Econimics and Tokenomics
dived deep into CKByte Tokenomics, understanding that the nervous token covers three types of fees Cycles (computation), Transaction Fees (security), and State Rent (storage). This creates long term sustainability and incentivizes efficient on-chain usage.

### Next
I will continue with the L1 developer course and lab setup exercises and look to start more practical exercices.