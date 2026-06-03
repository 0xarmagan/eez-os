# EEZ Smart Contract Architecture — Technical Deep-Dive

**Based on:** EEZ Community Call #1 (May 13, 2026) — Jesus & Jordi Segments  
**Focus:** Smart contract core, execution model, proof verification, atomic settlement  
**Audience:** Protocol engineers, smart contract auditors, technical integrators

---

## 1. OVERVIEW: The Settlement Layer

### Core Concept

The **EEZ Settlement Contract** on Ethereum L1 is the orchestrator for all synchronous composability. It:
- Manages rollup state
- Verifies proofs from multiple provers
- Settles atomic batches across rollups
- Handles L1 ↔ L2 interactions

### Design Principles

1. **Permissionless:** Anyone registers a rollup, no gatekeeper
2. **Agnostic:** Multiple proof systems, multiple rollup types
3. **Atomic:** All interacting rollups settle in ONE transaction
4. **Deterministic:** State changes pre-verified, no optimistic windows
5. **Composable:** L1 can call L2, L2 can call L1, L2 can call L2

---

## 2. SMART CONTRACT STRUCTURE

### 2.1 Core Data Structures

```solidity
// Rollup Registration
struct Rollup {
    address owner;                    // Who can modify config
    address proofContract;            // Verifies proofs for this rollup
    mapping(bytes32 => bool) provers; // Which proof systems accepted
    bytes32 currentState;             // Current L2 state root
    uint256 lastUpdate;               // Timestamp of last settlement
}

// Execution Entry (Pre-verified state change)
struct ExecutionEntry {
    address source;                   // L1 or which L2
    address target;                   // L1 or which L2
    bytes callData;                   // What to execute
    bytes expectedResult;             // What we expect back
    bool settled;                     // Has this been executed?
}

// State Delta (Result of execution)
struct StateDelta {
    bytes32 stateRoot;                // New state after execution
    bytes returnData;                 // Return value(s)
    bool success;                     // Did execution succeed?
}

// Batch (Multiple transactions in one settlement)
struct Batch {
    uint256 batchNumber;
    mapping(uint256 => ExecutionEntry) entries;
    mapping(uint256 => StateDelta) deltas;
    mapping(bytes32 => bool) proofsReceived;
    uint256 entryCount;
}
```

### 2.2 Core Interface

```solidity
contract EEZSettlement {
    
    // ============ ROLLUP MANAGEMENT ============
    
    /// Register a new rollup (permissionless)
    function registerRollup(
        address rollupOwner,
        address proofVerifierContract,
        bytes32[] calldata acceptedProverTypes
    ) external returns (uint256 rollupId);
    
    /// Rollup can update which provers it accepts
    function setAcceptedProvers(
        uint256 rollupId,
        bytes32[] calldata proverTypes
    ) external onlyRollupOwner;
    
    
    // ============ BATCH SUBMISSION & SETTLEMENT ============
    
    /// Submit proof + execution entries for atomic settlement
    function postAndVerifyBatches(
        uint256[] calldata rollupIds,          // Which rollups involved
        ExecutionEntry[] calldata entries,     // What's happening
        bytes[] calldata proofs,               // Proofs from each prover
        bytes32[] calldata proverTypes         // Which prover each proof from
    ) external;
    
    /// Verify that all proofs are valid for their respective rollups
    function verifyAllProofs(
        uint256[] calldata rollupIds,
        bytes[] calldata proofs,
        bytes32[] calldata proverTypes
    ) internal returns (bool);
    
    /// Execute all entries atomically
    function executeEntries(
        ExecutionEntry[] calldata entries
    ) internal returns (StateDelta[] memory);
    
    /// Update state of all affected rollups
    function settleRollups(
        uint256[] calldata rollupIds,
        StateDelta[] calldata deltas
    ) internal;
    
    
    // ============ CROSS-CHAIN INTERACTIONS ============
    
    /// Create a proxy for calling a rollup contract from L1
    function createCrossChainProxy(
        uint256 targetRollupId,
        address targetAddress
    ) external returns (address proxyAddress);
    
    /// Execute a call on L1 contract that may have L2 interactions
    function executeL1Call(
        address target,
        bytes calldata callData
    ) external returns (bytes memory);
    
    /// Handle L1 ← L2 callbacks
    function processL2Response(
        uint256 sourceRollupId,
        ExecutionEntry calldata entry,
        bytes calldata result
    ) internal;
}
```

