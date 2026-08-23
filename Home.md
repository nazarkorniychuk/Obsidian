---
type: dashboard
---

# 🏠 Home

> The front door. Everything is reachable from here.

## 🗺 Maps of Content

- [[Machine Learning MOC]]
- [[Deep Learning MOC]]
- [[Mathematics MOC]]

*(Add every new MOC here — this list is maintained by hand.)*

---

## 🕐 Recently edited

```dataview
LIST FROM "10 Notes" OR "20 Maps" OR "30 Sources"
SORT file.mtime DESC
LIMIT 10
```

## 🌱 Seedlings — needs growing

```dataview
LIST FROM "10 Notes"
WHERE status = "seedling"
SORT file.mtime ASC
LIMIT 15
```

## 📚 Reading pipeline

```dataview
TABLE authors, year, status FROM "30 Sources"
WHERE status != "processed"
SORT status DESC
```

## 🏝 Orphans — no note links here

```dataview
LIST FROM "10 Notes"
WHERE length(file.inlinks) = 0
```

## 📥 Inbox

```dataview
LIST FROM "00 Inbox"
```
