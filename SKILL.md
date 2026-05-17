---
name: chip-obsidian-wiki
description: "Use when building or maintaining an Obsidian knowledge base for digital chip frontend/backend design, CPU/GPU/DPU/TPU architecture, ARM/x86/AMD-related IP, protocols, specs, and system engineering. Triggers: ingesting chip/design sources, maintaining spec versions, querying CPU/GPU/DPU/TPU/IP knowledge, linting Obsidian wiki quality, or updating technical glossary/version pages."
---

# Chip Obsidian Wiki

Build and maintain a professional Obsidian knowledge base for digital chip frontend/backend design, CPU/GPU/DPU/TPU architecture, ARM/x86/AMD-related IP, protocols, specs, and system engineering. The agent maintains compiled wiki pages; the user curates sources, reviews results, and asks engineering questions.

Core principles:
- The wiki is a persistent, compounding engineering artifact.
- Raw material is immutable source evidence; wiki pages are maintained synthesis.
- Claims about protocols, specs, architecture behavior, IP behavior, and implementation constraints must carry version or applicability scope.
- English normative definitions may be preserved when useful, with a Chinese explanation or translation.

## Architecture

Three layers live under the user's project root:

**raw/** — Flat source-material directory. Raw files are not organized into topic subdirectories. The agent reads raw files but does not rewrite existing raw source text except when creating a new raw file during ingest. Users may also create raw files directly through Obsidian Web Clipper.

**wiki/** — Compiled Obsidian knowledge pages. The agent owns this layer. Deep directory structures are allowed when they improve navigation, for example:
- `wiki/architecture/cpu/arm/`
- `wiki/architecture/cpu/x86/`
- `wiki/architecture/gpu/`
- `wiki/architecture/dpu/`
- `wiki/architecture/tpu/`
- `wiki/microarchitecture/cache/`
- `wiki/frontend-design/rtl/`
- `wiki/frontend-design/clock-reset-power/`
- `wiki/backend-design/floorplan/`
- `wiki/backend-design/timing-closure/`
- `wiki/dft/scan-atpg/`
- `wiki/specs/amba-chi/`
- `wiki/protocols/pcie/`
- `wiki/ip/arm/`
- `wiki/ip/amd/`
- `wiki/memory-system/`
- `wiki/verification/`
- `wiki/glossary/`
- `wiki/design-notes/`
- `wiki/archive/`

`wiki/` contains two special files:
- `wiki/index.md` — Global index grouped by domain/path, with Obsidian wiki links, page type, applicability/version, summary, and Updated date.
- `wiki/log.md` — Append-only operation log.

**SKILL.md** — Schema layer. Defines structure, metadata, and workflows.

Templates live in `references/` relative to this file. Read them when the exact raw, article, archive, or index format is needed.

### Initialization

Triggers only on the first Ingest. Check whether `raw/` and `wiki/` exist. Create only what is missing; never overwrite existing files:

- `raw/` directory with `.gitkeep`
- `wiki/` directory with `.gitkeep`
- `wiki/index.md` — heading `# Chip Knowledge Base Index`, empty body
- `wiki/log.md` — heading `# Wiki Log`, empty body
- `wiki/glossary/` directory with `.gitkeep`

If Query or Lint cannot find the wiki structure, tell the user: "Run an ingest first to initialize the wiki." Do not auto-create.

---

## Metadata Schema

All wiki knowledge pages use Obsidian-compatible YAML frontmatter for Dataview queries.

Required fields:

```yaml
---
title: "{Page Title}"
type: "concept | architecture | microarchitecture | spec-family | spec-version | protocol | ip-block | interface | register-model | frontend-note | backend-note | dft-note | timing-note | physical-design-note | verification-note | comparison | design-note | glossary | archive"
domain:
  - "cpu | gpu | dpu | tpu | architecture | microarchitecture | frontend-design | backend-design | dft | physical-design | specs | protocols | ip | memory-system | verification | glossary"
tags:
  - "wiki"
aliases: []
version: ""
version_status: "draft | active | deprecated | superseded | unknown | not-applicable"
supersedes: []
superseded_by: []
applies_to: []
source_authority:
  - "official-spec | vendor-doc | paper | book | blog | forum | internal-note | unknown"
source_language:
  - "en | zh | mixed | unknown"
updated: YYYY-MM-DD
---
```

Use empty arrays or `not-applicable` when a field is not relevant. Do not omit required fields.

### Schema Reference

`SKILL.md` is the source of truth for controlled schema values. Templates must stay consistent with this section.

**domain** is a multi-value field. Use it to combine engineering responsibility, business object, and technical area:

- Responsibility domains: `frontend-design`, `backend-design`, `physical-design`, `dft`, `verification`
- Object domains: `cpu`, `gpu`, `dpu`, `tpu`, `ip`
- Technical domains: `architecture`, `microarchitecture`, `specs`, `protocols`, `memory-system`, `glossary`

Examples:

```yaml
domain:
  - "frontend-design"
  - "cpu"
  - "ip"
```

**type** describes page intent. Choose exactly one:

- General pages: `concept`, `comparison`, `design-note`, `archive`
- Architecture pages: `architecture`, `microarchitecture`
- Spec/protocol pages: `spec-family`, `spec-version`, `protocol`
- IP/interface pages: `ip-block`, `interface`, `register-model`
- Engineering notes: `frontend-note`, `backend-note`, `dft-note`, `timing-note`, `physical-design-note`, `verification-note`
- Glossary pages: `glossary`

**applies_to** uses controlled short phrases for applicability scope. Prefer established names and versions:

- ISA / architecture: `ARMv8-A`, `ARMv9-A`, `x86-64`
- Microarchitecture / product family: `AMD Zen`, `CPU cluster`, `GPU cache subsystem`
- Protocol / spec: `PCIe 5.0`, `AMBA CHI Issue E`
- IP / subsystem: `GIC`, `SMMU`, `NoC`, `L2 cache`

Use `applies_to: []` only when the page is genuinely scope-independent. Do not put long prose in `applies_to`; put explanation in `Version Scope` or the relevant body section.

**tags** are auxiliary. Use `wiki`, `raw`, and `archive` as structural tags, and optional state/topic tags such as `needs-review` or `draft`. Do not duplicate `domain`, `type`, or `applies_to` in `tags`.

### Schema Governance

- Do not invent new `domain` or `type` values during ingest, query, archive, or lint.
- If existing values cannot express a new chip-design category, report the gap to the user first.
- Add new enum values only through an explicit schema update that changes `SKILL.md`, affected templates, and README together.
- When schema changes may affect existing pages, lint should report candidate pages for migration instead of silently rewriting them.

Raw source files created by the agent use Obsidian properties, not the wiki schema:

```yaml
---
title: "{Source Title}"
source: "{URL or origin description}"
author:
  - "[[Author Or Organization]]"
published: YYYY-MM-DD
created: YYYY-MM-DD
description: ""
tags:
  - "raw"
---
```

If the source published date is unknown, use `published: Unknown`. `created` is the date the Obsidian raw note is created.

---

## Linking Conventions

- Wiki knowledge pages use Obsidian wikilinks for internal knowledge links: `[[PCIe No Snoop]]`, `[[AMBA CHI Issue E]]`, `[[Request Node|RN]]`.
- Raw/source references use standard markdown relative links so file provenance remains explicit. Compute the path from the current wiki file to the flat `raw/` file, for example `[raw source](../../../raw/pci-e-no-snoop.md)` from `wiki/specs/amba-chi/issue-e.md`.
- In conversation output, cite project-root-relative markdown paths, for example `[PCIe No Snoop](wiki/protocols/pcie/no-snoop.md)`.
- In `wiki/index.md`, use Obsidian wikilinks for article references and markdown links only for raw/source paths when needed.

---

## Ingest

Fetch or locate a source in `raw/`, then compile it into `wiki/`. Ingesting a source usually affects multiple wiki pages.

### Fetch or Locate Raw Source

1. If the user provides an existing raw note, read it from `raw/`.
2. If the user provides pasted text or a local file, create a new flat raw note at `raw/YYYY-MM-DD-descriptive-slug.md`.
3. If the user provides a URL and no local raw note exists, fetch the content using available tools and create a flat raw note at `raw/YYYY-MM-DD-descriptive-slug.md`.
4. Do not create topic subdirectories under `raw/`.
5. Use `references/raw-template.md` exactly for newly created raw notes.
6. Preserve original source text. Clean formatting noise only when creating the raw note. Do not rewrite opinions, definitions, or spec language.

URL ingest through Chrome + Obsidian Web Clipper is a future workflow and is tracked in `todo.md`; until implemented, use available non-browser fetch/file tools or ask the user to provide the clipped raw note.

### Compile into Wiki

Determine where the source belongs:

- **Existing concept or entity** → Merge into the relevant article and update affected sections.
- **New concept/entity** → Create a new page in the most relevant deep directory.
- **Protocol or spec family** → Maintain a family page plus separate version pages.
- **Specific spec version** → Create or update a `spec-version` page under the family directory.
- **Cross-domain material** → Place the primary page in the strongest domain and add wikilinks to related pages elsewhere.

Suggested directory anchors:
- CPU architecture: `wiki/architecture/cpu/arm/`, `wiki/architecture/cpu/x86/`
- GPU architecture: `wiki/architecture/gpu/`
- DPU/TPU architecture: `wiki/architecture/dpu/`, `wiki/architecture/tpu/`
- Microarchitecture: `wiki/microarchitecture/<area>/`
- Frontend design: `wiki/frontend-design/rtl/`, `wiki/frontend-design/cdc-rdc/`, `wiki/frontend-design/low-power/`, `wiki/frontend-design/register-interface/`, `wiki/frontend-design/clock-reset-power/`
- Backend design: `wiki/backend-design/synthesis/`, `wiki/backend-design/floorplan/`, `wiki/backend-design/pnr/`, `wiki/backend-design/sta/`, `wiki/backend-design/cts/`, `wiki/backend-design/ir-em/`, `wiki/backend-design/eco/`
- DFT/test: `wiki/dft/scan-atpg/`, `wiki/dft/mbist-lbist/`
- Specs/protocols: `wiki/specs/<family>/` or `wiki/protocols/<protocol>/`
- IP blocks: `wiki/ip/<vendor-or-family>/<ip-name>/`
- Memory system: `wiki/memory-system/`
- Verification/system engineering: `wiki/verification/`
- Glossary: `wiki/glossary/`

When merging, check for factual conflicts. If sources disagree, annotate the disagreement with source authority, date, version, and applicability.

### Spec and Protocol Versions

Use the family + version model:

- Family page: `wiki/specs/<spec-family>/index.md`
- Version page: `wiki/specs/<spec-family>/<version-slug>.md`

Example:
- `wiki/specs/amba-chi/index.md` — AMBA CHI family overview, version matrix, terminology, links to versions.
- `wiki/specs/amba-chi/issue-e.md` — AMBA CHI Issue E behavior, definitions, constraints, and differences.

Rules:
- A version-specific claim must state its `version` and `applies_to` scope.
- The family page summarizes stable concepts and version differences; it must not hide version-dependent behavior.
- When adding a new version page, update the family page's version matrix and cross-links.
- Use `supersedes` and `superseded_by` when the relationship is known.

### English Original + Chinese Explanation

For English source material:

- Preserve short normative definitions, protocol requirements, and precise spec wording when useful.
- Add a Chinese translation or explanation directly below the original.
- Do not copy long passages from copyrighted sources; quote only short necessary snippets and otherwise paraphrase.
- Keep technical English terms when they are industry-standard, and map them through the glossary.

Recommended format:

```md
> Original: A Request Node issues requests into the coherent interconnect.
> 中文：请求节点向一致性互连发起请求。

说明：这里的 Request Node 是协议角色，不等同于 CPU core 本身；具体实现中可能由 cluster、DMA 或其他 master 侧组件承担。
```

### Engineering Synthesis

Wiki pages should translate source material into engineering value. Prefer sections such as:

- `Overview`
- `Terminology`
- `Version Scope`
- `Original / 中文说明`
- `Architecture / Protocol Details`
- `RTL / Microarchitecture Mapping`
- `Frontend Design Notes`
- `Backend / Physical Design Notes`
- `DFT / Test Notes`
- `Implementation Implications`
- `PPA / Timing / Area Notes`
- `Verification / Integration Notes`
- `Version Differences`
- `Open Questions`
- `Sources`
- `See Also`

Omit sections that do not apply.

### Cascade Updates

After updating the primary page:

1. Update related pages linked from the article.
2. Search `wiki/index.md` for related pages in other domains.
3. For spec-version changes, update the spec-family page.
4. For new or changed terminology, update relevant `wiki/glossary/` pages.
5. Refresh `updated` in every wiki page whose knowledge content changed.

Archive pages are point-in-time snapshots and are not cascade-updated.

### Post-Ingest

Update `wiki/index.md` for every touched page. Append to `wiki/log.md`:

```md
## [YYYY-MM-DD] ingest | <primary page title>
- Raw: <raw file path>
- Updated: <cascade-updated page title>
```

Omit `- Updated:` lines when no cascade updates occur.

---

## Query

Search the wiki and answer questions using the compiled knowledge base.

### Steps

1. Read `wiki/index.md` to locate relevant pages.
2. Read the relevant wiki pages and, when needed, linked raw sources.
3. Prefer wiki content over training knowledge.
4. When answering technical questions, include version/applicability scope when it affects correctness.
5. Cite wiki pages with project-root-relative markdown links in conversation.
6. Do not write files unless the user asks to archive or update the wiki.

### Archiving

When the user explicitly asks to archive or save an answer:

1. Write a new `type: archive` wiki page using `references/archive-template.md`.
2. Store it under the most relevant domain or `wiki/archive/`.
3. Use Obsidian wikilinks for cited wiki pages.
4. Do not include a Raw field unless the answer directly cites raw sources.
5. Update `wiki/index.md`.
6. Append to `wiki/log.md`:

```md
## [YYYY-MM-DD] query | Archived: <page title>
```

---

## Lint

Quality checks keep the wiki useful for chip engineering work.

### Deterministic Checks (auto-fix when safe)

**Index consistency** — compare `wiki/index.md` against actual wiki files:
- File exists but missing from index → add an entry with `(no summary)` placeholder.
- Index entry points to nonexistent file → mark as `[MISSING]`; do not delete.

**Wikilinks** — for every `[[...]]` in wiki pages:
- Target does not exist → search wiki for a matching title or alias.
- Exactly one match → fix the title/alias if safe.
- Zero or multiple matches → report to the user.

**Raw references** — every markdown link to `raw/` must point to an existing flat raw file:
- Target does not exist → search `raw/` by file name.
- Exactly one match → fix the relative path.
- Zero or multiple matches → report to the user.

**Required frontmatter** — every wiki page must include the required metadata fields. Add missing fields with conservative values when safe.

**Schema enum consistency** — `domain` and `type` must use values from Schema Reference:
- Unknown values in a newly edited page → replace only when the intended value is obvious.
- Unknown values in existing pages → report as migration candidates.

### Heuristic Checks (report unless asked to fix)

- Version-specific technical claims missing `version` or `applies_to`
- Protocol/spec behavior copied to a family page without version scope
- English terminology with inconsistent Chinese translation
- Conflicting claims across pages without authority/version annotation
- Outdated claims superseded by newer specs or source material
- Important concepts mentioned repeatedly but missing dedicated pages
- Orphan pages without inbound wikilinks
- Missing cross-domain references
- Archive pages whose cited wiki pages changed materially after archival
- New chip-design categories that may require an explicit schema update

### Post-Lint

Append to `wiki/log.md`:

```md
## [YYYY-MM-DD] lint | <N> issues found, <M> auto-fixed
```

---

## Conventions

- Use standard markdown plus Obsidian wikilinks.
- Deep `wiki/` directories are allowed when they improve domain navigation.
- Keep `raw/` flat; do not create raw topic subdirectories.
- File names use kebab-case English slugs when practical.
- Today's date is used for `created`, `updated`, log entries, and archived dates.
- Published dates come from the source; use `Unknown` when unavailable.
- Wiki pages are maintained synthesis; raw files are source evidence.
- Ingest, archive, and lint operations update `wiki/log.md`; ingest and archive also update `wiki/index.md`.
