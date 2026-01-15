# Builder Track Weekly Status Update -- Week 4

**Name:** Anih Soma (AnihDev)  
**Duration:** 8th Jan, 2026 - 14th Jan, 2026

## Focus of the Week

Week 4 centered on troubleshooting and properly configuring the CKB development environment to align with official documentation standards. The week involved:

- Resolving persistent tooling and configuration issues
- Understanding the architectural differences between OffCKB and official CKB setup
- Mastering the complete CKB node initialization workflow
- Successfully configuring system cell hashes for transaction construction
- Enabling proper RPC modules for wallet operations
- Exploring CKB development tools and DApp building tutorials

## Understanding CKB Development Approaches

### OffCKB vs CKB Cli

During troubleshooting, I discovered important architectural differences between development approaches:

**OffCKB Approach:**
- Simplified, one-command devnet startup (`offckb node`)
- Abstracts away configuration complexity
- Runs RPC proxy on non-standard port (28114)
- Does not automatically configure `ckb-cli` accounts
- Ideal for rapid prototyping and testing

**CKB-Cli Approach:**
- Full control over node configuration
- Standard RPC port (8114)
- Explicit account management via `ckb-cli`
- Better alignment with production workflows
- Required for understanding core CKB mechanics

### Official CKB Devnet Setup

