# Workflow

## Daily workflow

1. Collect new material into `sources/`.
2. Ingest one source at a time. Ask claude "I have just added new files in the raw folder. Please read it and update the wiki."
3. Review the generated pages in `wiki/`.
4. Ask cross-source questions.
5. Save valuable answers back into the vault.
6. Run lint periodically. [page:1][web:1]

## Ingest pattern

A practical ingest should usually do the following:

- create `wiki/sources/<slug>.md` as the source summary,
- update relevant entity pages,
- update relevant concept pages,
- maintain `wiki/index.md`,
- append a timestamped entry to `wiki/log.md`. [page:1][page:2]

Karpathy notes that the index is content-oriented and helps the LLM find relevant pages, while the log is chronological and records ingests, queries, and lint passes. [page:1]

## Query pattern

Ask the LLM questions against the wiki, not just the raw sources. The answer should cite or link back to relevant wiki pages, and strong outputs can be saved back as synthesis notes so they are not lost in chat history. [page:1]

## Lint pattern

A lint pass should look for:

- contradictions,
- stale claims,
- orphan pages,
- missing cross-links,
- dead links,
- duplicates,
- knowledge gaps for future research. [page:1][page:2]

## Example prompts

### Ingest prompt

```text
Read sources/<file>. Create or update the wiki using the rules in CLAUDE.md.
Write a source summary, update relevant concept/entity pages, refresh wiki/index.md,
and append a timestamped entry to wiki/log.md.
```

### Query prompt

```text
Answer this question using the wiki first, then save the answer as a synthesis page if it adds durable value.
```

### Lint prompt

```text
Audit the wiki for dead links, duplicates, orphans, contradictions, and stale claims.
Propose or apply fixes and record the lint pass in wiki/log.md.
```