---

## 3. EXECUTION MODEL: The Core Innovation

### 3.1 The Execution Entry Concept

**Key Insight:** Instead of waiting for transactions to execute and then proving them (slow), EEZ pre-verifies what WILL happen.

#### Traditional Flow (Other L2s):
```
1. Submit transaction to L2
2. L2 executes transaction
3. Wait for block finality
4. Create proof of execution
5. Submit proof to L1
6. L1 verifies proof
→ Total time: Minutes to hours
```

#### EEZ Flow (Synchronous):
```
1. Creator predicts what WILL happen
   "Uniswap on L2A will do swap X, return amount Y"
2. Provers verify this prediction
3. If correct → settle with predicted results
4. If wrong → batch rejected, try again
→ Total time: One Ethereum block (~12s)
```

### 3.2 Execution Entry Lifecycle

```
┌─────────────────────────────────────────────────┐
│ CREATE: Predict what will happen               │
│                                                 │
│ {                                               │
│   source: L1 or L2A                           │
│   target: L2B (Uniswap contract)              │
│   callData: swap(USDC, ETH, 1000)             │
│   expectedResult: 0.5 ETH                      │
│ }                                               │
└──────────────────┬────────────────────────────┘
                   │
                   ↓
┌─────────────────────────────────────────────────┐
│ PROVE: Verify this prediction is correct       │
│                                                 │
│ Provers must prove:                             │
│ - Call is valid (correct contract address)     │
│ - Call executes without revert                 │
│ - Call returns exactly expectedResult          │
│ - State updates are as expected               │
│                                                 │
│ If any prover says "wrong" → batch fails      │
└──────────────────┬────────────────────────────┘
                   │
                   ↓
┌─────────────────────────────────────────────────┐
│ SETTLE: If all proofs pass                      │
│                                                 │
│ Execute:                                        │
│ 1. Update L2B state (swap executed)            │
│ 2. Create StateDelta (new state root)          │
│ 3. Return expectedResult to caller             │
│ 4. Mark entry as settled                       │
└─────────────────────────────────────────────────┘
```

### 3.3 State Deltas

```solidity
/// After verification, create state delta
struct StateDelta {
    bytes32 newStateRoot;    // L2 state after execution
    bytes returnData;        // What contract returned
    bool success;            // Did execution work?
}

// Example:
StateDelta uniswapDelta = StateDelta({
    newStateRoot: keccak256(abi.encode(newL2AState)),
    returnData: abi.encode(uint256(0.5 ether)),
    success: true
});
```

---

## 4. THE ATOMIC SETTLEMENT MECHANISM

### 4.1 Multi-Rollup Atomic Execution

The core innovation: **All interacting rollups settle together or not at all.**

#### Problem It Solves:

```
Without atomic settlement:
┌────────────────┐        ┌────────────────┐
│ Settlement L2A │ ✅     │ Settlement L2B │ ❌
│ User sends ETH │        │ User gets USDC │
│ (settled)      │        │ (not settled)  │
└────────────────┘        └────────────────┘

Result: User lost ETH, never got USDC (double-spend risk)


With atomic settlement:
Either:
┌────────────────┐        ┌────────────────┐
│ Settlement L2A │ ✅     │ Settlement L2B │ ✅
│ User sends ETH │        │ User gets USDC │
│ (settled)      │        │ (settled)      │
└────────────────┘        └────────────────┘

Or:
┌────────────────┐        ┌────────────────┐
│ Settlement L2A │ ❌     │ Settlement L2B │ ❌
│ User sends ETH │        │ User gets USDC │
│ (not settled)  │        │ (not settled)  │
└────────────────┘        └────────────────┘

Result: Both succeed or both fail (atomic, safe)
```

