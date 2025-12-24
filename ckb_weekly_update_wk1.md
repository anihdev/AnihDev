# Builder Track Weekly Status Update -- Week 1

**Name:** Anih Soma (AnihDev)  
**Duration:** <st: 18th Dec, 2025 - stp: 25th Dec, 2025>

## Courses Completed This Week

- **CKB Basic Theoretical Knowledge**
- **CKB Basic Practical Operation**
- **NFT Starter Course** *(in progress)*

## What I Learned This Week

### Understanding CKB

This week helped me understand that CKB is fundamentally a **chain of cells**. CKB which means **Common Knowledge Base** trying to make it known that only truly valuable data belongs onchain. Everything, balances, contracts, NFTs; ultimately comes down to cells being created and destroyed through transactions, very similar to Bitcoin’s UTXO (i.e Uspent transaction Output) model but far more flexible.

A cell is best thought of as a small container that:

* Holds capacity (paid in CKB)
* Stores arbitrary data
* Is protected by scripts

Live cells represent current state; spent cells become dead.  
The global state of CKB is simply the collection of all live cells.

### Capacity = Storage Economics

One of the most important ideas was that **1 CKB = 1 byte of on-chain storage**.  
This makes storage a scarce and valuable resource.

**Key implications:**

* Every cell must pay for its own size
* Large data (e.g. NFTs, binaries) must be carefully designed
* On-chain storage encourages efficiency and off-chain composition

This design strongly reinforces CKB’s idea of a **Common Knowledge Base** only truly valuable data belongs on-chain.


### Cell Structure

Each cell consists of four core fields:

* **capacity** - how much space the cell can occupy
* **lock script** - who can spend the cell *(mandatory)*
* **type script** - what rules the cell must follow *(optional)*
* **data** - arbitrary bytes (state, metadata, or even executable code)

A cell’s total byte size must never exceed its capacity.


### Lock Scripts vs Type Scripts (Clear Separation of Concerns)

I understood the dynamism of the dual concepts

#### Lock Script
* Controls ownership  
* Runs only on inputs  
* Commonly checks cryptographic signatures  
* Acts as the **gatekeeper**

#### Type Script
* Controls state transition rules  
* Runs on both inputs and outputs  
* Used for tokens, NFTs, and application logic  
* Acts as the **guardian of behavior** more like a code of coduct to abide with when interacting with the code


### Where Smart Contract Code Lives

A major insight was realizing that scripts don’t contain code they just reference it.

* Actual contract code is stored as **RISC-V binaries inside cells**
* These are referenced via **cell_deps**
* `code_hash` + `hash_type` determine how the VM locates and executes code

This design avoids duplication and allows many cells to share the same logic (e.g. SUDT, NFT standards).


### Transactions = State Transitions

At its essence, a transaction is: `Inputs (live cells) >> Outputs (new live cells)`

## Practical Works

* Inspected live cells and their JSON structure
* Observed real transactions and block data via explorer
* Located real on-chain contract code (e.g. **SUDT**) via dep cells
* Began **NFT Starter Course**, focusing on how NFTs are modeled as cells with type scripts
