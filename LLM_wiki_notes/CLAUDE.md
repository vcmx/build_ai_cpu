# CLAUDE.md

Purpose: maintain this Obsidian-based LLM Wiki as a persistent, interlinked markdown knowledge base.

## Architecture

This vault has three layers:

1. `sources/` = raw source documents; read-only.
2. `wiki/` = generated markdown wiki; editable by the agent.
3. `schema/` = supporting rules, templates, and conventions. [page:1]

## Operating principles

- Never modify original raw source files unless the user explicitly asks.
- Prefer incremental updates over full rewrites.
- Preserve existing valid wiki-links.
- Keep pages concise, factual, and cross-linked.
- When new information contradicts older pages, preserve the contradiction with attribution instead of silently overwriting it. [page:1]

## Required files

- `wiki/index.md`: master catalog of pages, grouped by type, with one-line summaries. Karpathy describes the index as the content-oriented navigation layer the LLM should read first when answering queries. [page:1]
- `wiki/log.md`: append-only operation log for ingest, query, and lint actions. Karpathy describes the log as chronological and useful for recent-history inspection. [page:1]

## Ingest workflow

When asked to ingest a source:

1. Read one file from `sources/`.
2. Create or update a source summary page in `wiki/sources/`.
3. Create or update related entity pages in `wiki/entities/`.
4. Create or update related concept pages in `wiki/concepts/`.
5. Add or improve `[[wiki-links]]` between affected pages.
6. Update `wiki/index.md`.
7. Append an ingest record to `wiki/log.md`. [page:1]

A single source may affect many pages, and Karpathy notes that 10 to 15 touched pages is normal. [page:1]

## Query workflow

When asked a question:

1. Read `wiki/index.md` first.
2. Open only the most relevant pages.
3. Synthesize the answer from wiki content.
4. If the answer creates durable value, save it as a new synthesis page.
5. Append a query record to `wiki/log.md`. [page:1]

## Lint workflow

When asked to lint:

- detect contradictions,
- stale claims,
- orphan pages,
- dead links,
- missing cross-references,
- duplicates,
- missing concept/entity pages for frequently mentioned topics. [page:1][page:2]

Apply safe fixes where possible and record the lint pass in `wiki/log.md`.

## Style conventions

- Prefer markdown headings over long prose blocks.
- Use short sections such as Summary, Key Points, Related Concepts, Related Entities, Sources, and Open Questions.
- Use wiki-links generously, but only when the target page is genuinely relevant.
- Add YAML frontmatter when useful for tags, aliases, dates, or source references. Karpathy notes Dataview becomes useful when the LLM adds frontmatter consistently. [page:1]
