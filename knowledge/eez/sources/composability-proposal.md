# **Proposal: Composability Between Rollups on Ethereum**

## **1\. Motivation**

Ethereum’s long-term scalability roadmap relies on rollups as the primary execution environment. As the number and diversity of rollups grows, composability becomes the limiting factor for building complex applications that span multiple execution domains.

Today, most rollups interact through **asynchronous, decentralized bridge mechanisms** that do not rely on trusted third parties, but nonetheless introduce fundamental constraints, including::

Latency measured in minutes or hours

Fragmented liquidity and state

Additional trust and operational assumptions

This proposal is motivated by the observation that not all rollups need to be treated equally. By distinguishing Native Rollups—those that publish their transaction data directly on Ethereum L1 and provide validity proofs in real time—we can recover a form of composability that closely resembles Ethereum’s original synchronous execution model.

The core idea is to support two complementary composability techniques:

* Synchronous composability, enabled by L1 data availability and real-time proving, and primarily used by native rollups.  
* Asynchronous composability, used when synchronous execution is not possible, in particular for non-native rollups.  
* This dual model preserves flexibility while enabling a fast path for applications that require atomic cross-rollup interactions.

By anchoring all communication, events, and proofs on Ethereum L1, the system:

* Avoids trusted bridges and multisigs  
* Preserves Ethereum’s security and neutrality  
* Allows heterogeneous execution environments (EVM and non-EVM) to coexist

Ultimately, the goal is to make multiple rollups behave like a single, composable system, without sacrificing scalability or decentralization.

---

## **2\. Native Rollups (NRs)**

### **2.1 Initial Definition**

We start by defining Native Rollups as rollups whose transactions are published directly on Ethereum L1 as data availability (DA), and whose correctness is guaranteed by a consensuated state transition function (STF) agreed at the protocol level.

In the baseline model, native rollups are assumed to follow the standard EVM state transition function:

Transactions are EVM transactions.

Execution semantics match Ethereum mainnet.

Existing smart contracts and tooling can be reused with minimal friction.

This EVM-based definition provides a familiar and conservative starting point for native rollups.

### **2.2 Generalized Native Rollups**

Importantly, this proposal does not restrict native rollups to EVM semantics.

A native rollup is more generally defined by:

Publishing its transaction data on L1.

Following a consensuated state transition function, with proofs used to attest correct execution.

Making its state deterministically defined as soon as DA is available.

Under this generalized definition, it is possible to define custom native rollups, where the state transition function (STF) is explicitly defined by the rollup designer and may differ from the EVM.

Examples include:

* Application-specific rollups  
* ZK-optimized execution environments  
* Domain-specific virtual machines

The implications and constraints of custom native rollups are discussed in a later section. For the remainder of this section, references to native rollups should be understood to include both EVM-based native rollups and custom native rollups unless stated otherwise.

---

## **3\. Synchronous Composability for Native Rollups**

Synchronous composability is the key differentiator of Native Rollups. Because their state is defined immediately after DA publication (with proof), they can execute cross-rollup logic atomically within the same L1 block.

A transaction in Rollup A may synchronously call a contract in Rollup B. This interaction:

* Transfers Ether from A → B as part of the call.  
* Transfers gas-associated Ether needed for execution.  
* Guarantees that Rollup A cannot send more Ether than it previously received. (Specially important for Custom Native Rollups)

Execution flow:

1. Rollup A executes a transaction that invokes a function in Rollup B.  
2. A partial transaction is created and executed in Rollup B.  
3. Rollup B returns results, creating a return partial transaction back in Rollup A.  
4. All partial transactions execute atomically and synchronously across both rollups.

The final combined state is fully deterministic and identical for all verifiers.

---

## **4\. Data Availability \+ Proof Coupling**

Native Rollups publish data availability (DA) together with state transition proofs in the same L1 interaction. This model:

* **Removes the need for special prover-incentive mechanisms**, since proving is an inherent part of block production rather than a separate, optional process.

* **Ensures the explicit post-state is available and verified immediately**, making state queries fast and unambiguous.

