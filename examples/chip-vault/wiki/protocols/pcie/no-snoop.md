---
title: "PCIe No Snoop"
type: "protocol"
domain:
  - "protocols"
  - "cpu"
  - "memory-system"
tags:
  - "wiki"
aliases:
  - "No Snoop Attribute"
version: "5.0"
version_status: "active"
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
---

# PCIe No Snoop

> Sources: Example Notes, 2026-05-17
> Raw: [PCIe No Snoop Overview](../../../raw/2026-05-17-pcie-no-snoop-overview.md)

## Overview

PCIe No Snoop is a transaction attribute concept. In this sample vault, the page is scoped to [[PCIe 5.0]] and focuses on the engineering risk around CPU cache coherency assumptions.

## Terminology

- [[No Snoop]]: preferred English term. Use Chinese explanation rather than replacing it with a loose translation in headings.
- Requester: the agent that issues the PCIe transaction.
- Snoop: platform cache-coherency lookup or intervention, depending on the system architecture.

## Version Scope

This example page applies to `PCIe 5.0` only. For other PCIe versions or platform-specific host bridge behavior, create or update the appropriate `spec-version` page.

## Original / 中文说明

> Original: No Snoop is a PCIe transaction attribute indicating that a requester does not require platform cache-snoop processing for the transaction.
> 中文：No Snoop 表示 requester 不要求平台对该 transaction 执行 cache snoop 处理；这不等价于所有系统都可以安全跳过软件同步或 cache 管理。

## Architecture / Protocol Details

No Snoop affects how a transaction is interpreted by the platform coherency path. The attribute itself is protocol-level, but the observable behavior depends on CPU cache policy, I/O coherency support, firmware configuration, and endpoint DMA programming.

## RTL / Microarchitecture Mapping

For an IP block that can generate PCIe traffic, the No Snoop attribute usually maps to request metadata that must be carried alongside address, length, traffic class, and ordering attributes. The design should avoid dropping or defaulting the bit without an explicit integration rule.

## Frontend Design Notes

- Define whether the IP can generate No Snoop traffic.
- Keep the attribute visible at module boundaries where integration policy is applied.
- Document reset/default behavior and software-visible controls.

## Backend / Physical Design Notes

No Snoop itself is not usually a physical design constraint, but carrying extra request metadata can affect muxing, queue width, and timing on request issue paths.

## Implementation Implications

Incorrect use can create stale-data bugs when DMA traffic and CPU cache state are not synchronized. Treat No Snoop enablement as a system-level decision rather than a local endpoint optimization.

## Verification / Integration Notes

- Verify attribute propagation from endpoint request generation to host-facing interface.
- Add negative tests where No Snoop is blocked by configuration.
- Cover software synchronization assumptions in coherency-sensitive DMA tests.

## Version Differences

See [[PCIe]] and [[PCIe 5.0]]. Do not generalize this page to other versions without updating `version` and `applies_to`.

## Open Questions

- Confirm target SoC host bridge behavior for No Snoop requests.
- Confirm firmware and OS policy for coherent vs non-coherent DMA mappings.

## See Also

- [[PCIe]]
- [[PCIe 5.0]]
- [[No Snoop]]