### 4.2 The Settlement Algorithm

```solidity
function postAndVerifyBatches(
    uint256[] calldata rollupIds,      // e.g., [1, 2, 3] (L2A, L2B, L2C)
    ExecutionEntry[] calldata entries, // All calls in batch
    bytes[] calldata proofs,           // Proofs from each prover
    bytes32[] calldata proverTypes     // Which prover each proof from
) external {
    
    // STEP 1: Validate inputs
    require(entries.length > 0, "No entries");
    require(proofs.length == proverTypes.length, "Proof/prover mismatch");
    
    
    // STEP 2: Pre-verify all entries are syntactically correct
    for (uint i = 0; i < entries.length; i++) {
        ExecutionEntry memory entry = entries[i];
        require(entry.source != address(0), "Invalid source");
        require(entry.target != address(0), "Invalid target");
        // ... more validation
    }
    
    
    // STEP 3: Verify ALL proofs BEFORE touching any state
    //         (ensures atomicity: all verify or none do)
    bool[] memory allProofsValid = new bool[](proofs.length);
    
    for (uint i = 0; i < proofs.length; i++) {
        address proverContract = getProverContract(proverTypes[i]);
        allProofsValid[i] = proverContract.verify(
            proofs[i],
            abi.encode(entries)  // Proof is about these entries
        );
        
        require(allProofsValid[i], "Proof verification failed");
    }
    
    
    // STEP 4: All proofs passed, now execute entries
    StateDelta[] memory deltas = executeEntries(entries);
    
    
    // STEP 5: All executions succeeded, update all rollups atomically
    //         This is a single Ethereum transaction, so atomic at L1 level
    for (uint i = 0; i < rollupIds.length; i++) {
        uint256 rollupId = rollupIds[i];
        Rollup storage rollup = rollups[rollupId];
        
        // Update state root
        rollup.currentState = deltas[i].newStateRoot;
        rollup.lastUpdate = block.number;
    }
    
    
    // STEP 6: Emit event (for off-chain verification)
    emit BatchSettled(
        rollupIds,
        entries.length,
        block.timestamp
    );
}
```

### 4.3 Why This Is Safe

```
Ethereum guarantees:
- All code in one transaction executes atomically
- Either all succeeds or all reverts
- No partial state changes

Therefore:
┌─────────────────────────────────────────────┐
│ Within postAndVerifyBatches transaction:    │
│                                             │
│ 1. Verify all proofs ✅                    │
│ 2. Execute all entries ✅                  │
│ 3. Update ALL rollup states ✅             │
│                                             │
│ = Single Ethereum transaction               │
│ = Atomic at Ethereum L1 level               │
│ = Either all 3 happen or transaction reverts│
└─────────────────────────────────────────────┘
```

---

## 5. CROSS-CHAIN INTERACTION PATTERNS

### 5.1 The Execution Table

Before execution, the system builds a table of what needs to happen:

```
Internal Table (Rollup A state changes):
┌──────────────────────────────────────────┐
│ Action | Target | CallData | Result      │
├──────────────────────────────────────────┤
│ 0      | User B | Transfer | 1 ETH sent  │
│ 1      | Token  | Burn     | 100 burned  │
│ 2      | Aave   | Repay    | Success     │
└──────────────────────────────────────────┘

From L1 perspective (what L1 sees):
┌──────────────────────────────────────────────────┐
│ Action | Source | Target | L2 State | Result    │
├──────────────────────────────────────────────────┤
│ 0      | L1     | L2A    | Hash XYZ | OK        │
│ 1      | L1     | L2B    | Hash ABC | OK        │
│ 2      | L2A    | L1     | Hash XYZ | Returns 5 │
└──────────────────────────────────────────────────┘

Hash: All interactions + state changes are hashed together
Proof: Provers prove hash is correct
```

