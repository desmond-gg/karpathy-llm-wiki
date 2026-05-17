# Workflow Checklist

Use this checklist for non-trivial ingest, query, archive, and lint operations. It turns the schema and templates into an execution gate so the wiki stays consistent as it grows.

## Ingest Planning

Before editing wiki pages, identify:

- Source location: existing `raw/` note, pasted text, local file, or fetched URL content.
- Source language: `en`, `zh`, `mixed`, or `unknown`.
- Source authority: `official-spec`, `vendor-doc`, `paper`, `book`, `blog`, `forum`, `internal-note`, or `unknown`.
- Primary domain values from the controlled schema.
- Page type from the controlled schema.
- Version scope: exact spec version, architecture generation, IP version, product family, or `not-applicable`.
- Applicability scope: short controlled phrases for `applies_to`.
- Candidate glossary terms and whether a dedicated glossary page is needed.

If the operation starts from a URL, create or locate a flat `raw/` note first. Chrome + Obsidian Web Clipper automation is deferred and tracked in `todo.md`.

## Page Plan

For each ingest, decide the minimal page set:

| Source Pattern | Primary Page | Required Cascade |
|----------------|--------------|------------------|
| Stable concept | `concept` or domain-specific note | Update glossary and related pages when terms or links change |
| Architecture behavior | `architecture` or `microarchitecture` | Link affected IP, protocol, verification, or design-note pages |
| Protocol/spec family | `spec-family` | Create/update version pages and version matrix |
| Exact protocol/spec version | `spec-version` | Update family page and any version-sensitive concepts |
| IP block or interface | `ip-block`, `interface`, or `register-model` | Link architecture, frontend/backend, verification, and spec pages |
| RTL/design implementation note | `frontend-note`, `backend-note`, `timing-note`, `physical-design-note`, or `dft-note` | Link the governing architecture/spec/IP pages |
| Cross-source comparison | `comparison` | Link all compared pages and preserve version scope |

Do not create placeholder pages unless they carry useful structure or unblock a necessary wikilink.

## Claim Handling

- State version or applicability scope on claims about protocols, specs, architecture behavior, IP behavior, and implementation constraints.
- Preserve short English originals for key definitions or normative wording, then add Chinese explanation.
- Prefer official specs and vendor documentation over blogs or forums when resolving conflicts.
- When sources disagree, record the conflict with authority, date, version, and applicability instead of flattening it into one claim.
- Keep long source passages in `raw/`; wiki pages should synthesize.

## Update Gate

Before finishing an ingest or archive:

- Every changed wiki page has required frontmatter.
- `domain`, `type`, `version_status`, `source_authority`, and `source_language` use controlled values.
- Internal knowledge links use `[[wikilink]]`.
- Raw/source references use markdown relative links to flat `raw/` files.
- Spec-version changes update the family version matrix.
- New or changed terminology updates glossary pages when useful.
- `wiki/index.md` contains every touched non-log wiki page.
- `wiki/log.md` has one append-only entry for the operation.

## Query Answer Contract

Use `references/query-archive-guide.md` for detailed answer shape and archive criteria.

For wiki-backed answers:

- Start from `wiki/index.md`, then read relevant wiki pages and raw sources only when needed.
- State the scope first when version, architecture, IP, or protocol context affects correctness.
- Cite project-root-relative wiki paths in conversation.
- Mark gaps as source gaps instead of filling them with unsourced assumptions.
- If the answer creates reusable synthesis, ask or follow the user's instruction to archive it as a `type: archive` page.

## Lint Report Contract

Use `references/lint-rules.md` for detailed severity, deterministic checks, heuristic checks, and report format.

For lint operations, report:

- Auto-fixed deterministic issues.
- Remaining issues that need user review.
- Schema migration candidates.
- Broken or ambiguous wikilinks.
- Missing raw references.
- Version-scope risks.
- Terminology conflicts.
- Suggested follow-up sources or pages.

Do not silently rewrite existing pages for heuristic issues unless the user explicitly asks for fixes.
