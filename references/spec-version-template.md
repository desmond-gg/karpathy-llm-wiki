---
title: "{Spec Version Title}"
type: "spec-version"
domain:
  - "specs"
  - "protocols"
tags:
  - "wiki"
aliases: []
version: "{exact version, issue, revision, or release}"
version_status: "{draft | active | deprecated | superseded | unknown}"
supersedes: []
superseded_by: []
applies_to:
  - "{controlled applicability phrase, e.g. AMBA CHI Issue E}"
source_authority:
  - "{official-spec | vendor-doc | paper | book | blog | forum | internal-note | unknown}"
source_language:
  - "{en | zh | mixed | unknown}"
updated: {YYYY-MM-DD}
---

# {Spec Version Title}

> Sources: {Author, Organization, or Publication + date; semicolon-separated}
> Raw: [{raw source title}]({relative path to flat raw file})
> Family: [[{Spec Family Title}]]

## Overview

{Summarize what this exact version defines and where it applies. State the version scope explicitly.}

## Version Scope

{State exact version/revision, publication status, supersedes/superseded-by relationship, and affected architecture/IP/protocol contexts.}

## Terminology

{Define version-relevant terms and link glossary pages with Obsidian wikilinks.}

## Original / 中文说明

{For critical English definitions or normative spec statements, include short original wording and a Chinese translation or explanation. Do not quote long copyrighted passages.}

```md
> Original: {short original definition or normative sentence}
> 中文：{Chinese translation or explanation}
```

## Normative Behavior

{Capture precise shall/must/may behavior, protocol rules, ordering rules, transaction semantics, state transitions, or compliance points for this version.}

## Architecture / Protocol Details

{Describe protocol flow, roles, message/packet fields, ordering/coherency implications, error handling, or interface behavior.}

## Implementation Implications

{Explain consequences for IP integration, RTL, microarchitecture, firmware/software interface, PPA, physical implementation, or system behavior.}

## Verification / Compliance Notes

{Capture compliance checks, corner cases, assertions, coverage points, interoperability risks, and debug guidance.}

## Differences From Related Versions

| Area | Related Version | Change | Engineering Impact |
|------|-----------------|--------|--------------------|
| {area} | [[{Other Version Page}]] | {short difference} | {design/verification/integration impact} |

## Open Questions

{Unresolved points, ambiguous wording, source gaps, or items needing official spec confirmation. Omit when empty.}

## See Also

{Use Obsidian wikilinks for related wiki pages, for example:
- [[{Spec Family Title}]]
- [[Related Protocol Concept]]
- [[Related Glossary Term]]}