### 5.2 Simple Example: L1 → L2A (Transfer)

```solidity
// User on L1 wants to send 1 ETH to user B on L2A

// 1. CREATE EXECUTION ENTRY
ExecutionEntry entry = ExecutionEntry({
    source: address(0),              // L1 (0x0 = L1)
    target: L2A_ROLLUP_ID,
    callData: abi.encodePacked(
        "transfer",
        userB,                        // recipient
        1 ether
    ),
    expectedResult: abi.encode(true), // Success expected
    settled: false
});

// 2. PROVER VERIFIES:
// "If you execute this on L2A, will user B receive 1 ETH?"
// Prover: "Yes, I can prove it"

// 3. SETTLEMENT:
// - L2A state is updated (1 ETH moved to userB)
// - Execution entry marked settled
// - User gets confirmation (same Ethereum block)
```

### 5.3 Complex Example: L1 → L2A → L1 → L2B

```
User on L1 wants to:
1. Call Uniswap on L2A (swap USDC → ETH)
2. Use result to repay Aave on L2B
3. Get final result back on L1

execution entries:

Entry 1: L1 → L2A (Uniswap)
  - callData: swap(USDC, ETH, 1000)
  - expectedResult: 0.5 ETH (approximately)

Entry 2: L2A → L1 (callback with result)
  - callData: [return 0.5 ETH to L1]
  - expectedResult: 0.5 ETH received on L1

Entry 3: L1 → L2B (Aave)
  - callData: repay(aUSDC, amount=500)
  - expectedResult: success

Entry 4: L2B → L1 (final callback)
  - callData: [return Aave receipt to user]
  - expectedResult: receipt hash

PROOFS:
- Zisk proves: Entry 1 & 2 (L2A)
- SP1 proves: Entry 3 & 4 (L2B)

SETTLEMENT:
- All 4 entries execute atomically
- User receives: Aave repay receipt on L1
- All in ONE Ethereum transaction
```

---

## 6. PROOF SYSTEM INTEGRATION

### 6.1 Pluggable Architecture

```solidity
// Each prover is a separate contract implementing this interface
interface IProofVerifier {
    function verify(
        bytes calldata proof,
        bytes calldata publicInput  // The execution entries
    ) external view returns (bool);
}

// Example implementations:
contract ZiskVerifier is IProofVerifier { ... }
contract SP1Verifier is IProofVerifier { ... }
contract OpenBMVerifier is IProofVerifier { ... }
contract MultiSigVerifier is IProofVerifier { ... }  // m-of-n signatures

// Rollup decides which to accept:
rollup.acceptedProvers = [
    keccak256("zisk"),
    keccak256("sp1"),
    keccak256("openBM")
];

// When verifying:
for each proof:
    proverContract = getProver(proverType)
    require(proverContract.verify(proof, entries))
```

### 6.2 Multi-Prover Verification

The key innovation: **Multiple provers prove different chunks of the same batch.**

```
Scenario: 10 transactions across 5 rollups = 1000+ chunks

Without EEZ:
  1 Prover must prove all 1000 chunks
  = 1 huge proof (expensive, slow)

With EEZ:
  Chunk 1-300:   Zisk (300 chunks)   → Proof A
  Chunk 301-600: SP1 (300 chunks)    → Proof B
  Chunk 601-900: openBM (300 chunks) → Proof C
  Chunk 901-1000: Multisig (100)     → Signature

EEZ Settlement Contract:
┌─────────────────────────────────────┐
│ for each (proof, proverType):       │
│   require(verify(proof, proverType))│
│                                     │
│ if all proofs valid:                │
│   execute all 1000 chunks atomically│
│ else:                               │
│   revert entire batch              │
└─────────────────────────────────────┘

Result: 4 proofs instead of 1 giant proof
        Cost shared across all rollups
        Parallel verification possible
```