Completed a proper devnet following [official documentation](https://docs.nervos.org/docs/node/run-devnet-node):

**Installation & Chain Init:**
```bash
wget https://github.com/nervosnetwork/ckb/releases/download/v0.118.0/ckb_v0.118.0_x86_64-unknown-linux-gnu.tar.gz
tar -xzf ckb_v0.118.0_x86_64-unknown-linux-gnu.tar.gz
cd ckb_v0.118.0_x86_64-unknown-linux-gnu

./ckb-cli account new  # Create miner account
./ckb init --chain dev --ba-arg <lock_arg>  # Initialize devnet
```

**Startup:**
- Terminal 1: `./ckb run` (node on port 8114)
- Terminal 2: `./ckb miner` (begins block production)

**Genesis Accounts (Pre-funded):**
```bash
# Import two genesis accounts with massive CKB allocations
echo "0xd00c06bfd800d27397002dca6fb0993d5ba6399b4238b2f29ee9deb97593d2bc" > pk1
echo "0x63d86723e08f0f813a36ce6aa123bb2289d90680ae1e99d4de8cdb334553f24d" > pk2
./ckb-cli account import --privkey-path pk1
./ckb-cli account import --privkey-path pk2
```

Updated `developer-training-course/config.json` with the correct system cell references, These references are chain-specific and would differ on testnet or mainnet.

## Exploring CKB Development Tools

### CKB-CLI: Command-Line Interface

**My Experience:**
Using `ckb-cli` provided deep insights into how transactions are constructed at the protocol level. The explicit nature of commands like `wallet transfer` revealed the underlying cell model mechanics—how input cells are consumed and output cells are created. This direct interaction helped solidify my understanding of capacity calculations and lock script requirements.

### OffCKB

**My Experience:**
While OffCKB is excellent for rapid iteration, using the official CKB binary first provided essential knowledge about the underlying architecture. Understanding what OffCKB abstracts away proved valuable for debugging and production considerations.

### CKB-Debugger: Off-Chain Contract Development

**Purpose:** Standalone debugger for developing and testing CKB scripts without deploying to blockchain

**Key Capabilities:**
- **Off-Chain Execution:** Run scripts locally without blockchain interaction
- **Step-by-Step Debugging:** Line-by-line execution with breakpoints
- **Memory Inspection:** Examine script memory and stack states
- **Performance Profiling:** Measure cycle consumption (CKB's computation unit)
- **Mock Transaction Environment:** Simulate on-chain conditions locally

### DApp Building Tutorials Exploration

I explored the comprehensive [DApp tutorial series](https://docs.nervos.org/docs/dapp/transfer-ckb) provided by Nervos, which covers the fundamental patterns for building on CKB:

### Transfer CKB Tutorial

**Core Concepts:**
- Transferring CKBytes involves consuming input cells and creating output cells
- The capacity of consumed cells determines the transfer amount
- Lock scripts control who can unlock and spend cells
- CCC SDK provides high-level helper functions for transaction construction

**Code Pattern:**
```typescript
const tx = ccc.Transaction.from({
  from: senderAddress,
  to: receiverAddress,
  amount: transferAmount
});
await tx.completeInputsByCapacity(signer);
await tx.completeFeeBy(signer, feeRate);
const txHash = await signer.sendTransaction(tx);
```

## Advanced CKB Development Patterns

### Create Fungible Tokens (xUDT)
- User-Defined Tokens stored in cells with type scripts
- Token balance is uint128 in cell data
- Transfer requires collecting sender's token cells and creating outputs for receiver
- More scalable than account-based models like ERC-20

### Create Digital Objects (DOBs)
- Unique on-chain objects with programmable DNA via Spore Protocol
- Applications: game items, generative art, dynamic NFTs
- Data stored in cells controlled by type and lock scripts

### Build Custom Lock Scripts
- Scripts validate spending rights when cells are consumed
- Written in RISC-V, return 0 for success
- Common pattern: verify SECP256K1 signatures against public key hash
- Require thorough security auditing before production use

## CCC Playground: Interactive Development Environment

I explored the [CCC Playground](https://live.ckbccc.com/), an online interactive development environment for CKB

### Key Features

**Interactive Code Editor:**
- Syntax highlighting for TypeScript
- Real-time code execution without local setup
- Line-by-line debugging capabilities
- Built-in error reporting and console output
- Integrated development experience

**Transaction Visualization:**
The playground provides real-time visualization of:
- **Inputs and Outputs:** Visual representation of cells being consumed and created
- **Cell Dependencies:** Shows which system cells the transaction relies on
- **Witness Data:** Displays signatures and other witness information
- **Transaction Structure:** Complete breakdown of transaction components

**Multi-Tab Interface:**
1. **Transaction Tab:** View and analyze transaction details in structured format
2. **Scripts Tab:** Manage and edit CKB scripts with syntax support
3. **Console Tab:** Real-time output, logging, and debugging information
4. **About Tab:** Documentation and playground usage information

**Ideal Use Cases for Playground:**
- Learning CKB transaction construction
- Quick prototyping and testing
- Demonstrating concepts to others
- Debugging specific transaction issues
- Experimenting with new patterns

### Practical Applications
**Learning Transaction Construction:**
```typescript
import { ccc } from "@ckb-ccc/ccc";
import { render, signer } from "@ckb-ccc/playground";

console.log("Building transfer transaction...");

const tx = ccc.Transaction.from({
  from: await signer.getRecommendedAddress(),
  to: receiverAddress,
  amount: ccc.fixedPointFrom(100) // 100 CKB
});

// Step through transaction building
render("After creating base transaction");

await tx.completeInputsByCapacity(signer);
render("After adding inputs");

await tx.completeFeeBy(signer, 1000);
render("After calculating fees");

const hash = await signer.sendTransaction(tx);
console.log("Transaction hash:", hash);
```

**Debugging:**
The playground's step execution revealed exactly how:
- Input cells are selected and consumed
- Output cells are constructed with correct scripts
- Capacity is balanced across inputs/outputs
- Transaction fees are calculated and allocated

## Development Workflow Established
The week concluded with a fully functional, properly configured development environment:

1. **Node.js Environment**: Persists across sessions with correct versions
2. **CKB Node**: Running with proper RPC modules enabled
3. **Miner**: Continuously producing blocks for testing
4. **Accounts**: Three accounts available (1 miner + 2 genesis)
5. **Configuration**: System cell hashes correctly mapped for Lumos
6. **Tools**: `ckb-cli` fully operational for account and transaction management

This setup provides a solid foundation for the Developer Training Course exercises and DApp development.
## Resources Explored

### Official Documentation
- [Run a Devnet Node](https://docs.nervos.org/docs/node/run-devnet-node) - Comprehensive devnet setup guide
- [Platform Support](https://github.com/nervosnetwork/ckb/blob/develop/docs/platform-support.md) - OS-specific considerations
- [Transfer CKB Tutorial](https://docs.nervos.org/docs/dapp/transfer-ckb) - Basic transaction construction
- [Store Data on Cell](https://docs.nervos.org/docs/dapp/store-data-on-cell) - On-chain data storage
- [Create a Fungible Token](https://docs.nervos.org/docs/dapp/create-token) - xUDT implementation
- [CCC Playground](https://docs.nervos.org/docs/sdk-and-devtool/playground) - Interactive development environment
- [CCC Playground Live](https://live.ckbccc.com/) - Online interactive IDE

### Development tools explored 
- **CKB Binary**: Core node software for running blockchain nodes
- **ckb-cli**: Command-line interface for CKB operations and direct RPC access
- **OffCKB**: All-in-one development toolkit for rapid prototyping
- **CKB-Debugger**: Off-chain contract development and debugging tool
- **CCC Playground**: Interactive environment for building and debugging CKB transactions

## Next Steps
- Molecule format for encoding data structures.
- Molecule serialization
