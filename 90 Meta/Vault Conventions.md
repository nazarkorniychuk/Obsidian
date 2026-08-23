---
type: meta
created: 2026-08-22
---

# Vault Conventions

The rules of this vault. When in doubt, check here. When the rules change, update here.

## Core principles

1. **Folders = what a note IS. Links = what a note is ABOUT.**
2. **One concept per note** (atomic notes).
3. **No orphans** — every note links to at least one other note.
4. **Navigate by `Cmd+O` and links**, never by browsing folders.
5. **Curate MOCs by hand**; Dataview auto-collection is the safety net.

## Folders

| Folder | Contains | Rule |
|---|---|---|
| `00 Inbox/` | raw captures | emptied weekly, mess allowed |
| `10 Notes/` | atomic concept notes | FLAT — no subfolders, ever |
| `20 Maps/` | MOCs (topic hubs) | one per topic; split at ~15 links/section |
| `30 Sources/` | papers, books, courses | one note per source |
| `40 Projects/` | finite active work | one subfolder per project |
| `50 Daily/` | daily notes | auto-created |
| `90 Meta/` | templates, this file | — |
| `Attachments/` | images, PDFs, Excalidraw | auto-target for pastes |

## Note types & frontmatter

| Field | On | Values |
|---|---|---|
| `type` | all | `concept` / `moc` / `source` / `daily` / `project` |
| `topics` | concept, source | see controlled vocabulary below |
| `status` (concept) | concept | `seedling` → `growing` → `evergreen` |
| `status` (source) | source | `to-read` → `reading` → `processed` |
| `created` | all | `YYYY-MM-DD` |
| `aliases` | as needed | alternative names, abbreviations |
| `authors`, `year`, `url`, `source-type`, `rating` | source | — |

## Topic vocabulary (controlled)

One value ≈ one MOC that exists or will exist. Add new values here first.

- `machine-learning`
- `deep-learning`
- `optimization`
- `linear-algebra`
- `probability`
- `calculus`

## Naming

- Concept notes: **descriptive singular noun phrase** — `Eigenvalue`, `Transformer`, `Central Limit Theorem`
- Disambiguate with parentheses: `Regularization (ML)` — and add aliases
- Sources: `Title (Year).md`
- Dailies: `YYYY-MM-DD.md`

## Linking grammar

- Link the **first meaningful mention** in prose
- "Related" sections: **annotate every link** with the relationship ("generalizes X", "contrast with Y", "prerequisite: Z")
- If you can't phrase why two notes connect — don't link them
- Red links (unwritten notes) are encouraged: they map the frontier
- Check the **backlinks panel** every time you open a note

## Tags

Only for transient states: `#todo`, `#check-this`. Everything durable lives in frontmatter or links. No tag hierarchies.

## Math & code

- Inline math `$x$`, display math `$$...$$`
- Notation is defined in the note that owns the concept
- Proofs go in collapsed callouts: `> [!note]- Proof`
- Short snippets in notes; real code in git repos, linked from notes

## Weekly maintenance (~20 min)

1. Empty `00 Inbox/`
2. File unfiled notes into MOCs (check auto-collected sections)
3. Grow 1–2 seedlings
4. Check the Orphans query on [[Home]]
5. Commit & push to git
