May 12, 2026

## EEZ roadmap & updates

Invited [Armagan Ercan](mailto:armagan@gnosis.io) [Sebastian Bürgel](mailto:sebastian.buergel@gnosis.io) [Philippe Schommers](mailto:philippe@gnosis.io) [Friederike Ernst](mailto:ernst@gnosis.io) [Ben Carvill](mailto:ben.carvill@gnosis.io)

Attachments [EEZ roadmap & updates](https://calendar.google.com/calendar/event?eid=NGdoaDZrZ3E3ZzhldXA2b29pN2V2bGwzOWggZXJuc3RAZ25vc2lzLmlv)

Meeting records [Transcript](https://docs.google.com/document/d/1efNQMuiJSjRxByJBOmsvrkRq_ih8wZfZRenK_8hhsLw/edit?usp=drive_web&tab=t.mrnko8wsc66b) [Recording](https://drive.google.com/file/d/1-RErauHC7TW-CY0ji1rC_GFcos6h6VR7/view?usp=drive_web) 

### Summary

The team established a phased development timeline for EEZ smart contracts and the Nosis Layer 2 launch.

**Development and Rollup Timeline**  
The team finalized plans to deploy EEZ smart contracts by August, followed by an initial Rollup One test instance. This phased approach prioritizes early utility over full immediate composability.

**Technical Architecture and Sequencers**  
The architecture utilizes a centralized sequencer for rapid block times while shifting trust to a validator committee for Layer 1 data posting. The system enables synchronous bridging from mainnet.

**Governance and Monetization Strategy**  
The team confirmed a shift in economic models for GNO holders, prioritizing user experience and liquidity integrations. They will publicly address censorship trade-offs inherent in the centralized sequencer design.

*Rate this Summary:* [Helpful](https://google.qualtrics.com/jfe/form/SV_4YkxrBAaiTVqYCi?isGoogler=false&isHelpful=true) or [Not Helpful](https://google.qualtrics.com/jfe/form/SV_4YkxrBAaiTVqYCi?isGoogler=false&isHelpful=false)

### Decisions

We've **updated the Decisions section** using your feedback.

Let us know what you think: [Helpful](https://google.qualtrics.com/jfe/form/SV_5p6FWBVWvynleNU?isGoogler=no&isHelpful=yes) or [Not Helpful](https://google.qualtrics.com/jfe/form/SV_5p6FWBVWvynleNU?isGoogler=no&isHelpful=no)

## NEEDS FURTHER DISCUSSION

* **Forced inbox inclusion under negotiation** The team deferred the final decision on the implementation and timeline of a forced inbox for censorship resistance, pending further negotiation.

## ALIGNED

* **Modular prover architecture setup adopted** The team decided to adopt a modular prover architecture including Sisk, Succinct SP, and Trusted Execution Environment (TEE) servers, with a requirement to use at least two provers.

* **Centralized sequencer strategy chosen** The team decided to discontinue the plan to migrate the existing validator set and will instead implement an in-house centralized sequencer.

* **Nosis L2 replaces Nosis chain** The team established that the upcoming Nosis L2 deployment will serve as a total replacement for the current Nosis Chain.

* **Gas token remains unchanged** The team decided to maintain the current gas token without changes for the new system.

### Next steps

- [ ] \[Friederike Ernst\] Write GIP Draft: Draft content outlining the economic model and value capture perspective of the rollup for the GIP proposal.

- [ ] \[Philippe Schommers\] Scope Inbox Feature: Determine complexity for implementing the forced inclusion inbox functionality; necessary for the December 31st target.

### Details

* **EEZ Smart Core Smart Contracts and Timeline**: Friederike Ernst detailed the plan to complete the EEZ smart core smart contracts by the end of the current month, approximately two weeks from the meeting date. Following completion, the contracts will undergo four weeks of external review, succeeded by a budgeted four weeks for an external audit, placing the core contracts' deployment to the test net and Ethereum mainnet in August ([00:00:00](?tab=t.mrnko8wsc66b#heading=h.dykjmdvjkqxr)). The timeline for Nosis chain is to implement an early version as soon as possible to provide tangible value to users and dApps, even if it is not the final form of Nosis in the EEZ ([00:02:48](?tab=t.mrnko8wsc66b#heading=h.k8uhg3i0frwu)).

* **Prover Setup and Requirements**: The team has opted for a modular prover setup, which will include several available zero-knowledge (ZK) provers such as sisk and the succinct SP prover. Users are allowed to add their own ZK provers, and the system will also incorporate Trusted Execution Environment (TEE) servers ([00:01:20](?tab=t.mrnko8wsc66b#heading=h.ceqki1fyzblk)). Chains within the Ethereum Economic Zone must specify and utilize at least two proving systems ([00:02:48](?tab=t.mrnko8wsc66b#heading=h.k8uhg3i0frwu)).

* **Nosis Chain Early Implementation and Benefits**: The simplest version for Nosis chain involves allowing a single call from the Ethereum mainnet onto Nosis chain with a return value, which enables shared liquidity between mainnet and Nosis chain ([00:02:48](?tab=t.mrnko8wsc66b#heading=h.k8uhg3i0frwu)). This one-way call, initiated only from the mainnet, is expected to confer about 80% of the benefits the EEZ could enable today, though down the line there will be many more advantages ([00:04:25](?tab=t.mrnko8wsc66b#heading=h.y9h9kpp79f3d)). The plan is to upgrade Nosis chain to a limited, synchronously composable first version by December 1st, using a limited prover setup ([00:05:34](?tab=t.mrnko8wsc66b#heading=h.3o4fnllvp68e)).

* **Stepping Stone Approach to Composability**: Friederike Ernst stressed that the initial, limited implementation is a necessary stepping stone toward full synchronous composability ([00:05:34](?tab=t.mrnko8wsc66b#heading=h.3o4fnllvp68e)). Building out the entire system before launching it all at once could take approximately two years, making an earlier, useful release important for users. Philippe Schommers agreed with the high-level representation and found the plan fairly realistic ([00:06:59](?tab=t.mrnko8wsc66b#heading=h.ksy9312z63td)).

* **Block Times and Centralized Sequencer**: The team is not currently in favor of taking the validator set with them due to the security trade-offs required for shorter block times, which would still only achieve approximately four-second blocks. The current idea is to run a centralized in-house sequencer that creates blocks every second, potentially with a slightly longer latency around the composing block ([00:08:11](?tab=t.mrnko8wsc66b#heading=h.ub9vg4s8znjk)). Ben Carvill raised questions about the complexities this introduces for decentralized finance (DeFi), such as missing updates when the Nosis instance's block time of four seconds is paired with updates every 12 seconds from the Ethereum mainnet ([00:09:24](?tab=t.mrnko8wsc66b#heading=h.g6mjrey85rnq)).

* **Addressing Block Time Discrepancies**: Friederike Ernst noted that complexities due to different block times are already present, explaining that the current canonical bridge is much worse, with bridging taking 20 minutes ([00:09:24](?tab=t.mrnko8wsc66b#heading=h.g6mjrey85rnq)). The first instance of the rollup, Rollup 0.2 or 0.3, will be deployed in August or September, while Nosis L2 is scheduled for the end of the year ([00:10:33](?tab=t.mrnko8wsc66b#heading=h.n9z97pc6frun)).

* **Rollup One Implementation and Bundles**: Sebastian Bürgel sought clarification on the August deployment, which Friederike Ernst clarified would be an instance of Rollup One and not the Nosis L2 ([00:10:33](?tab=t.mrnko8wsc66b#heading=h.n9z97pc6frun)). Philippe Schommers explained that the team will utilize bundles from B-math, sending them to builders like Titan Beaver, who must put the entire bundle into one block in that specific order. This method allows the team to compose blocks for single calls and posts to Layer 1 ([00:11:28](?tab=t.mrnko8wsc66b#heading=h.6q4o01ie9f1v)).

* **Rollup One as a Test Environment**: The Rollup One deployment will be a non-production test net labeled as such, although Sebastian Bürgel observed that it will involve real money. The intent is to test composer issues and state proofing capabilities, and Philippe Schommers confirmed that they will attempt to limit the amount of money that can move through the EEZ on Rollup Zero ([00:13:33](?tab=t.mrnko8wsc66b#heading=h.fixq0ydi7tzx)).

* **Nosis L2 Implementation at Year-End**: Sebastian Bürgel established December 31st as the target date for the Nosis L2 launch. Friederike Ernst confirmed that the Nosis L2 is intended as a replacement for Nosis chain, which means the way blocks are built will be switched out from underneath the existing chain ([00:15:10](?tab=t.mrnko8wsc66b#heading=h.n5dwoah3vrfe)). Clients will be updated to support the transition, although users might need to switch to clients built on stacks more easily verifiable by the ZK program, such as Rust or C\# ([00:16:26](?tab=t.mrnko8wsc66b#heading=h.5e8a6c5sxinz)).

* **Centralized Sequencer and Trust Assumptions**: Sebastian Bürgel inquired about the trust assumptions of the new block-building system, which uses a centralized sequencer ([00:18:26](?tab=t.mrnko8wsc66b#heading=h.nifapzz8iln2)). Philippe Schommers clarified that users trust the sequencer for pure Layer 2 (L2) blocks, but once the data is posted to Layer 1 (L1), trust shifts to a committee, which is composed of the current bridge validators. The sequencer's role is to provide pre-confirmation of the kill chain, while a smaller committee of validators will attest to and post the sync slot to the L2 ([00:19:34](?tab=t.mrnko8wsc66b#heading=h.15djy4j4ja5r)).

* **Synchronous Bridging Limitations in the First Version**: In December, bridging will initially be limited to L1 to Nosis, meaning from mainnet to Nosis. Philippe Schommers clarified that users can still bridge out, but it will likely be asynchronous or require triggering from L1 to be synchronous ([00:20:39](?tab=t.mrnko8wsc66b#heading=h.j56l2njr1pqn)). Ben Carvill recognized that this allows for accessing L1 liquidity, and Philippe Schommers confirmed that users could execute a flash loan on L1, bridge it to L2 to liquidate an asset, and then immediately bridge it back to L1, all within one L1 transaction ([00:21:34](?tab=t.mrnko8wsc66b#heading=h.k9cano1lp8ah)).

* **User Experience and Abstraction of Cross-Chain Swaps**: Users will not need to be aware of the underlying transaction mechanics, as the process can be abstracted away. Sebastian Bürgel argued that dedicated dApps, such as a wrapped Uniswap interface, would be necessary ([00:22:28](?tab=t.mrnko8wsc66b#heading=h.idk9r9t3xa0n)). Friederike Ernst suggested that intent-based architectures like CowSwap could use a solver to understand when to initialize a transaction from the mainnet to tap into L1 liquidity ([00:23:18](?tab=t.mrnko8wsc66b#heading=h.nhj2rygzr7ii)).

* **BD Activity and Money Market Integration**: Having a money market that understands the EEZ logic and taps into it is a high-priority Business Development (BD) task ([00:23:18](?tab=t.mrnko8wsc66b#heading=h.nhj2rygzr7ii)). CowSwap is set to be a launch partner for the EEZ, and their solvers will be asked to integrate this ahead of time ([00:24:29](?tab=t.mrnko8wsc66b#heading=h.2ttljrimuz3v)). The BD team's primary ask is to ensure an EEZ-aware and positive money market is active, potentially either taking over the existing Aave deployment or ensuring a new money market, like Mark's new money market, is ready ([00:25:08](?tab=t.mrnko8wsc66b#heading=h.7b2oqim1m3x1)) ([00:28:27](?tab=t.mrnko8wsc66b#heading=h.e435i0i41apj)).

* **Economic Impact of the Nosis L2 and Revenue Capture**: The Rollup One deployment is expected to be a reference implementation that will not capture significant upside. Nosis chain can capture revenue in several ways, primarily by being an early and significant network in the EEZ, which provides a narrative and first-mover advantage ([00:30:52](?tab=t.mrnko8wsc66b#heading=h.9o5uvhb3i683)). The goal is to push Nosis chain as the default ecosystem for deployment for those wanting Ethereum security without the fees ([00:32:08](?tab=t.mrnko8wsc66b#heading=h.sz1bm2pgyb1b)).

* **Monetization Strategies Beyond Transaction Volume**: Should the first-mover advantage not materialize, Nosis has several other monetization avenues. The network could become an aggregator for other L2s moving into the EEZ by bundling proving costs. They could also become a cross-chain block builder or a highly advanced searcher, leveraging the lucrative nature of block building ([00:33:45](?tab=t.mrnko8wsc66b#heading=h.svdex7tz9bsl)).

* **GNO Token Economics and Staking Changes**: Nosis chain currently generates low revenue, approximately 100,000 XDAI, with GNO staker rewards coming from the DAO treasury, making the chain a loss-maker ([00:37:43](?tab=t.mrnko8wsc66b#heading=h.uq6vwcv6jxjk)). The economic model will shift, and while staking is expected to go away, transaction volumes are projected to increase due to an improved user and developer experience. Increased transaction fees could then be routed to GNO holders via the Nosis DAO ([00:39:46](?tab=t.mrnko8wsc66b#heading=h.hrk9dvwu10a)).

* **Protocol Governance and Censorship**: Ben Carvill suggested that retiring the validator set might allow the DAO to introduce protocol-level governance and set standards for the rollup environment ([00:51:00](?tab=t.mrnko8wsc66b#heading=h.4oh207du4jy8)). Friederike Ernst believes that down the line this will be possible, but for the mid-to-near term, they will need to act as "benevolent dictators" ([00:52:14](?tab=t.mrnko8wsc66b#heading=h.95sgtvs04nkj)). The team is considering an explicit censorship protocol for Nosis chain that would allow smart contract developers to specify business logic to prevent transactions that go against the contract's intended behavior, such as unauthorized token minting ([00:53:12](?tab=t.mrnko8wsc66b#heading=h.tlvxph3kuuyi)).

* **Censorship Resistance and UX Trade-Offs**: Sebastian Bürgel raised concerns that the centralized sequencer and the lack of a forced inbox on launch day make the initial L2 strictly worse than the current Nosis chain regarding censorship resistance ([00:43:21](?tab=t.mrnko8wsc66b#heading=h.9bebbxpoopo0)). Friederike Ernst acknowledged the concerns but argued that forgoing full censorship resistance in the first version is worthwhile if it significantly improves the user experience, particularly concerning liquidity. Philippe Schommers agreed that most users do not care about credible neutrality and will prioritize faster bridging and better user experience ([00:45:48](?tab=t.mrnko8wsc66b#heading=h.qfmwv3m9je6k)) ([00:49:22](?tab=t.mrnko8wsc66b#heading=h.dejjhpfkee6g)).

* **Gas Token and Protocol Improvements**: Friederike Ernst confirmed that the gas token will not change ([00:51:00](?tab=t.mrnko8wsc66b#heading=h.4oh207du4jy8)). The team acknowledged that for the Nosis L2 to succeed, the BD team must deliver key integrations (like Aave or a new money market) and the promised liquidity benefits upon launch ([00:49:22](?tab=t.mrnko8wsc66b#heading=h.dejjhpfkee6g)). They also confirmed they would explicitly discuss the potential censorship protocol publicly, rather than "sneaking it in" ([00:55:41](?tab=t.mrnko8wsc66b#heading=h.j5pxcggvdwt)).

*You should review Gemini's notes to make sure they're accurate. [Get tips and learn how Gemini takes notes](https://support.google.com/meet/answer/14754931)*

*How is the quality of **these specific notes?** [Take a short survey](https://google.qualtrics.com/jfe/form/SV_9vK3UZEaIQKKE7A?confid=oHqKAkcbhPwzsNWqx0_qDxIOOAIIigIgABgDCA&detailid=standard&screenshot=false) to let us know your feedback, including how helpful the notes were for your needs.*