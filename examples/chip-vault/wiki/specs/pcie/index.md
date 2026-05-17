---
title: "PCIe"
type: "spec-family"
domain:
  - "specs"
  - "protocols"
tags:
  - "wiki"
aliases:
  - "PCI Express"
version: ""
version_status: "not-applicable"
supersedes: []
superseded_by: []
applies_to:
  - "PCIe"
source_authority:
  - "internal-note"
source_language:
  - "mixed"
updated: 2026-05-17
---

# PCIe

> Sources: Example Notes, 2026-05-17
> Raw: [PCIe No Snoop Overview](../../../raw/2026-05-17-pcie-no-snoop-overview.md)

## Overview

PCIe is modeled as a spec family because protocol behavior, transaction attributes, ordering rules, and implementation requirements can vary by version and platform integration.

## Version Matrix

| Version | Status | Published | Supersedes | Superseded By | Notes |
|---------|--------|-----------|------------|---------------|-------|
| [[PCIe 5.0]] | active | Unknown | PCIe 4.0 | PCIe 6.0 | Example version page used to demonstrate version-scoped protocol notes |

## Stable Concepts

- [[No Snoop]] is a transaction attribute term that should remain in English in most wiki prose, with Chinese explanation where needed.
- [[PCIe No Snoop]] captures the protocol concept and engineering implications.

## Version-Specific Behavior

- No Snoop behavior and platform coherency interpretation must be checked against the exact PCIe version and host integration model. See [[PCIe 5.0]] for the example version-scoped page.

## Engineering Impact

No Snoop can affect DMA coherency assumptions, CPU cache visibility, verification scenarios, and system integration rules. Treat it as a cross-domain topic involving protocol, CPU memory system behavior, firmware/software configuration, and verification.

## Open Questions

- Replace this illustrative source with official PCI-SIG material before using the page for design decisions.

## See Also

- [[PCIe 5.0]]
- [[PCIe No Snoop]]
- [[No Snoop]]