* **Naturally regulates scalability according to proving capacity and DA bandwidth**, without relying on external coordination or delayed finality.

* **Guarantees safe Ether accounting across rollups, even in custom STFs**, through direct L1 verification.

While similar safety properties *can* be achieved by idealized rollups—including optimistic designs—this model provides them **by construction and without additional protocol layers**. The tight coupling between DA publication and proof verification eliminates deferred resolution, challenge windows, and auxiliary assumptions, keeping the protocol simple and making synchronous, atomic cross-rollup execution practical.

Native Rollups publish **DA together with state transition proofs**. This model:

* Removes the need for special prover-incentive mechanisms.  
* Ensures the explicit post-state is available and verified immediately, making state queries fast.  
* Naturally regulates scalability according to proving capacity and DA bandwidth.  
* Guarantees safe Ether accounting across rollups, even in custom STFs.

This tight coupling is what enables synchronous, atomic cross-rollup execution.

## **Real-time proving makes this model practical, even under high throughput.**

Coupling DA with real-time state proofs implies that \*\*only entities capable of producing proofs in real time can include transactions in a block\*\*.

In contrast to many existing rollup designs—where any party that can land a transaction on L1 can force its inclusion—this model requires the block builder or proposer to:

\* Sequence transactions, \*\*and\*\*  
\* Produce a valid state transition proof within the same block.

This means block inclusion is restricted to builders/validators with real-time proving capability.

This is an explicit design tradeoff. It simplifies the protocol, removes delayed verification windows, and avoids complex inclusion or fallback mechanisms, at the cost of raising the technical requirements for block builders.

Given the rapid reduction in proving costs and latency, this tradeoff is considered acceptable and aligned with the long-term trajectory of proving systems. Real-time proving becomes the natural throttle for throughput, ensuring that scalability remains economically and technically bounded.

## **5\. Block Building Process**

The block building process is the core mechanism that allows Native Rollups to achieve synchronous composability, deterministic execution, and real-time verification. Because multiple rollups can interact within the same L1 block, block construction must follow a structured flow that guarantees correctness, ordering, Ether safety, and proof availability.

Below is the high-level process for building a block containing one or more Native Rollup updates.

### **5.1 Transaction Collection and Ordering**

Each Native Rollup collects user transactions independently.  
 For a given L1 block:

1. Rollup proposers submit DA containing their ordered list of transactions.  
2. The ordering of DA submissions across rollups is determined by the L1 block builder.  
3. This ordering becomes the canonical cross-rollup execution order for synchronous calls.

This ensures that synchronous calls across rollups have a globally deterministic execution context.

### **5.2 Execution of Transactions and Partial Transactions**

For each DA batch included in the block:

1. The rollup executes its transactions according to its STF (EVM or custom).  
2. If a transaction triggers a cross-rollup synchronous call, the system:  
   * Creates a partial transaction in the target rollup.  
   * Executes that partial transaction immediately.  
   * Returns a second partial transaction back to the caller rollup.

All partial transactions across all involved rollups must complete before the block can be finalized.

The entire multi-rollup execution forms a single atomic dependency graph, which must be fully resolved within the block.

### **5.3 State Root Calculation**

After all execution steps (including partial transactions) finish:

* Each rollup computes its new state root.  
* These roots are deterministic and uniquely defined by the DA \+ ordered execution.

The L1 builder aggregates all updated rollup state roots to form the block’s global rollup state snapshot.

### **5.4 Real-Time Proving**

Each Native Rollup must attach a state transition proof corresponding to the post-state:

* Proofs must be generated fast enough to accompany the DA in the same L1 block.  
* This ensures that only proven state updates enter the global rollup state.

Main responsibility of the proof generation is from block proposers. They can delegate this proof to specialized services, like the block builders.

Real-time proving guarantees:

* No delayed verification window  
* No trusted fallback mechanisms  
* No separate prover-incentive economics

The block builder only accepts updates that include a valid proof.

### **5.5 Ether Accounting and Safety Checks**

During block construction, the system verifies that:

* Ether transferred across rollups matches the rules established in synchronous calls.  
* No rollup can send more Ether than it previously received.  
* Custom STFs respect the same invariant.

