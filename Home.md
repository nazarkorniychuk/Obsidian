---
type: dashboard
---

# 🏠 Home

> Entry point. Navigate by [[Deep Learning Basics|the map]] or `Cmd+O` — never by folder.

## 🗺 Maps

- **[[Deep Learning Basics]]** — the whole cluster, organized
- **[[RL MOC]]** — reinforcement learning (foundations written, filling in)
- Hubs: [[Transformer]] (architecture) · [[Neural Network]] (foundations) · [[Reinforcement Learning]] (the RL loop)

## 🕐 Recently edited

```dataview
LIST FROM "10 Notes" OR "20 Maps"
SORT file.mtime DESC
LIMIT 8
```

## 🌱 Seedlings & growing — needs work

```dataview
LIST FROM "10 Notes" OR "00 Inbox"
WHERE status = "seedling" OR status = "growing"
SORT file.mtime ASC
```

## 📥 Inbox — process weekly

```dataview
LIST FROM "00 Inbox"
```

## 🏝 Orphans — no note links here

```dataview
LIST FROM "10 Notes"
WHERE length(file.inlinks) = 0
```

## 📚 Latest sources added

```dataview
TABLE year, rating FROM "30 Sources"
SORT file.ctime DESC
LIMIT 8
```
