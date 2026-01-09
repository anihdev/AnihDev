# Builder Track Weekly Status Update -- Week 3

**Name:** Anih Soma (AnihDev)  
**Duration:** st: 1st Jan, 2026 - stp: 7th Jan, 2026

---

## Focus of the Week

Week 3 I transition from theory into to more of a hands-on excersices, with emphasis on:

- setting up a local CKB devnet  
- preparing official developer training materials  
- learning core ckb developer tooling (`ckb`, `ckb-cli`, Lumos config, OffCKB)  
- executing real BC transactions   


## Developer Training Course Setup

### Cloning the Training Repository

I cloned the official **Developer Training Course** repository, which contains all example code and lab exercises used throughout the L1 training, I verified my Node.js version to ensure compatibility

## Running a Local CKB Dev Blockchain

To interact with Nervos L1 locally, a full CKB node running in dev mode is required. This provides:

- a private test network
- instant block production
- pre-funded genesis cells

I explored two supported approaches.

### Option 1: Manual Devnet Setup (CKB Binary)

Using the official Nervos documentation, I completed:

- Dummy-Worker blockchain setup
- Adding Genesis Issued Cells

This command outputs the locations of critical system cells required for transaction construction.

#### Updating config.json 

The Developer Training scripts rely on correct system script references. I manually Updated the values, This configuration allows the Lumos framework to correctly locate on-chain code cells when building transactions.

### Option 2: OffCKB

I also reviewed OffCKB, an all-in-one CLI that simplifies local CKB development.


#### Starting a Devnet
I also started a Devnet node using OffCKB node start-devnet command, which automatically handles genesis cell creation and configuration.

## Executing First Transactions

### Verifying Accounts
On a devnet, two special genesis-issued accounts are present. These accounts are pre-funded with large amounts of CKBytes and are used for development and testing.

### Understanding Addresses

- Addresses are network-specific
- Devnet uses testnet-prefixed addresses (`ckt1...`)
- Using a testnet address on mainnet will always fail

For readability, addresses are commonly abbreviated as:

```
ckt1...gwga
```

### Sending a Basic Transaction

I sent CKBytes between two devnet accounts, heres theParameter breakdown:
- `--from-account`: source account
- `--to-address`: destination address
- `--capacity`: amount of CKBytes

After entering the wallet password, the command returned a transaction hash, confirming successful submission.


## Key Takeaways

- A properly configured devnet is essential for L1 development
- System script hashes must be mapped correctly for Lumos-based tooling
- `ckb-cli` exposes the raw mechanics of the cell model
- OffCKB greatly reduces setup friction and accelerates development
- Transactions are explicit state transitions between cells

## Next Steps

- Construct transactions using Lumos
- Explore mock transactions and debugging workflows
- Explore more transaction types in the Developer Training Course