This check is performed before accepting the final state roots into the block.

### **5.6 Block Assembly and Finalization**

Once all rollups have:

* Submitted DA  
* Provided valid proofs  
* Completed synchronous interactions  
* Updated their state roots

…the L1 block builder assembles the final block:

1. DA blobs for all rollups  
2. Proofs for all state transitions  
3. New state roots  
4. Cross-rollup-event tree updates forming part of the Global Exit Root

The block is then finalized, and all rollups share a consistent, verified, cross-rollup execution state.

### **5.7 Visual Diagram**

**![][image1]**  
---

## **6\. Cross-Rollup-Events and Global Event Tree**

### **6.1 Cross-rollup-events**

Both native and non-native rollups can emit cross-rollup-events.

* **Native rollups** emit cross-rollup-events as execution progresses.  
* **Non-native rollups** emit cross-rollup-events when a new state (with proof) is submitted to L1.

Every cross-rollup-event includes two pieces of metadata that are guaranteed to be correct:

1. **Rollup Origin** — the rollup in which the cross-rollup-event was produced.  
2. **Emitter Address** — the contract or account that emitted the cross-rollup-event.

Because these values are enforced by the rollup’s consensuated state transition function, **they cannot be forged or spoofed** by any other rollup.

### **6.2 Tree of Append-Only Cross-rollup-events Trees**

A global structure is maintained:

* Each rollup maintains an append-only event tree.  
* Every cross-rollup-event leaf includes the rollup origin and emitter address, authenticated by the rollup’s STF and proof.  
* These per-rollup trees are recursively aggregated into a **tree of trees**.  
* Historical trees are preserved, forming an immutable cross-rollup-event history.

### **6.3 Global Exit Root**

The root of this aggregated structure is the Global Exit Root:

* Accessible to all rollups.  
* Acts as a shared source of truth for cross-rollup events.  
* Enables verification of cross-rollup-events inclusion without trusted intermediaries.  
* Allows any rollup to verify not only that a cross-rollup-event exists, but which rollup produced it and which address emitted it, with full cryptographic guarantees.

---

## **7\. Inter-Rollup Messaging**

### **7.1 Native Rollup Messages**

Native rollups generate **InterRollup Messages** (IRMs) during execution:

* Emitted as part of the normal transaction flow.  
* Immediately available to other native rollups synchronously.  
* Also recorded in the global event tree structure for async consumers.

### **7.2 Non-Native Rollup Messages**

Non-native rollups:

* Generate cross-rollup-events only when a new state is submitted with a proof.  
* Rely on the async cross-rollup-event mechanism for interoperability.

---

## **8\. Trustless Bridge Model**

The bridge mechanism is fully **trustless**:

* No multisig.  
* No privileged operators.  
* Anyone can join as a prover, verifier, or relayer.

Security derives solely from:

* L1 Data Availability (DA)  
* Validity proofs  
* Cryptographic inclusion proofs in the Global Exit Root

### **8.1 Token Bridging: ERC20, ERC721, and ERC1155**

For transferring fungible or non-fungible tokens across rollups, a **trustless wrapper-based bridge** can be built on top of this framework:

* A generic wrapper can support standard ERC20/ERC721/ERC1155 assets.  
* Alternatively, token issuers may define **customized wrappers** with specific policies.  
  * This is particularly relevant for **centralized tokens**, such as most stablecoins, where the issuer must retain full control across all rollups.  
  * These wrappers allow issuers to preserve compliance, freeze/mint policies, or multi-domain supply tracking.

### **8.2 Conservation of Tokens Across Rollups**

The bridge must guarantee that **no rollup—native, non-native, or custom-native—can mint more tokens than it legitimately owns**.

This ensures global asset conservation across the entire multi-rollup system.

This concept corresponds to what Polygon AggLayer calls the pessimistic proof.

The pessimistic proof is a process that takes as input the raw Global Exit Tree and deterministically computes a derived tree containing only valid transfers and the token balances of each rollup. During this computation, it verifies that no rollup ever holds a negative balance, ensuring strict conservation of tokens across the entire system.