### 6.3 Proof Structure

```solidity
// What each prover proves:
struct ProofInput {
    // Rollup state before
    bytes32 prevStateRoot;
    
    // Chunks this prover is responsible for
    uint256[] chunkIndices;           // Which chunks to prove
    
    // Rollup state after
    bytes32 newStateRoot;
    
    // State transitions
    // For each chunk:
    //   input state → execution → output state
    bytes[] stateTransitions;
}

// Example proof verification:
function verifyProof(
    bytes calldata proof,
    ProofInput calldata input
) external view returns (bool) {
    // Zisk verifier would:
    // 1. Decompress the proof
    // 2. Verify each chunk in input.chunkIndices
    // 3. Check state root transitions
    // 4. Return true if all valid
    
    // This is SNARK/STARK specific
    // We don't need to know the details
    // Just that it returns true/false
}
```

---

## 7. STATE MANAGEMENT: Execution Entries & State Deltas

### 7.1 The Execution Entry Flow

```
PHASE 1: DEFINITION
┌──────────────────────────────────────┐
│ Create execution entries describing  │
│ what WILL happen (prediction)        │
│                                      │
│ ExecutionEntry {                     │
│   source: L1                         │
│   target: L2A                        │
│   callData: uniswap.swap(...)        │
│   expectedResult: 0.5 ETH           │
│   settled: false                     │
│ }                                    │
└──────────────────────────────────────┘

PHASE 2: VERIFICATION
┌──────────────────────────────────────┐
│ Provers verify entries are correct   │
│                                      │
│ For each entry:                      │
│   Does execution match expectation?  │
│   Will state transition be valid?    │
│                                      │
│ Return: All match? ✅                │
│         Any mismatch? ❌             │
└──────────────────────────────────────┘

PHASE 3: EXECUTION & SETTLEMENT
┌──────────────────────────────────────┐
│ Execute all entries                  │
│ Create state deltas (results)        │
│                                      │
│ StateDelta {                         │
│   newStateRoot: hash(newL2AState)    │
│   returnData: 0.5 ETH                │
│   success: true                      │
│ }                                    │
│                                      │
│ Update rollup:                       │
│   rollup.currentState = newStateRoot │
│   rollup.lastUpdate = block.number   │
└──────────────────────────────────────┘
```

### 7.2 Handling Complex Cases

#### Case 1: Revert Handling

```solidity
// What if execution reverts?

ExecutionEntry entry = {
    target: L2A_BadContract,
    callData: badFunc(),
    expectedResult: ???  // What should happen on revert?
};

// Two options:
// Option A: Include revert in expectedResult
//   expectedResult: abi.encode(false, "Reverted")
//   Prover proves: "Execution will revert"
//   Settlement: Mark as reverted but settled

// Option B: Batch fails if any entry reverts
//   require(allEntriesSuccess);
//   Batch is rejected, entire batch reverts
```

#### Case 2: Re-entrant Calls

```
User on L1 calls Compound on L2A
Compound on L2A calls back to L1 (callback)
L1 contract calls flashloan on L2B
L2B returns funds to L1
L1 repays Compound
Compound returns to L2A
L2A returns to user

This is 7 nested steps, all atomic!

EEZ handles this by:
1. Each step is an execution entry
2. All entries pre-verified by provers
3. All settled in ONE Ethereum transaction
4. Safe because atomic at L1 level
```

---

## 8. CROSS-CHAIN PROXIES

### 8.1 The Proxy Mechanism

**Problem:** How do users interact with L2 contracts without knowing they're on L2?

**Solution:** Permissionless proxy creation

