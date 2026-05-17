# Lint Rules

Use these rules when running a lint pass over a chip Obsidian wiki. The goal is to keep the knowledge base navigable, version-aware, and useful for CPU/GPU/DPU/TPU architecture, IP, frontend/backend design, DFT, verification, and system engineering work.

## Severity

| Severity | Meaning | Default Action |
|----------|---------|----------------|
| Error | Breaks navigation, schema validity, or source traceability | Auto-fix only when deterministic; otherwise report |
| Warning | Can mislead engineering interpretation or future maintenance | Report unless the user asks to fix |
| Info | Improvement opportunity or missing enrichment | Report as follow-up |

## Deterministic Checks

These checks may be auto-fixed when the intended fix is unambiguous.

### Required Wiki Frontmatter

Every wiki knowledge page must have these fields. Exclude `wiki/index.md` and `wiki/log.md` from this frontmatter requirement.

- `title`
- `type`
- `domain`
- `tags`
- `aliases`
- `version`
- `version_status`
- `supersedes`
- `superseded_by`
- `applies_to`
- `source_authority`
- `source_language`
- `updated`

Rules:

- Missing required field on a newly edited page: add a conservative value.
- Missing required field on an existing page: add only if the value is obvious; otherwise report.
- `type` must be a scalar string.
- `domain`, `tags`, `aliases`, `supersedes`, `superseded_by`, `applies_to`, `source_authority`, and `source_language` must be YAML arrays.
- `updated` must use `YYYY-MM-DD`.

### Controlled Enums

Validate values against `SKILL.md`.

- `type` must be one of the controlled page types.
- `domain` must only contain controlled domain values.
- `version_status` must be `draft`, `active`, `deprecated`, `superseded`, `unknown`, or `not-applicable`.
- `source_authority` must only contain controlled source authority values.
- `source_language` must only contain `en`, `zh`, `mixed`, or `unknown`.

Rules:

- Unknown enum value introduced by the current edit: fix when the intended value is clear.
- Unknown enum value in older content: report as a schema migration candidate.
- Do not invent enum values during lint.

### Index Consistency

Validate `wiki/index.md` against actual wiki knowledge pages.

- Every wiki knowledge page except `wiki/index.md` and `wiki/log.md` should appear in `wiki/index.md`.
- Every index row should point to an existing wiki page title.
- Index rows should include Page, Type, Applies To / Version, Summary, and Updated.

Rules:

- Missing page in index: add an index row with a conservative one-line summary or `(no summary)`.
- Index entry for a missing page: mark `[MISSING]`; do not delete unless the user asks.

### Wikilinks

Validate Obsidian wikilinks in wiki pages.

- `[[Page Title]]` should resolve to a wiki page title or alias.
- `[[Page Title|Alias]]` should resolve by `Page Title`.
- Markdown links should not be used for internal wiki knowledge links.

Rules:

- Exactly one matching title or alias: fix the link target when safe.
- Zero or multiple matches: report with candidate pages.
- Do not create placeholder target pages only to silence lint.

### Raw References

Validate raw/source references.

- Links from wiki pages to `raw/` must use markdown relative links.
- Raw files must live directly under `raw/`, not topic subdirectories.
- The referenced raw file must exist.

Rules:

- Broken relative path with exactly one matching raw file name: fix the path.
- Missing or ambiguous raw file: report.
- Existing raw content is immutable; do not rewrite it during lint.

### Spec Family / Version Structure

Validate spec/protocol version modeling.

- `type: spec-family` pages should live at `wiki/specs/<family>/index.md` or an equivalent family anchor.
- `type: spec-version` pages should live under the family directory.
- Spec-version pages should link their family page.
- Family pages should contain a version matrix when version pages exist.
- Version matrix entries should link existing version pages.

Rules:

- Missing version matrix row for an existing version page: add when version/status are clear.
- Broken version link: fix when the target is unambiguous.
- Do not merge version pages into family pages.

## Heuristic Checks

These checks should be reported unless the user explicitly asks for fixes.

### Version Scope Risk

Report when:

- A protocol/spec/IP behavior claim has no `version` and no meaningful `applies_to`.
- A family page describes behavior that appears version-specific without linking a version page.
- A page mixes multiple spec versions without separating the claims.
- `version_status` contradicts `superseded_by` or body text.

### Source Authority Risk

Report when:

- Normative language is sourced only from `blog`, `forum`, or `unknown`.
- A lower-authority source conflicts with an official spec or vendor doc.
- A page uses `source_authority: unknown` for a design-significant claim.

### English Original / Chinese Explanation Risk

Report when:

- Important English definitions or normative `shall/must/may` wording are paraphrased without preserving a short original.
- English original is present but no Chinese explanation follows.
- The Chinese explanation changes the strength of a normative statement.

### Terminology Consistency Risk

Report when:

- The same English term has multiple Chinese translations without a glossary explanation.
- A glossary page exists but related pages do not link it.
- A term has spec-specific meanings but the glossary does not state scope.
- Headings translate industry-standard English terms in a way that may obscure searchability.

### Cross-Domain Coverage Risk

Report when:

- Architecture pages mention frontend/backend/DFT/verification impacts without linking related notes.
- IP pages reference protocols or register models without wikilinks.
- Protocol pages mention CPU cache, DMA, memory ordering, or coherency without linking memory-system or architecture pages when those pages exist.
- Backend or timing notes mention RTL controls, clocks, resets, or test modes without linking frontend or DFT pages when relevant.

### Orphan / Hub Risk

Report when:

- A non-archive wiki page has no inbound wikilinks.
- A repeated concept appears across multiple pages but has no dedicated page.
- A broad hub page is overloaded with implementation details that should move into dedicated pages.

### Archive Freshness Risk

Report when:

- An archive page cites wiki pages that have materially changed after the archive date.
- An archive answer lacks the version/applicability scope needed to interpret it.

## Report Format

Use this structure for lint output:

```md
## Lint Summary

- Checked: <N> wiki pages, <M> raw references
- Auto-fixed: <count>
- Errors: <count>
- Warnings: <count>
- Info: <count>

## Auto-Fixed

| File | Rule | Change |
|------|------|--------|
| <path> | <rule> | <change> |

## Needs Review

| Severity | File | Rule | Finding | Suggested Action |
|----------|------|------|---------|------------------|
| Error | <path> | <rule> | <finding> | <action> |

## Suggested Follow-Up

- <source gap, page candidate, schema migration, or review question>
```

Append a matching entry to `wiki/log.md` after lint:

```md
## [YYYY-MM-DD] lint | <N> issues found, <M> auto-fixed
```
