# Knowledge Base Content

This repository holds the **content** (markdown source) for the knowledge base
that powers RAG search across multiple consumers (Feishu bot, web UI, CLI, etc.).

## Structure

Knowledge articles are plain `.md` files. Each file can be:

- A standalone concept / FAQ entry
- A category-level overview linking to sub-files
- A reference table (e.g. error codes)

Recommended frontmatter (all fields optional):

```markdown
---
title: <human-readable title>
tags: [tag1, tag2]
category: <category>
summary: <one-line summary>
source: <original URL if imported>
---
```

## Conventions

- One concept per file (preferred).
- Use clear H1/H2 headings — they become chunk boundaries for retrieval.
- Keep files under ~800 words; split long ones.
- Use descriptive file names — they appear in citations (`handbook/leave-policy.md`).

## Editing

Edit locally, then:

```bash
git add . && git commit -m "..." && git push
```

The server pulls every 5 minutes and re-indexes automatically.

## Consumers

This repo is **content-only**. The retrieval/indexing service lives separately
in `/root/knowledge-base/`. Feishu bot, web UIs, and other tools query that
service over HTTP.