```solidity
// Anyone can create a proxy for any L2 contract
function createCrossChainProxy(
    uint256 targetRollupId,
    address targetAddress  // L2 contract address
) external returns (address proxyAddress) {
    
    // Create new proxy contract
    CrossChainProxy proxy = new CrossChainProxy(
        targetRollupId,
        targetAddress,
        address(this)  // Reference to EEZ Settlement
    );
    
    return address(proxy);
}

// The proxy contract:
contract CrossChainProxy {
    uint256 immutable targetRollupId;
    address immutable targetAddress;
    EEZSettlement immutable eez;
    
    // User calls proxy like normal L1 contract
    fallback() external payable {
        // Forward call to EEZ Settlement
        // EEZ will:
        // 1. Create execution entry
        // 2. Verify it will work on target L2
        // 3. Execute it atomically
        // 4. Return result to user
        
        bytes memory callData = msg.data;
        bytes memory result = eez.executeL2Call(
            targetRollupId,
            targetAddress,
            callData
        );
        
        // Return result to user
        assembly {
            return(add(result, 0x20), mload(result))
        }
    }
}

// User experience:
// User treats proxy like normal L1 contract
// Proxy transparently forwards to L2
// User gets result in same transaction
// User doesn't even know L2 is involved
```

### 8.2 Example Usage

```solidity
// Uniswap deployed on L2A at 0x1234...
// Proxy created at 0x9999...

// User interaction:
IUniswapRouter uniswap = IUniswapRouter(0x9999);

// User calls proxy (thinks it's L1 Uniswap)
uint amountOut = uniswap.swapExactTokensForTokens(
    amountIn,
    minAmountOut,
    path,
    address(this),
    deadline
);

// What actually happens:
// 1. Proxy receives call
// 2. Proxy forwards to EEZ Settlement
// 3. EEZ creates execution entry:
//    {
//      source: L1 (proxy)
//      target: L2A (Uniswap at 0x1234)
//      callData: swap(...)
//      expectedResult: amountOut
//    }
// 4. EEZ verifies with L2A prover
// 5. L2A state updates (swap executed)
// 6. Result returned to user
// 7. User gets amountOut in same transaction
//
// Total time: ~12 seconds (one Ethereum block)
// UX: Identical to native L1 contract
```

---

## 9. GAS COST OPTIMIZATION

### 9.1 Nested Flattened Approach

**Problem:** Original implementation used table-based approach (complex, expensive)

**Solution:** Switched to nested flattened structure

```solidity
// OLD (Table-based, expensive):
struct ExecutionTable {
    mapping(uint256 => ExecutionEntry) entries;
    mapping(uint256 => StateDelta) deltas;
}
// Gas cost: High (multiple storage reads/writes)

// NEW (Nested flattened, optimized):
struct FlattenedExecution {
    bytes32 stateRoot;
    bytes callData;
    bytes result;
    uint256 gasUsed;
}
// Gas cost: Lower (fewer storage ops)

// Result: Significant gas cost improvement
//         - Nested calls cheaper
//         - Multi-rollup batches more efficient
//         - Proof sharing reduces gas (fewer proofs)
```

### 9.2 Cost Sharing Across Rollups

```
Scenario: 5 rollups all use Zisk prover

Traditional (1 proof per rollup):
  Rollup A: 1 Zisk proof (100 chunks)  → 500k gas
  Rollup B: 1 Zisk proof (100 chunks)  → 500k gas
  Rollup C: 1 Zisk proof (100 chunks)  → 500k gas
  Rollup D: 1 Zisk proof (100 chunks)  → 500k gas
  Rollup E: 1 Zisk proof (100 chunks)  → 500k gas
  ─────────────────────────────────────
  Total: 5 × 500k = 2.5M gas

EEZ (1 shared proof):
  Zisk proof (500 chunks from all 5)    → 600k gas
  ─────────────────────────────────────
  Total: 600k gas

Savings: 75% reduction in verification gas
         Cost amortized across 5 rollups
```

---

## 10. SAFETY & SECURITY

