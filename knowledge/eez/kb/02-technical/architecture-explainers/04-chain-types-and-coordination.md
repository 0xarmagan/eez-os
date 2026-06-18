# Chain Types and Cross-Chain Coordination

*Explainer 4 of 7. Source: `knowledge/eez/sources/dappcon-2026-eez-node-architecture.md` (DAPPCon EEZ Workshop, 17 June 2026, Jordi Baylina). Engineering-level founding material. Quote as Jordi's framing, not as approved EEZ comms.*

The Ethereum Economic Zone is not one chain. It is an economic zone built on Ethereum, and it lets many rollups behave as one synchronous system. Different rollups are built in different ways. EEZ has to account for that. The DAPPCon deck names three chain types, and for each it sets out how that chain joins the zone and what it owes the rest of the system.

This explainer walks through the three types. It then covers the two methods for coordinating cross-chain inclusion, optimistic and pessimistic. It closes with the binding versus non-binding sequencer distinction from the node-architecture diagram, and how that ties back to coordination.

One framing note before the detail. EEZ is not deployed yet. The roadmap (deck slide 55) runs through smart-contract cleanup, an audit, the proving system, Composer 1.0, Chain Zero, and then connecting Gnosis Chain. So treat every "can a chain join" question as a roadmap question, not a "today" one. The mechanics below describe the design, not a live network.

## Chain type 1: custom native rollups

A custom native rollup brings its own state transition function. The deck is explicit that this function can run in WASM, in RISC-V, or in a custom format. The rollup defines its own rules and its own accepted proving systems. EEZ does not impose one execution model. Rollups are sovereign, governed by a rollup smart contract.

Cross-rollup interaction here is a normal Ethereum CALL and RETURN between smart contracts on different chains. A contract on chain 1 calls a contract on chain 2, and the result returns. This is not message passing and not a bridge. On L1, the participating contracts exist as proxies of their real counterparts on the rollup. The proxies are synchronous and they share state. That is the concrete form of "proxies, not bridges".

Inside a native rollup, the operations are execution entries, not transactions. Keep that distinction. "Transaction" is the right word for L1, and for partner chains that run their own transaction model, but not for the work happening inside an EEZ native rollup.

Native rollups that use ETH as their native asset can include ETH directly in CALLs. Moving ETH across rollups does not use a bridge. It uses rollup-level ETH accounting maintained in L1. The L1 layer keeps the books so the zone stays consistent.

Creation is permissionless. Anyone can create a native rollup and opt it in, and a rollup can opt out again. No committee gates entry.

On finality, native rollups follow the native path, which targets roughly 12 seconds. That is distinct from the async path, which targets roughly 20 minutes. Name the path whenever you cite a figure. There is no single finality number for EEZ as a whole.

## Chain type 2: based and centralized sequencer rollups

The second type covers based rollups and centralized sequencer rollups. The deck treats any centralized rollup as a base rollup whose state transitions are all signed by the sequencer. The sequencer is the authority over what the chain includes and in what order.

This creates a coordination requirement. The L2 sequencer must coordinate with the composer to include cross-chain transactions. The composer is the component that builds and sends the batch to L1, and anyone can run one. For a self-contained chain the sequencer can act alone. The moment a transaction needs to touch another chain, the sequencer and the composer have to agree on inclusion, because the cross-chain effect has to land atomically with the rest of the zone.

The deck gives two methods for that coordination, optimistic and pessimistic. They are covered in their own section below, because they apply across chain types, not only to this one.

A practical note on terms. For these partner chains, "transaction" is acceptable, scoped to that chain's own model. The "execution entries" rule applies to EEZ native rollups, not to a based or centralized chain that already speaks in transactions.

## Chain type 3: validiums

A validium keeps its data off-chain rather than posting it all to blobs. The deck states that a validium may omit self-chain-only transactions from blobs. If a transaction only affects the validium itself, the validium does not have to publish it for the rest of the zone to function.

The condition is cross-chain reach. If a validium's transactions are to touch other chains, it must provide that information, or a coordination mechanism with composers, for the cross-chain part. The composer cannot reason about a cross-chain effect it cannot see. So the validium has to expose enough, either the data itself or an agreed mechanism, for the cross-chain interaction to be included and proven. Self-contained work can stay private to the validium. Anything that reaches out has to be visible to the coordination layer.

This keeps the validium's data-availability savings for its own internal activity, while still letting it join cross-chain flows when it chooses to. As with native rollups, joining is the chain's choice, and EEZ does not force a single data model on every participant.

## Coordination methods: optimistic and pessimistic

Cross-chain inclusion needs a way to keep the participating chains consistent while a cross-chain interaction is being assembled and proven. The deck names two methods. They sit on a liveness-versus-safety trade-off, and neither is universally better.

### Optimistic: reorgs

