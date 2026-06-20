# EEZ BuilderRoom Workshop — Distribution Kit

**Status: DRAFT** — copy is review-ready; the `[bracketed]` links must be filled in and the whole thing signed off by Armagan before publishing. Nothing here is approved comms yet.

**The asset.** Recording of the EEZ BuilderRoom workshop, 17 June 2026 (DAPPCon Berlin — confirm venue), ~3 hours, with Friederike Ernst, Martin Köppelmann and Jordi Baylina. Rationale → live demo → smart contracts → real-time proving, with extended audience Q&A.

**Companion source:** the full digest and Q&A live at `../02-technical/sources/dappcon-2026-builderroom-workshop-recording.md` (gnosis-marketing: `Dappcon-2026-Sources/EEZ-BuilderRoom-Workshop-Recording-DAPPCon-2026.md`).

**Links to fill in before publishing:**
- [ ] YouTube URL
- [ ] Slides (attach the deck PDFs, or a link)
- [ ] Public home for the written summary (the KB digest is currently repo-internal)
- [ ] EEZ site / docs / forum links

---

## 1. YouTube

**Suggested title:** EEZ BuilderRoom Workshop: Synchronous Composability Across Rollups (DAPPCon Berlin 2026)

**Description:**

A full technical workshop on the Ethereum Economic Zone (EEZ): how many rollups can behave as one synchronous system on Ethereum, with cross-chain calls settling inside a single 12-second slot.

Recorded at the EEZ BuilderRoom, DAPPCon Berlin, 17 June 2026, with Friederike Ernst, Martin Köppelmann and Jordi Baylina. It moves from the rationale into a live demo, then into the smart contracts and the real-time proving system, with extended audience Q&A throughout.

What's covered:
- Why cross-chain composability is broken today, and what "synchronous with Ethereum" actually means
- A live demo: calling an L2 contract from Ethereum through a deterministic proxy
- The composer, the lookup table, and how a cross-chain call is assembled
- Multi-prover settlement, where each rollup chooses its own proof systems (ZK, TEE or multisig)
- Chain types, the EEZ Trace blob format, and the proving pipeline (the cryptographic accumulator)
- Tokens, bridges, and the Gnosis Chain integration
- Current status and roadmap

Note: this is an engineering-level session and reflects work in progress. EEZ is not deployed for production use, and nothing here is financial or technical advice.

Chapters:
```
0:00 Introductions
2:28 Why EEZ: the composability problem
4:52 The idea: synchronous with Ethereum in 12 seconds
7:09 Roadmap: Rollup 0/1 and Gnosis Chain
9:51 Live demo: calling an L2 contract from L1
11:27 Deterministic proxies explained
25:46 Block building and pausing the chain
28:23 Static reads: pushing L1 state onto L2
40:23 Inside the proxy: the fallback forwarder
47:31 Who pays the gas?
50:55 Account abstraction and transient storage
54:01 Nonces and identities across chains
58:59 Following a chain: node software
1:25:18 Multi-prover: sharing proof systems
1:27:11 Registering a rollup
1:36:28 Chain types: optimistic vs pessimistic
1:38:21 Validiums and non-EVM chains
1:43:09 EEZ Trace: the blob format
1:57:34 Proof public inputs
2:14:13 Async and static calls
2:28:19 The proving system and the accumulator
2:41:33 Builder integration and proving speed
2:56:10 Status and roadmap
3:02:47 Tokens and bridges (the AMB)
3:06:58 Cross-chain flash loans
```
*Chapters are derived from the transcript and are approximate — sanity-check a couple before publishing.*

Resources:
- Written summary and full Q&A: [link]
- Slides: [link]
- Learn more about EEZ: [site / docs link]
- Forum: [link]

---

## 2. EEZ Alliance channel message

*Voice note: informative, not an announcement; no em dashes; British English. Adapt the opener per channel (Telegram / Discord / email).*

> Hi all,
>
> We recorded this week's EEZ BuilderRoom session and it is now online. If you want the clearest picture yet of how EEZ actually works under the hood, the proxies, the composer, multi-prover settlement, and where the code stands today, this is the best single resource we have so far.
>
> It is a roughly three-hour technical deep dive with Friederike, Martin and Jordi, plus a long and genuinely sharp audience Q&A. Three ways in, depending on how deep you want to go:
>
> 📹 **Recording:** [YouTube link] (chaptered, so you can jump straight to what is relevant)
> 🖥 **Slides:** [link / attached]
> 📝 **Written summary:** [link] (a section-by-section digest plus the full Q&A, if you would rather read than watch)
>
> Worth flagging for context: this is engineering-level material, not polished marketing. EEZ is not live yet. The contracts are semi-final, there is a Chiado deployment running now, and an Ethereum testnet is targeted for end of summer (explicitly not for real funds). Gnosis Chain is lined up to be the first EEZ L2, in a limited capacity, around end of year.
>
> If you are building and want to test against it, or you have feedback on the spec, we would like to hear it. That is the reason for sharing this early. Reply here or message me directly.
>
> — Armagan

---

*Drafted 2026-06-20 from the 17 June 2026 BuilderRoom recording. Hold as draft until links are added and Armagan signs off.*