### 10.1 Atomicity Guarantees

```solidity
// All-or-nothing semantics
function postAndVerifyBatches(...) external {
    
    // If ANY of these fail, entire transaction reverts
    
    // 1. Proof verification
    for (uint i = 0; i < proofs.length; i++) {
        require(verifyProof(proofs[i]));  // ← Reverts entire tx
    }
    
    // 2. Entry execution
    StateDelta[] memory deltas = executeEntries(entries);
    // Reverts if any entry fails
    
    // 3. State update
    for (uint i = 0; i < rollupIds.length; i++) {
        rollups[rollupIds[i]].currentState = deltas[i].newStateRoot;
    }
    // All update or none
}

// Guarantees:
// - Either ALL rollups update or NONE
// - No partial settlement possible
// - Atomicity at Ethereum L1 level
```

### 10.2 Proof System Diversity

```
If ONE prover is malicious:

Rollup A (1 prover):
  Accepts: [Zisk]
  If Zisk malicious → Rollup A unsafe

Rollup B (2-of-3 provers):
  Accepts: [Zisk, SP1, openBM]
  If Zisk malicious → Still safe (need 2 valid)
  If 2+ malicious → Unsafe (but unlikely)

Rollup C (different provers):
  Accepts: [Zisk, multisig]
  If Zisk malicious → Multisig still validates
  Safe if multisig is honest

Result: Heterogeneous trust model
        No single point of failure
        System resilient to 1 malicious prover
```

### 10.3 Pre-Verification vs Optimistic

```
OPTIMISTIC ROLLUPS (e.g., Optimism):
1. Submit transaction
2. Execute on L2 (assume valid)
3. Create fraud proof if someone disagrees
4. Wait for fraud proof window (7 days)
5. Finalize on L1

Result: 7-day withdrawal delays, fraud proofs possible

EEZ (DETERMINISTIC):
1. Create execution entry (prediction)
2. Provers verify prediction is correct
3. Execute and settle atomically
4. User has result immediately

Result: No fraud window, deterministic, atomic
```

---

## 11. ROADMAP MILESTONES & CURRENT IMPLEMENTATION

### 11.1 Current Status (May 2026)

```
SPEC: ✅ Complete and finalized
  - Smart contract interface defined
  - Execution model specified
  - Proof system abstraction finalized
  - All edge cases addressed (revert, reentrancy, nested calls)

SMART CONTRACT: 🔄 Final version in development
  - Refactored from table to nested flattened
  - Gas optimizations complete
  - Audit-ready by mid-July
  
  Key changes since POC:
  - Naming conventions standardized
  - State management optimized
  - Multi-prover support robust
  - Error handling comprehensive
```

### 11.2 Audit Timeline

```
Mid-July 2026: Smart contract audit complete
  - Security review of:
    - Settlement mechanism
    - Atomicity guarantees
    - Proof verification
    - State management
  
  - Expected to address:
    - Edge cases in execution entries
    - Reentrancy patterns
    - Gas optimization
    - Storage safety
```

### 11.3 Rollup Zero Launch (Mid-June)

```
Features:
✅ L2 → L1 calls
✅ L1 → L2 calls
✅ Proof verification (limited)
❌ Full nested composability (V2)
❌ All proof systems (V2)

Purpose:
- Test core mechanism
- Mature technology
- Community feedback
- Ready for Rollup One in summer
```

### 11.4 Rollup One Launch (Summer 2026)

```
Features:
✅ All L1 ↔ L2 interactions
✅ L2 ↔ L2 composability
✅ Nested calls (arbitrary depth)
✅ Multi-prover system
✅ Full feature completeness
✅ Reference implementation

Purpose:
- Production-ready
- Full vision demonstrated
- Framework open for all rollups
```

---

## 12. IMPLEMENTATION CHECKLIST FOR ROLLUPS

### Rollup Integration Requirements

