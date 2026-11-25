# Raw clip — LLM wiki pattern (IMMUTABLE: do not edit, re-fetch instead)

- Source: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
- Fetch date: 2026-11-25
- Author: Andrej Karpathy, gist dated 2026-04-04

## Pattern summary (distilled 2026-11-25, no training data)

Core idea: a persistent compounding artifact, NOT RAG re-derivation.

Three layers:

1. Raw sources (immutable) — the LLM reads them, never modifies them.
2. Wiki (LLM-owned markdown dir) — summaries, entity/concept pages,
   comparisons, synthesis. This repo's `wiki/` plays this role.
3. Schema doc (`AGENTS.md` / `CLAUDE.md`) — key config defining structure,
   conventions, and ingest/query/maintain workflows; co-evolved over time.

Operations:

- INGEST — one source can touch 10-15 pages: source summary + index update +
  entity/concept page updates + log entry. Prefer one-at-a-time supervised
  ingest; batch optional.
- QUERY — search index, drill in, synthesize with citations. Good answers get
  filed back as new pages so explorations compound.
- LINT — periodic health check: contradictions, stale/superseded claims,
  orphans, missing pages/refs, gaps, new questions/sources.

Indexing:

- `index.md` = content catalog (page + 1-line summary + metadata,
  per-category). The LLM reads it first on every query. Works to ~100
  sources / hundreds of pages; avoids embeddings.
- `log.md` = append-only chronology with parseable
  `## [YYYY-MM-DD] verb | Title` prefixes.

Optional tooling: CLI search (qmd: hybrid BM25/vector, CLI+MCP), Obsidian
Web Clipper + local image downloads, graph view, Marp decks, Dataview
frontmatter, git repo for history.

Why it works: maintenance bookkeeping (xrefs, summaries, contradiction
flags, consistency) is the burden humans abandon; the LLM does it at ~zero
cost. Human curates/sources/asks; LLM files/maintains.

Spirit: Bush's Memex associative trails; the LLM solves the maintenance
problem. Deliberately abstract — instantiate per domain.

What makes it useful to an LLM agent: distilled-once sources (compiled
synthesis, not chunk retrieval), cross-linking already in place,
contradiction flags already recorded, index-first navigation, log for
recency, schema for disciplined maintenance.
