---
title: "No Snoop"
type: "glossary"
domain:
  - "glossary"
tags:
  - "wiki"
aliases:
  - "不执行缓存窥探"
  - "不要求 cache snoop"
version: ""
version_status: "not-applicable"
supersedes: []
superseded_by: []
applies_to:
  - "PCIe 5.0"
source_authority:
  - "internal-note"
source_language:
  - "mixed"
updated: 2026-05-17
---

# No Snoop

> Sources: Example Notes, 2026-05-17
> Raw: [PCIe No Snoop Overview](../../raw/2026-05-17-pcie-no-snoop-overview.md)

## Translation

| English | 中文 | Aliases |
|---------|------|---------|
| No Snoop | 不要求 cache snoop 处理 | 不执行缓存窥探; 不要求缓存 snoop |

## Definition

No Snoop is a PCIe transaction attribute term. In Chinese wiki prose, keep the English term `No Snoop` and add a short explanation when the cache coherency implication matters.

## Original / 中文说明

> Original: No Snoop is a PCIe transaction attribute indicating that a requester does not require platform cache-snoop processing for the transaction.
> 中文：No Snoop 表示 requester 不要求平台对此 transaction 执行 cache snoop 处理。

## Usage Notes

- Prefer `No Snoop` in headings and tables.
- Avoid translating it as a general performance optimization; the term is about transaction attributes and coherency handling.
- If a page discusses actual system behavior, state the relevant spec version and platform coherency model.

## Scope

- `PCIe 5.0`
- `CPU cache subsystem`
- DMA coherency discussion

## Related Terms

- [[PCIe No Snoop]]

## See Also

- [[PCIe]]
- [[PCIe 5.0]]
