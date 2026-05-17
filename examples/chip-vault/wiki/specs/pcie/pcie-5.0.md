---
title: "PCIe 5.0"
type: "spec-version"
domain:
  - "specs"
  - "protocols"
tags:
  - "wiki"
aliases:
  - "PCI Express 5.0"
version: "5.0"
version_status: "active"
supersedes:
  - "PCIe 4.0"
superseded_by:
  - "PCIe 6.0"
applies_to:
  - "PCIe 5.0"
source_authority:
  - "internal-note"
source_language:
  - "mixed"
updated: 2026-05-17
---

# PCIe 5.0

> Sources: Example Notes, 2026-05-17
> Raw: [PCIe No Snoop Overview](../../../raw/2026-05-17-pcie-no-snoop-overview.md)
> Family: [[PCIe]]

## Overview

This example page demonstrates how to scope protocol knowledge to one exact spec version. The content is illustrative and must be replaced with official specification evidence before design use.

## Version Scope

This page applies to `version: "5.0"` and `applies_to: PCIe 5.0`. It is a sample page for documenting version-sensitive transaction attribute behavior.

## Terminology

- [[No Snoop]]: transaction attribute term; preferred Chinese explanation is "不要求平台执行 cache snoop 处理".
- [[PCIe No Snoop]]: protocol concept page that links the version behavior to engineering implications.

## Original / 中文说明

> Original: No Snoop is a PCIe transaction attribute indicating that a requester does not require platform cache-snoop processing for the transaction.
> 中文：No Snoop 是 PCIe transaction attribute，表示 requester 不要求平台对此 transaction 执行 cache snoop 处理。

## Normative Behavior

This sample does not quote PCIe Base Specification normative language. A production page should preserve only short necessary original wording and cite the official source.

## Architecture / Protocol Details

No Snoop is documented as a transaction-level attribute. In a CPU-attached platform, its practical effect depends on the host bridge, coherency fabric, memory attributes, DMA programming model, and software-visible ordering/coherency contract.

## Implementation Implications

- Frontend RTL should preserve the transaction attribute through the relevant request path when the IP supports it.
- Integration logic should not reinterpret No Snoop without a documented platform rule.
- Firmware/software enablement should be aligned with the memory type and DMA coherency model.

## Verification / Compliance Notes

- Add tests that distinguish snooped and non-snooped DMA paths when the platform supports both.
- Check stale-data scenarios where CPU cache state, DMA writes, and software synchronization are misaligned.
- Cover disabled, enabled, and blocked No Snoop configurations.

## Differences From Related Versions

| Area | Related Version | Change | Engineering Impact |
|------|-----------------|--------|--------------------|
| Transaction attribute handling | [[PCIe]] | This example page scopes behavior to PCIe 5.0 | Prevents treating a version-specific note as a family-wide rule |

## Open Questions

- Replace illustrative wording with official PCIe 5.0 references.
- Confirm platform-specific coherency handling for the target CPU/IP subsystem.

## See Also

- [[PCIe]]
- [[PCIe No Snoop]]
- [[No Snoop]]
