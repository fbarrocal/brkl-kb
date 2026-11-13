---
type: index
title: Knowledge Base Index
updated: <YYYY-MM-DD>
---

# Index

Navigation entry point. Every query and ingest starts here — most of them read nothing else. Keep it current; lint checks it against the files actually on disk. Group node entries by type as the KB grows.

Each node entry carries the page's title **and its aliases**, so resolving a name — dedup on ingest, lookup on query — costs one read of this file instead of a scan of every page. An entry missing its aliases silently turns a one-read operation into a folder scan. Format:

```
- [[Page Title]] — aliases: Variant Name, Another Name
- [[Page With No Aliases]]
```

A knowledge page's link is its title, because its filename is its title.

## Nodes
_(none yet — node pages appear here as they are written by ingest, grouped by type)_

## Sources awaiting ingest
- See the root of `raw/` (processed sources live in `raw/archive/`).
