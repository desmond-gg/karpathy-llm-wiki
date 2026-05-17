# Chip Vault Example

This directory is a minimal Obsidian vault example for `chip-obsidian-wiki`.

It demonstrates:

- Flat `raw/` source storage.
- Deep `wiki/` organization.
- Obsidian `[[wikilink]]` usage between wiki pages.
- Markdown relative links from wiki pages back to raw sources.
- Spec family + spec version modeling.
- Dedicated glossary pages for bilingual terminology.
- Query answer archival as a point-in-time wiki page.
- `wiki/index.md` and `wiki/log.md` maintenance.

The technical content is intentionally small and illustrative. It is not a replacement for official PCIe specifications.

## Files

| File | Purpose |
|------|---------|
| `raw/2026-05-17-pcie-no-snoop-overview.md` | Example flat raw note with Obsidian properties |
| `wiki/specs/pcie/index.md` | Spec family page |
| `wiki/specs/pcie/pcie-5.0.md` | Spec version page |
| `wiki/protocols/pcie/no-snoop.md` | Protocol concept page |
| `wiki/glossary/no-snoop.md` | Bilingual glossary page |
| `wiki/archive/pcie-no-snoop-cache-coherency.md` | Archived query answer example |
| `wiki/index.md` | Global wiki index |
| `wiki/log.md` | Append-only operation log |
