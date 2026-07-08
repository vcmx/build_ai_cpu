# Setup Guide

## 1. Create the vault

Create a new Obsidian vault or point Obsidian at this repository folder. The tutorial demonstrates setting up Obsidian first and then creating the folder structure for the wiki. [web:1]

## 2. Create the folders

At minimum, create:

- `sources/` for raw inputs,
- `wiki/` for generated notes,
- `schema/` for supporting templates or schema notes. [page:1][web:1]

Karpathy's architecture separates immutable raw sources from generated wiki pages and the schema that governs them. [page:1]

## 3. Create the schema file

Create `CLAUDE.md` in the vault root. In Karpathy's description, this file tells the LLM how the wiki is structured, what conventions it should follow, and how ingest, query, and lint workflows should work. [page:1]

The video also calls this the rules document that tells the AI how to operate the wiki. [web:1]

## 4. Add your first sources

Drop markdown notes, clipped articles, PDFs converted to markdown, transcripts, or paper notes into `sources/`. The video recommends source-by-source ingestion, and Karpathy notes he personally prefers ingesting one source at a time with human oversight. [web:1][page:1]

## 5. Run the first ingest

Ask your agent to:

1. Read a file from `sources/`.
2. Create a source summary in `wiki/`.
3. Create or update concept and entity pages.
4. Update `wiki/index.md`.
5. Append an entry to `wiki/log.md`. [page:1]

## 6. Inspect in Obsidian

Use backlinks, graph view, and local browsing to verify that the pages are interconnected and readable. Karpathy specifically highlights keeping the LLM open on one side and Obsidian on the other, using Obsidian as the IDE and the LLM as the programmer. [page:1]

## 7. Run lint regularly

Lint the wiki after several ingests. Both the original pattern and current Obsidian implementations describe linting as the mechanism for catching contradictions, dead links, orphans, empty pages, and missing cross-references. [page:1][page:2][web:1]
