# LLM Wiki in Obsidian

A starter GitHub repo that documents how to build a Karpathy-style **LLM Wiki** in Obsidian for note-taking.

This workflow uses three layers: immutable raw sources, an LLM-maintained wiki of markdown pages, and a schema file that tells the agent how to ingest, query, and lint the vault. The pattern comes from Andrej Karpathy's LLM Wiki gist and is reflected in current Obsidian implementations and tutorials. [page:1][page:2][web:1]

## What this repo is for

Use this repo if you want to:
- keep notes in Obsidian,
- let an LLM build and maintain cross-linked wiki pages,
- ingest articles, papers, PDFs, and transcripts one source at a time,
- query your own knowledge base instead of relying only on classic RAG, and
- run periodic wiki maintenance checks such as dead-link, duplicate, orphan, and contradiction review. [page:1][page:2]

## Folder structure

```text
.
├── README.md
├── SETUP.md
├── WORKFLOW.md
├── OBSIDIAN.md
├── CLAUDE.md
├── .gitignore
├── .github/
│   └── workflows/
│       └── markdown-lint.yml
├── schema/
│   └── README.md
├── sources/
│   └── README.md
└── wiki/
    ├── README.md
    ├── index.md
    └── log.md
```

This mirrors the three-layer architecture described by Karpathy: `sources/` for raw documents, `wiki/` for generated knowledge pages, and a schema file such as `CLAUDE.md` that governs the agent's behavior. [page:1]

## Quick start

1. Install Obsidian and create or open a vault. The tutorial video explicitly walks through creating the vault, setting up folders, writing the schema file, ingesting a first document, exploring graph view, and linting the wiki. [web:1]
2. Put this repo inside the vault root, or use this repo itself as the vault.
3. Add your source notes and clips to `sources/`.
4. Open `CLAUDE.md` and adapt the rules to your domain.
5. Use your coding agent to ingest one source at a time into `wiki/`.
6. Open Obsidian graph view and browse the generated links.
7. Periodically run a lint pass to detect duplicates, dead links, missing aliases, or orphan pages. [page:1][page:2][web:1]

## Core operations

Karpathy's pattern centers on three operations:

- **Ingest**: read a new source, write a summary page, update entity/concept pages, update `wiki/index.md`, and append to `wiki/log.md`. One source may touch 10 to 15 pages. [page:1]
- **Query**: ask questions against the existing wiki, then save valuable answers back into the vault as new synthesis pages so the knowledge base compounds over time. [page:1]
- **Lint**: health-check the wiki for contradictions, stale claims, missing cross-references, orphan pages, and knowledge gaps. [page:1]

Current Obsidian plugin implementations expose the same core actions as commands such as Ingest single source, Ingest from folder, Query wiki, Lint wiki, and Regenerate index. [page:2]

## Recommended tools

- **Obsidian** for the vault, graph view, backlinks, and markdown browsing. [page:1][web:1]
- **Obsidian Web Clipper** for quickly converting web articles into markdown sources. [page:1][web:1]
- **Claude Code, Codex, or another agentic coding tool** to read sources and maintain the wiki according to `CLAUDE.md`. [page:1]
- **GitHub** for version control, collaboration, and history, since the wiki is ultimately a git repo of markdown files. [page:1]

## Notes

This starter repo documents the process and provides templates. It is not tied to a single plugin, because Karpathy's original gist is intentionally abstract and designed to be adapted to your own tools and preferences. [page:1]
