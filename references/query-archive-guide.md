# Query Archive Guide

Use this guide when answering questions from the compiled wiki and when saving a reusable answer back into the wiki as an archive page.

## Query Flow

1. Read `wiki/index.md` first.
2. Read the relevant wiki pages.
3. Read linked raw sources only when the wiki page lacks enough detail or provenance needs confirmation.
4. State the scope before the conclusion when version, architecture, protocol, IP, or product context affects correctness.
5. Cite wiki pages in conversation with project-root-relative markdown paths.
6. Do not write files unless the user asks to archive, save, or update the wiki.

## Answer Shape

For chip engineering questions, prefer:

- Scope: exact protocol/spec version, architecture generation, IP, or platform context.
- Short answer: direct conclusion.
- Engineering details: frontend/backend/DFT/verification/system implications when relevant.
- Source gaps: missing official specs, unclear platform behavior, or assumptions.
- Citations: wiki pages first, raw sources only when directly used.

Example conversation answer:

```md
Scope: PCIe 5.0 and CPU-attached DMA paths.

Short answer: No Snoop can reduce platform snoop work for a transaction, but it is not a generic coherency-safe optimization. If software or firmware assumes coherent DMA visibility, enabling No Snoop without a matching platform policy can expose stale-data bugs.

Engineering notes:
- Keep the No Snoop attribute visible through request metadata.
- Verify blocked/enabled No Snoop configurations.
- Confirm OS and firmware DMA coherency policy before enabling it.

Sources:
- [PCIe No Snoop](wiki/protocols/pcie/no-snoop.md)
- [PCIe 5.0](wiki/specs/pcie/pcie-5.0.md)
- [No Snoop](wiki/glossary/no-snoop.md)
```

## Archive Criteria

Archive a query answer only when at least one is true:

- The answer contains reusable synthesis across multiple pages.
- The answer captures an engineering decision or interpretation.
- The answer compares versions, architectures, protocols, or IP blocks.
- The answer records a source gap or follow-up research question.
- The user explicitly asks to save or archive the answer.

Do not archive simple lookups that add no durable value.

## Archive Page Rules

- Use `references/archive-template.md`.
- Set `type: "archive"`.
- Add both `wiki` and `archive` tags.
- Store under the most relevant domain directory or `wiki/archive/`.
- Use Obsidian wikilinks for cited wiki pages.
- Do not include a `Raw:` line unless the archived answer directly cites raw sources.
- Preserve version/applicability scope in frontmatter and in the Overview.
- Archive pages are point-in-time snapshots and are not cascade-updated.

## Required Cascade

After creating an archive page:

1. Add it to `wiki/index.md`.
2. Append to `wiki/log.md`:

```md
## [YYYY-MM-DD] query | Archived: <page title>
```

3. Do not update cited wiki pages unless the user also asked to improve the underlying knowledge.
