---
type: meta
created: 2026-08-25
---

# Vault Conventions

The rules, as actually practiced.

## Structure

| Folder | Contains |
| --- | --- |
| `00 Inbox/` | captured stubs; graduate to `10 Notes/` when written, weekly sweep |
| `10 Notes/` | atomic concept notes — flat, no subfolders |
| `20 Maps/` | MOCs, one per domain: [[Deep Learning Basics]], [[RL MOC]] |
| `30 Sources/` | one page per paper: summary, key results, links, consensus URL + DOI |
| `Attachments/` | charts (matplotlib, house palette), Excalidraw |

## Note style (the house register)

- **Dense**: facts, numbers, formulas, results, conclusions — no motivational prose
- Per-topic structure: definition + notation → ⚡ summary tables up top → sections with **Results** and **Conclusion** bullets → Related → Sources
- **Every claim cites a source page** via wikilink (`[[Paper (Year)\|Author Year]]`); famous papers are not exempt
- In tables, escape wikilink pipes: `[[Note\|alias]]`
- Formulas over vibes; per-mini-plot charts where curves matter (function + derivative + reference)
- Completeness check before writing a big note: enumerate the canonical sub-topics, diff against draft, report omissions
- A stub graduates to its own note if ≥2–3 dedicated papers exist about it

## Statuses

`seedling` → `growing` → `evergreen` (notes) · sources are `processed` on creation, `rating` 1–5

## Workflow

- Every work session ends with commit + push (repo: nazarkorniychuk/Obsidian)
- Research via Consensus search; source page created before the claim is written
- Weekly: sweep inbox, check Home's orphans query, file new notes into the MOC
