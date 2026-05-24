Jordi Baylina’s Talk at ETH Denver

Cleaned by Gemini

---

## **Jordi Baylina’s Talk at ETH Denver: A ZKVM Foundation for Next-Generation Apps**

### **Introduction to Ziszk**

Welcome, everyone. I’m Jordi Baylina from **Ziszk**. To give you a quick overview, Ziszk is a 64-bit ZKVM based on RISC-V. It is open-source, features 128-bit proven security, and is quantum-resistant. We’ve been building this for the last two years.

Today, I want to discuss my vision for how future blockchains will look. While I don’t have a crystal ball, I believe the core shift lies in the **separation of data and execution.**

### **Data vs. Execution**

In this vision, blockchains ultimately become layers for **Data Availability (DA)**—essentially just blobs of data. When you execute this data deterministically, you arrive at a specific state.

A good example is a standard blockchain like Ethereum. If you query every transaction from the genesis block and execute them, you get a unique state. The power of Zero-Knowledge (ZK) technology is that you can perform this query, reach that state, and provide a proof that the state is correct. When a new block arrives, you use the previous state, the new data in the blobs, and generate the newest state. This is a streamlined way of maintaining a blockchain.

### **Static vs. Dynamic Consensus**

This concept extends to what we call **Dynamic** and **Static Consensus**:

* **Dynamic Consensus:** The consensus reached on every individual block (the data appearing in the chain).  
* **Static Consensus:** The protocol itself—the rules for how data is processed.

Normally, changing how transactions are processed requires a hard fork. However, if you define a new static consensus, you can launch a new "blockchain" simply by putting data on the main chain. You don’t need to fork the underlying layer.

### **Synchronous Composability**

This model enables **synchronous composability** between rollups (L2s) or even between a rollup and L1. Synchronous composability means that if you are executing a smart contract on one rollup, you can call a contract on a completely different chain, receive a return value, and complete the entire sequence atomically in a single transaction.

How does this work without forking Ethereum? We use **Proxy Contracts**.

If a DAO contract on an L2 needs to be accessed from L1, we create a "counterfactual" proxy contract on L1. Its address is a deterministic function of the L2 address and Chain ID.

When this proxy receives a call:

1. It looks up an **Execution Table**.  
2. It verifies the state transition via a ZK proof.  
3. It emulates the call, updates the state, and returns the result to the user.

To a user, it feels as if all these chains are executing as one. For a block builder, this means they can bundle state transitions for multiple rollups into a single transaction with a single ZK proof.

### **App Chains and Security**

This model is flexible. Rollups don’t have to be EVM-compatible; they could be WASM-based or use custom transition functions. As long as they can accept and emit calls, they can plug into this network.

Crucially, if a specific app-chain changes its governance or transition function, it doesn’t break the rest of the network. Even a malicious network would only affect itself, with the exception of value transfers (Ether), which require L1 smart contracts to maintain accountability to ensure a rollup doesn't "spend" more than it has received.

### **Current Status and Benchmarks**

This entire system is orthogonal to "pre-confirmations" (which can be built on top) and, importantly, **does not require an L1 fork.** We can start building this today.

Currently, Ziszk is achieving **real-time proving.** Using 16 GPUs, we can prove almost all blocks in an average of 9 seconds.

* **Version 0.7:** Releasing next week with a projected 30% performance improvement.  
* **Version 0.8:** Will focus on packaging, SDKs, and documentation.  
* **Future Focus:** We are shifting heavily toward formal verification and security to make the system as robust as possible.

We want the community to use this. If you are interested in ZK, please dive in. Thank you.

