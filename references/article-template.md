---
title: "{Title}"
type: "{concept | architecture | microarchitecture | spec-family | spec-version | protocol | ip-block | interface | register-model | frontend-note | backend-note | dft-note | timing-note | physical-design-note | verification-note | comparison | design-note | glossary}"
domain:
  - "{cpu | gpu | dpu | tpu | architecture | microarchitecture | frontend-design | backend-design | dft | physical-design | specs | protocols | ip | memory-system | verification | glossary}"
tags:
  - "wiki"
aliases: []
version: "{version or empty string}"
version_status: "{draft | active | deprecated | superseded | unknown | not-applicable}"
supersedes: []
superseded_by: []
applies_to: []
source_authority:
  - "{official-spec | vendor-doc | paper | book | blog | forum | internal-note | unknown}"
source_language:
  - "{en | zh | mixed | unknown}"
updated: {YYYY-MM-DD}
---

# {Title}

> Sources: {Author, Organization, or Publication + date; semicolon-separated}
> Raw: [{raw source title}]({relative path to flat raw file})

## Overview

{One paragraph summarizing the stable knowledge in this page. Include version/applicability scope when it affects correctness.}

## Terminology

{Define important English terms and their Chinese translations. Link dedicated glossary pages with Obsidian wikilinks, for example [[Request Node]].}

## Version Scope

{State exact spec/protocol/IP/architecture versions covered by this page. Use "Not applicable" only for genuinely version-independent concepts.}

## Original / 中文说明

{For critical English definitions or normative spec statements, include short original wording and a Chinese translation or explanation. Do not quote long copyrighted passages.}

```md
> Original: {short original definition or normative sentence}
> 中文：{Chinese translation or explanation}
```

## Architecture / Protocol Details

{Synthesize the technical behavior, architecture relationship, protocol flow, state machine, interface implication, or IP behavior.}

## RTL / Microarchitecture Mapping

{Describe how the concept maps into RTL hierarchy, datapath/control-path partitioning, pipeline stages, state machines, buffers, queues, arbiters, cache structures, or IP block boundaries. Omit when not applicable.}

## Frontend Design Notes

{Capture digital frontend concerns such as RTL design intent, interface definitions, register model, clock/reset strategy, CDC/RDC, low-power intent, parameterization, synthesizability constraints, and design tradeoffs. Omit when not applicable.}

## Backend / Physical Design Notes

{Capture backend concerns such as synthesis constraints, floorplan assumptions, placement/routing pressure, timing closure risks, CTS implications, congestion hotspots, IR/EM sensitivity, ECO considerations, and signoff boundaries. Omit when not applicable.}

## DFT / Test Notes

{Capture scan, MBIST/LBIST, ATPG, test mode interactions, observability/controllability concerns, test coverage risks, and manufacturing test assumptions. Omit when not applicable.}

## Implementation Implications

{Explain consequences for CPU/GPU/IP design, integration, firmware/software interface, performance, power, area, or system behavior.}

## PPA / Timing / Area Notes

{Summarize expected impact on frequency, latency, throughput, area, power, thermal behavior, timing-critical paths, and implementation cost. Omit when not applicable.}

## Verification / Integration Notes

{Capture verification risks, corner cases, interoperability concerns, compliance points, or debug notes.}

## Version Differences

{For spec-family or version-sensitive pages, summarize differences across versions. Omit when not applicable.}

## Open Questions

{Unresolved points, source gaps, or items needing official spec confirmation. Omit when empty.}

## See Also

{Use Obsidian wikilinks for wiki knowledge pages, for example:
- [[Related Concept]]
- [[AMBA CHI Issue E]]
- [[PCIe No Snoop]]}
