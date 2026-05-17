---
title: "PCIe No Snoop Overview"
source: "example://pcie-no-snoop-overview"
author:
  - "[[Example Notes]]"
published: Unknown
created: 2026-05-17
description: "Illustrative raw note for the chip Obsidian wiki example vault."
tags:
  - "raw"
---

# PCIe No Snoop Overview

This example raw note is intentionally short. It exists to demonstrate source provenance and wiki compilation structure.

No Snoop is a PCIe transaction attribute indicating that a requester does not require platform cache-snoop processing for the transaction.

In CPU-attached systems, misuse of No Snoop can expose stale data if software, firmware, DMA programming, or I/O memory attributes assume coherent visibility.

For engineering use, treat No Snoop as a version-scoped protocol behavior and validate the platform coherency model before enabling it for DMA traffic.