```solidity
// For a rollup to join EEZ, implement:

interface IEEZRollup {
    
    // 1. Register with EEZ
    function registerWithEEZ(
        address eezSettlement,
        address proofVerifier,
        bytes32[] acceptedProvers
    ) external returns (uint256 rollupId);
    
    // 2. Accept proof updates from EEZ
    function updateState(
        bytes32 newStateRoot,
        StateDelta[] calldata deltas
    ) external onlyEEZSettlement;
    
    // 3. Provide state root on demand
    function getStateRoot() external view returns (bytes32);
    
    // 4. Execute L2 logic
    function executeEntry(
        ExecutionEntry calldata entry
    ) external returns (bytes memory result);
}
```

---

## 13. PSEUDOCODE: Full Settlement Flow

```pseudocode
function settleAtomically(
    rollupIds: uint256[],
    entries: ExecutionEntry[],
    proofs: bytes[],
    proverTypes: bytes32[]
):
    
    // STEP 1: Validate structural correctness
    assert(proofs.length == proverTypes.length)
    assert(entries.length > 0)
    
    // STEP 2: Verify ALL proofs before touching state
    for each proof in proofs:
        prover = getProver(proverType)
        assert(prover.verify(proof, entries))
    
    // STEP 3: Execute all entries
    for each entry in entries:
        if entry.source == L1:
            result = executeOnL1(entry)
        else if entry.source == L2X:
            result = executeOnL2(entry)
        
        assert(result == entry.expectedResult)
        stateDeltas.push(result)
    
    // STEP 4: Update all rollup states atomically
    for each rollupId in rollupIds:
        rollups[rollupId].state = stateDeltas[rollupId].newStateRoot
        rollups[rollupId].lastUpdate = block.timestamp
    
    // STEP 5: Emit settlement event
    emit BatchSettled(rollupIds, entries.length)

// If ANY step fails, entire transaction reverts (atomicity)
```

---

## 14. REFERENCE: Core Formula

### Atomic Composability Equation

```
Synchronous Composability = 
    Real-Time Proving + 
    Execution Entries + 
    Atomic Settlement + 
    Pluggable Proof Systems + 
    Permissionless Rollups

Where:
  Real-Time Proving     = Zisk innovation (state in ~12s)
  Execution Entries     = Pre-verify what WILL happen
  Atomic Settlement     = All succeed or all fail
  Pluggable Provers     = Each rollup chooses (heterogeneous trust)
  Permissionless Rollups = No gatekeeper (open framework)

Result: 
  Users see: "Same as L1, but with options"
  Developers see: "Composable network, no replication needed"
  System sees: "One Ethereum, many chains"
```

---

## 15. REMAINING QUESTIONS

### Q: What happens if a prover submits invalid proof?

**A:** 
```solidity
require(prover.verify(proof, entries));  // ← Reverts if false

// Entire batch rejected
// No state changes occur
// Proof fees may be lost (not yet defined)
// Batch can be resubmitted with correct proofs
```

### Q: How are state roots chosen for L2s?

**A:**
```
Each L2 defines its own:
- State root calculation method
- Canonical state root source (trusted source)
- Update frequency

EEZ accepts whatever L2 provides
(EEZ is agnostic to L2 internals)
```

### Q: What about finality on L2s?

**A:**
```
EEZ settlement happens when:
- All proofs verified ✅
- Entry execution succeeds ✅
- State roots updated ✅

After this point:
- Settlement is final on Ethereum
- Cannot be reversed (except Ethereum reorg)
- L2s cannot dispute settled state
```

---

*Technical Deep-Dive completed from May 13, 2026 EEZ Community Call  
Speakers: Jesus, Jordi, Eduardo (Edu), Yesus, Philip*

*For implementation details, see:*  
- *Smart contract audit materials (coming mid-July)*  
- *Rollup integration guide (Rollup Zero launch)*  
- *Proof system specifications (per-prover docs)*