The result of this process is then proved in a single pessimistic proof, which can be verified by any rollup. Rollups can use this proof to safely mint or release wrapped tokens, knowing that the global system state respects token conservation and that no tokens have been created improperly in any rollup.

---

## **9\. Custom Native Rollups**

A **custom native rollup** is defined by a single deterministic state transition function (STF).  
 This function takes as input the current state and either:

* a transaction coming from a user, or  
* a call originating from another rollup,

and outputs a new state. As with all native rollups, the state transition must be fully deterministic so that all verifiers compute the same result.

### **9.1 Deterministic Single-TX State Transition**

A custom native rollup must define:

```
state' = STF(state, tx_or_call)
```

The STF is the *only* source of truth for how the rollup evolves.  
 It must never produce ambiguous or nondeterministic outcomes.

### **9.2 Resource-Proportional Execution**

A crucial requirement for custom native rollups is that **execution cost and proving cost must be bounded by the gas provided** by the transaction or the cross-rollup call.

This ensures:

* No rollup can force excessive proving workload on the system.  
* State transitions are predictable and economically safe.  
* Block builders can sequence custom rollups without risk of unbounded computation.

### **9.3 Enforcing Limits Through RISC-V ELF Execution**

One practical way to guarantee proportional resource consumption is to define the STF as:

* A **RISC-V ELF program**,  
* Executed inside a VM that halts deterministically with an error once its resource budget (cycles, memory, etc.) is exceeded.

In this model:

* The transaction provides a gas budget.  
* The RISC-V VM interprets the STF under strict metering.  
* If execution exceeds the allowed resources → it terminates with an error.  
* This behavior is deterministic and easy to prove in a zk system.

This enables flexible, application-specific rollups while preserving the guarantees required for synchronous composability, deterministic execution, and safe cross-rollup interaction.

---

## **10\. Mainnet as a Rollup**

Ethereum mainnet itself fits naturally into this model:

* In the future, mainnet can behave as a **native rollup**.  
* Before the necessary hard forks, it behaves as a **non-native rollup**.

This provides a smooth migration path without breaking composability assumptions.

---

## **11\. Summary**

This proposal introduces a unifying framework that enables Ethereum to scale through a diverse ecosystem of interconnected rollups while preserving strong security and composability guarantees.

**Native Rollups** anchor their execution to Ethereum L1 by publishing transaction data together with state proofs. This allows:

* **Synchronous composability** — cross-rollup calls execute atomically within the same block.  
* **Deterministic state** — every rollup’s state is fully defined immediately after DA publication.  
* **Secure Ether accounting** — no rollup can spend more Ether than it has received.  
* **Efficient real-time proving** — proving cost and throughput scale with hardware and bandwidth.

**Block building** becomes the coordination layer that orders rollups, sequences partial transactions, and ensures the correct global execution graph. This enables high throughput with minimal latency for cross-rollup interactions.

A **Global Exit Root** provides a shared messaging substrate for all rollups, enabling trustless asynchronous communication. Non-native rollups rely on this system for interoperability and can progressively migrate toward synchronous execution over time.

The **trustless bridge model** ensures global conservation of assets without multisigs or trusted operators. Wrapped tokens and pessimistic proofs guarantee that no rollup can mint or release assets improperly.

Finally, **custom native rollups** allow developers to define their own deterministic state transition functions—EVM-based or bespoke—while preserving safety through resource-proportional execution enforced by RISC-V ELF metering. These rollups benefit from the same synchronous composability as standard native rollups.

Overall, this architecture allows Ethereum to evolve into a highly scalable, modular, and interoperable system where many rollups behave like a single coherent compute environment, without sacrificing decentralization or trustlessness.

**12\. Interesting links & related work**

**Taiko — Gwyneth (Similar approach)**

