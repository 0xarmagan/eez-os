---
title: Zisk
type: entity
subtype: project
tags: [zkVM, ZK-proving, Ethereum, EEZ, Circom, Baylina]
source_count: 5
updated: 2026-04-09
---

## Summary

Zisk is a zero-knowledge virtual machine (zkVM) project founded by Jordi Baylina, the creator of the Circom ZK programming language. After two years of development, Zisk claims to have achieved real-time proving of Ethereum blocks — the technical breakthrough that makes EEZ's synchronous composability model theoretically possible.

## Role / Function

Technical backbone of EEZ. Zisk's zkVM is the proving engine that allows rollups to execute transactions and generate proofs fast enough to enable synchronous (single-transaction) cross-chain calls. Without real-time ZK proving, cross-chain atomicity requires either optimistic (delay-based) or trusted intermediary approaches.

## Key relationships

[[entities/jordi-baylina]] — founder
[[entities/gnosis]] — EEZ co-initiator
[[entities/ethereum-foundation]] — co-funder

## Technical claims

- Real-time Ethereum block proving using an efficient open-source zkVM
- Baylina: "We spent two years building a ZKVM that can prove Ethereum blocks in real time"
- Circom heritage: Baylina is one of the most credentialed ZK engineers in the Ethereum ecosystem, creator of the dominant ZK circuit compiler

## Open questions

[TODO: Independent benchmark of Zisk proving speed at production Ethereum block sizes]
[TODO: Proving cost per block — economic feasibility at mainnet scale]
[TODO: Testnet performance vs. claimed real-time proving]
[TODO: Open-source code availability and audit status]

## Source appearances

- [[sources/eez-announcement-overview]] — technical co-announcer

## Credibility signals

- Jordi Baylina's Circom language is used in production by Polygon zkEVM, zkSync, and others
- 2-year development claim suggests genuine technical investment, not a rapid announcement
- Ethereum Foundation co-funding implies some technical validation was conducted before public support
