---
title: "PCIe No Snoop and CPU Cache Coherency"
type: "archive"
domain:
  - "protocols"
  - "cpu"
  - "memory-system"
tags:
  - "wiki"
  - "archive"
aliases: []
version: "5.0"
version_status: "not-applicable"
supersedes: []
superseded_by: []
applies_to:
  - "PCIe 5.0"
  - "CPU cache subsystem"
source_authority:
  - "internal-note"
source_language:
  - "mixed"
updated: 2026-05-17
archived: 2026-05-17
---

# PCIe No Snoop and CPU Cache Coherency

> Sources: [[PCIe No Snoop]]; [[PCIe 5.0]]; [[No Snoop]]

## Overview

Archived answer for the question: "What is the impact of PCIe No Snoop on CPU cache coherency?" Scope is the example vault's [[PCIe 5.0]] note and CPU-attached DMA paths.

## Answer

Short answer: [[No Snoop]] can reduce platform snoop work for a transaction, but it is not a generic coherency-safe optimization. If software, firmware, or an endpoint driver assumes coherent DMA visibility, enabling [[PCIe No Snoop]] without a matching platform policy can expose stale-data bugs.

Engineering interpretation:

- Treat No Snoop as a protocol attribute whose system behavior depends on the host bridge, coherency fabric, memory attributes, and DMA programming model.
- Keep the attribute visible in RTL request metadata until the integration point that applies platform policy.
- Verify enabled, disabled, and blocked No Snoop configurations.
- Confirm OS and firmware DMA coherency policy before enabling No Snoop for traffic that touches CPU-visible memory.

This archive is a point-in-time answer. It should not be edited just because [[PCIe No Snoop]] or [[PCIe 5.0]] changes later; create a new archive or update the underlying wiki pages instead.

## See Also

- [[PCIe]]
- [[PCIe 5.0]]
- [[PCIe No Snoop]]
- [[No Snoop]]