[https://taiko.xyz/gwyneth](https://taiko.xyz/gwyneth)

**Synchronous Composability Protocol (SCOPE)**  
SCOPE is a protocol designed to enable synchronous composability between chains without requiring a shared sequencer. Instead, when initiating a cross-rollup transaction, the initiating chain must wait for a response from the destination chain’s sequencer and trust that response.  
[https://ethresear.ch/t/scope-synchronous-composability-protocol-for-ethereum/22978](https://ethresear.ch/t/scope-synchronous-composability-protocol-for-ethereum/22978)

**Interoperable Addresses (ERC/EIP-7930)**  
Ongoing discussion around interoperable addresses that can be used consistently across multiple chains and rollup.

* EIP: [https://eips.ethereum.org/EIPS/eip-7930](https://eips.ethereum.org/EIPS/eip-7930)  
* Discussion: [https://ethereum-magicians.org/t/erc-7930-interoperable-addresses/23365](https://ethereum-magicians.org/t/erc-7930-interoperable-addresses/23365)

**Interop Working Group Notes**

[https://notes.ethereum.org/@rudolf/interop-all-notes/](https://notes.ethereum.org/@rudolf/interop-all-notes/)

**Native Rollups (Definition)**  
[https://ethresear.ch/t/native-rollups-superpowers-from-l1-execution/21517](https://ethresear.ch/t/native-rollups-superpowers-from-l1-execution/21517)

**Based Rollups (Definition)**  
[https://ethresear.ch/t/based-rollups-superpowers-from-l1-sequencing/15016](https://ethresear.ch/t/based-rollups-superpowers-from-l1-sequencing/15016)

[image1]: <data:image/png;base64,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P3UWL14c2AYAAACIlkqxREt3ky+++MI9hKiQm7SGUVTRkrvICnLnWLMNAAAAvkc25uaNQssxxRQtdWsH2bs1ePBgX1sYRROtSy+91HdZpOScc85xpk+fnnGBAQAAyg3ZPsp9J5944omyj5yAXgxkD5bpLVIbNmxYRm8pmmiFIaZ62WWXmWUAAICyJNNGHJJBv379zJJH4kQLAAAAqkCy0g+iBQAAkECQrNIA0QIAAEgY8mPFUBoURbTmzJnjPmLrAAAAftg2Jo93333XfezSpYvRkp2iiJbw3HPPWWfvqwAAAJQb7733nnP55ZebZUgII0aMsHwlircUTbSEiRMn+saXLFniGwcAACgX+vTpY5YgYcitHHTEW/773//6aiZFFa2rr77aLAEAAJQV+m/oQbLJ5XMqqmjJAmczQQAAgFJFbni5adMmswwJ5Msvv3Q/L7k7f3UommitXLnSOsYpd4sHAAAoB5566il3411afGsWSobRo0db3nLFFVeY3SyKJloAAADlSi6HoCCdFE20HnvsMc8I+/fv72zcuNFZs2aN2Q0AAKCkkMNPkD4GDhzoecuAAQPcE+HlStFsFE20ZEG//fZb91EuZ1ULDwAAUKrIjUiL9aPIUDOUt1x00UXOhRdeGNlbiiJa69atc+699153WC3k+++/72zdulXvBgAARad0z7mJm7POOsssQYoYM2aM+9ijRw+vtmLFCm84jKKIlti8Eiz1KJe3yuFEAACAUiPKng9INmeffbb7qN+a6oUXXvCGwyiKaAmyB0u44IILIu9+AwAASBuDBw82S5BCZs2a5T5+9tln1fKWoohW9+7dfePLly/3jQMAAKSdL774wunYsaNZhhQyadIk3/jSpUudDRs2+GphxCpanTt3dk4//fRACwyqAQAApJFly5Y55513nlmGlCHnjou3yEUMJlG9JVbRUojhz58/3/0jVLvf7rjjDrMbAABA6nj22Wedr7/+2ixDihFPmTNnju8uCYkWLQAAgFLk2muvdaZMmWKWoYyJTbTUPbN0AzTHAQAA0oocYoLSQvcU+W3mXLwlNtGSBfvoo49848Ls2bOrvdDlgr570kxQv3xRnXk9+OCD1rLlujz6NDKcaaUld+M1+wMAFAtZB/3nP/8xy5Bi5KakN954ozcun7HcDV7tOIpKrKKluO+++9hIRiBIWPQf41aY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>