The optimistic method proceeds on the assumption that the cross-chain transaction will be included as planned, and it relies on reorgs to clean up if that assumption breaks. The chain keeps producing blocks. If the cross-chain interaction ends up not landing the way it was assumed, the affected portion is reorged out and rebuilt.

The benefit is liveness and lower latency. The chain does not stall waiting for confirmation from elsewhere. It keeps moving. The cost is that recently produced blocks are not safe until the cross-chain assumption settles. A reorg can undo work. This method fits chains and applications that value continuous progress and can tolerate that recent blocks may be revised. It is the looser, faster option.

### Pessimistic: locking

The pessimistic method locks before it proceeds. It does not assume inclusion will succeed. It secures the relevant state so that the cross-chain interaction either completes or fails cleanly, without a reorg. The deck adds an important qualifier. The lock is not necessarily the full chain. The system can lock only the part that the cross-chain interaction touches, rather than freezing everything.

The benefit is safety. There is no reorg risk on the locked state, because nothing conflicting can be produced while the lock holds. The cost is liveness and latency. Locked state cannot make independent progress until the lock clears, and acquiring the lock adds delay. This method fits high-value or hard-to-reverse interactions where a reorg would be unacceptable. It is the stricter, slower option, made more usable by the fact that the lock can be scoped to only the affected state.

### Choosing between them

The choice is the classic one. Optimistic favours liveness and latency and accepts reorg risk. Pessimistic favours safety and accepts added latency and reduced liveness on the locked state. A chain that prizes uninterrupted block production leans optimistic. A chain handling interactions that must never be reversed leans pessimistic. Because locking can be partial, a chain can also mix the two, using pessimistic locking only on the sensitive paths and staying optimistic elsewhere. The deck presents both as intended methods, so this is a design choice for the participating chain, not a single mandated rule.

## Binding versus non-binding sequencers

The node-architecture diagram (Jordi's hand-drawn topology) shows two deployment modes for how a sequencer relates to a composer. The distinction matters because it sets how strong the cross-chain guarantee is.

In non-binding mode, the diagram shows a composer with a simulator and a "sequencer not binding", stacked across several rollups. The composer simulates and proposes, but the sequencer has not committed to the exact cross-chain ordering in advance. The expert review in the source is direct about the effect. Non-binding degrades same-block atomicity to "same-batch, eventually". The cross-chain interaction still resolves, but not with the tight same-block guarantee.

In binding mode, the diagram shows separate composer instances, each paired with a "sequencer binding", feeding sequenced chains. Here the sequencer commits to the composer's cross-chain inclusion. That commitment is what supports the stronger, same-block atomic behaviour, because the sequencer is bound to the ordering the composer assembled.

This connects straight back to the coordination methods and to chain type 2. A binding sequencer commits up front, which pairs naturally with the pessimistic, lock-and-commit style where the goal is no reorgs and a firm guarantee. A non-binding sequencer keeps producing and lets the composer simulate and propose, which pairs with the optimistic style where reorgs are the cleanup tool and liveness comes first. The based and centralized sequencer rollup type is exactly where this shows up, because that is where the L2 sequencer must coordinate with the composer to include cross-chain transactions in the first place.

There is an honest cost to name. Binding mode asks the sequencer for a commitment, and the deck states that fee incentives for composers are not yet defined. The source notes that rational operators may default to non-binding or optimistic in quiet periods, when cross-chain revenue is thin. So the binding, pessimistic end of the design buys stronger guarantees, and the system still has to make running it worthwhile. That economic question is open, and it is roadmap work, not a settled answer.

## Accuracy notes

- EEZ is not deployed yet. The roadmap ends with Composer 1.0, Chain Zero, and connecting Gnosis Chain (deck slide 55). Frame any participation question as a roadmap question, not a "today" one.
- Finality has two paths. The native path targets roughly 12 seconds and the async path targets roughly 20 minutes. Always name the path beside the figure. No single number describes EEZ as a whole.
- EEZ is an economic zone built on Ethereum, not "an L2". Native rollups are an L2 construction that EEZ builds on top of.
- Cross-chain interaction uses proxies, not bridges. Proxies are synchronous and share state. Cross-rollup ETH movement uses rollup-level ETH accounting in L1, not a bridge.
- Inside EEZ native rollups, the operations are execution entries, not transactions. "Transaction" is acceptable for L1 and for partner chains (based, centralized sequencer, validium) that run their own transaction model, scoped to them.
- EEZ enforces a minimum of two proving systems per rollup in-contract. The single `prover` box in the node-architecture diagram is a topology abstraction, not a single-prover claim. Do not use singular proving framing.
- The two coordination methods (optimistic = reorgs, pessimistic = locking) and the binding versus non-binding distinction are Jordi's engineering-level framing from the DAPPCon workshop, not approved EEZ comms.
- Composer fee incentives are undefined in the deck. The non-binding-default risk in quiet periods is an open design area, flagged as roadmap work.
