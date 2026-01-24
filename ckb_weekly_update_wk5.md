# Builder Track Weekly Status Update -- Week 5

**Name:** Anih Soma (AnihDev)  
**Duration:** 14th Jan, 2026 - 20th Jan, 2026

## Focus of the Week

- Molecule serialization format and its role in CKB
- CKB RFCs covering serialization, VM syscalls, CKB-VM architecture, and transaction structure
- Data encoding specifications and type systems
- Practicals and understanding of code examples starting with Simple Lock example
- CCC Playground experiment

## Molecule Serialization

### Why Molecule?

1. **Canonicalization** - Ensures consistent byte representations across implementations
2. **Partial Reading** - Self-contained substructures enable efficient data extraction
3. **Zero-Copy Architecture** - Direct memory access improves performance in resource-constrained environments

### Data Type Categories

**Fixed-Size:** byte, array, struct  
**Dynamic-Size:** vector, table, option, union

### Memory Layout Patterns

- **Arrays/Structs:** Items stored consecutively
- **Fixed Vectors (fixvec):** 4-byte count + items
- **Dynamic Vectors (dynvec):** Header with size and offsets + items
- **Tables:** Similar to dynvec with fixed fields
- **Options:** Zero bytes (None) or inner item (Some)
- **Unions:** 4-byte type ID + variant data

### Using Molecule in CKB Scripts

```toml
molecule = { version = "0.7", default-features = false }
# For 0.8+: add features = ["bytes_vec"]
```

## CKB RFCs Study
### [RFC 0008: Serialization](https://github.com/nervosnetwork/rfcs/blob/master/rfcs/0008-serialization/0008-serialization.md)
- Fixed vs dynamic type encoding
- Headers and offsets for partial reading
- Transaction hash calculation depends on canonical serialization

### [RFC 0009: VM Syscalls](https://github.com/nervosnetwork/rfcs/blob/master/rfcs/0009-vm-syscalls/0009-vm-syscalls.md)
- `ckb_load_cell`, `ckb_load_transaction`, `ckb_load_witness`
- Partial loading capabilities
- CKB_SOURCE_INPUT vs CKB_SOURCE_GROUP_INPUT

### [RFC 0003: CKB-VM](https://github.com/nervosnetwork/rfcs/blob/master/rfcs/0003-ckb-vm/0003-ckb-vm.md)
- RISC-V based (RV64IMC), single-threaded
- 4MB memory limit per execution
- W^X security model (memory either writable OR executable)
- Dynamic linking for code reuse
- ELF binary format compatibility

### [RFC 0022: Transaction Structure](https://github.com/nervosnetwork/rfcs/blob/master/rfcs/0022-transaction-structure/0022-transaction-structure.md)
- **Components:** inputs, outputs, cell_deps, header_deps, witnesses
- **Lock scripts:** Control authorization
- **Type scripts:** Control state transitions
- **Fee calculation:** `fee = sum(inputs) - sum(outputs)`

## Simple Lock DApp

### Hash Lock Concept
- Stores hash in script args
- Requires preimage in witness
- Verifies `hash(preimage) == stored_hash`
- Intentionally insecure for learning only

### Core Script Logic
```typescript
// Load hash from args
let expect_hash = new Uint8Array(HighLevel.loadScript().args).slice(35);

// Load and hash preimage from witness
let witness_args = HighLevel.loadWitnessArgs(0, bindings.SOURCE_GROUP_INPUT);
let hash = hashCkb(witness_args.lock!);

// Verify match
return bytesEq(hash, expect_hash.buffer) ? 0 : 11;
```

### JavaScript VM Wrapper
Actual lock uses `ckb_js_vm` with embedded script hash and hash type in args.

### Security Vulnerabilities
- **Miner front-running:** Preimage visible in witness
- **Preimage exposure:** Revealed on-chain after first unlock
- **Solution:** Use public-key cryptography (secp256k1)

## Continued exploring the CCC playground 

- Tested Molecule encoding/decoding
- Validated script args formatting
- Explored transaction patterns and lock/type script implementations

## Key Takeaways

- Molecule encoding is essential for transaction data understanding
- Syscalls bridge scripts and blockchain state
- W^X memory model ensures deterministic execution
- Dynamic linking reduces deployment costs
- Cryptographic signatures are required for secure locks

## Next Steps more building along, logics and occurence happening under the hood

- Implement secure lock using cryptographic signatures
- Explore type scripts for state transitions
- Study secp256k1 lock implementation
- Experiment with multi-sig and time locks
- Build simple UDT using type scripts
- Deploy on Testnet
