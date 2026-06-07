# The Infinite Knowledge Engine — Setup Guide

> A workflow that unifies **Google NotebookLM** and **Obsidian** into a single
> "agent operating system": NotebookLM turns raw sources into many content
> formats, while Obsidian acts as the persistent **memory layer** that grounds
> AI agents in your personal context and proprietary data — organized with the
> **PARA** method — so every generated asset is tailored to your goals.

*Status: written for the state of both products in early–mid 2026. Third-party
tools in this space move fast; verify any specific plugin/extension/repo against
its live page before relying on it. Items that are unofficial or fragile are
flagged inline.*

---

## 1. Why this system exists

Most "research → content" work is fragmented: you read in one tool, take notes
in another, draft in a third, and nothing remembers anything between sessions.
The Infinite Knowledge Engine fixes the two halves of that problem with two
purpose-built tools and a loop that connects them.

| Pillar | Strength | What it does NOT do |
|---|---|---|
| **NotebookLM** | Synthesizes a set of sources into a dozen ready-to-use formats, grounded with citations. | No long-term memory across projects; notebooks are isolated silos. |
| **Obsidian** | Durable, local-first, plain-Markdown memory layer with links, backlinks, and a graph. Owned by you forever. | No built-in source synthesis or media generation. |
| **AI agents (via MCP)** | Read and write the vault, file notes into PARA, link concepts, and extend the knowledge base across sessions. | Only as good as the context the vault gives them. |

The combination: NotebookLM is the **factory**, Obsidian is the **warehouse**,
and agents are the **workers** who keep moving stock between them.

---

## 2. Architecture at a glance

```
                       ┌─────────────────────────────┐
   raw sources ───────▶│        NotebookLM           │
 (PDF, Docs, URLs,     │  source-grounded synthesis  │
  YouTube, audio…)     │  → 12 content formats       │
                       └───────────────┬─────────────┘
                                       │ export (Markdown / media)
                                       ▼
                       ┌─────────────────────────────┐
                       │     Obsidian vault          │
                       │   the MEMORY LAYER (PARA)   │◀───┐
                       │  Markdown + links + graph   │    │ read / write
                       └───────────────┬─────────────┘    │ (MCP)
                                       │ re-aggregate      │
                                       ▼                   │
                       ┌─────────────────────────────┐    │
                       │   AI agents (Claude, etc.)  │────┘
                       │  query, link, summarize,    │
                       │  feed outputs back as new   │
                       │  NotebookLM sources         │
                       └─────────────────────────────┘
```

---

## 3. The synthesis layer — NotebookLM

### What it is
NotebookLM is Google's **source-grounded** research tool. You upload sources and
every output is generated *only* from those sources, with inline citations back
to the passages. It does not pull from the open web at answer time (aside from
optional "Discover sources" / Deep Research features that *find* new sources for
you to add).

### Capacity (verify current numbers at `notebooklm.google/plans`)
- **Sources per notebook:** Free ≈ 50, Plus ≈ 300, Ultra ≈ 600.
- **Per-source size:** up to ~500,000 words / ~200 MB.
- **Notebooks:** Free ≈ 100; paid ≈ 500.
- **Source types:** Google Docs, Slides, Sheets, PDF, `.txt`/`.md`, pasted text,
  web URLs, public YouTube URLs, audio files, Word `.docx`, CSV, and images
  (with OCR). *(Sheets, CSV, Word, and image-OCR were added late 2025.)*

### The content formats (the "twelve")
There is **no official Google list of exactly twelve** formats, and the menu
keeps growing — so this guide names a defensible set of twelve core formats
rather than asserting a count. Treat the list as living.

1. **Audio Overview** — AI podcast-style audio. Sub-modes: *Deep Dive* (2-host),
   *The Brief* (single host, <2 min), *The Critique*, *The Debate*.
2. **Video Overview** — narrated AI video/slides, multiple visual styles, ~80 languages.
3. **Mind Map** — interactive concept map of source connections (exportable PNG).
4. **Briefing Doc** — concise summary of key ideas, findings, and quotes.
5. **Study Guide** — key concepts + short-answer questions + glossary.
6. **FAQ** — Q&A digest of the sources.
7. **Timeline** — chronological list of events drawn from the sources.
8. **Flashcards** — study card sets (topic / difficulty / count configurable).
9. **Quiz** — auto-generated ~10-question quizzes with answers.
10. **Infographic** — single-image visual summary in predefined styles.
11. **Data Tables** — sources transformed into structured tables.
12. **Blog Post** — readable narrative takeaway format.

*Also available:* a **Report** umbrella generator (Briefing Doc, Study Guide,
FAQ, Timeline, Table of Contents, and AI-suggested custom report types all live
under it), saved **Notes** you can pin and reuse as context, and a "Studio
factory" layout that stores multiple outputs of the same type in one notebook.

### Why NotebookLM is NOT your memory
This is the whole reason Obsidian exists in the system:
- **Per-notebook silos** — notebooks cannot reference each other; no cross-notebook
  search or unified knowledge graph.
- **No cross-project memory** — insights don't surface across notebooks; carrying
  them forward is manual.
- **No live sync** — sources are uploaded *snapshots*; they are **not** refreshed
  when the original changes.
- **Source-slot pressure** — because notebooks can't share, you duplicate sources
  and burn limited slots.

In short: NotebookLM is a brilliant **per-project** synthesis workspace, not a
persistent second brain. That role belongs to Obsidian.

### Automation hooks (honest status)
- **No public consumer API** as of early 2026 (Google has said one is "in the
  works"; not shipped).
- **NotebookLM Enterprise API** exists (Google Cloud, ~Sept 2025): notebook CRUD,
  `sources.batchCreate`, `audioOverviews.create`, queries — **enterprise only**,
  not consumer Free/Plus.
- **Unofficial/community** tools exist (e.g. `notebooklm-py`) but are
  reverse-engineered, "not affiliated with Google," and can break anytime. Use at
  your own risk; see §7.

---

## 4. The memory layer — Obsidian

> This guide treats Obsidian as the **canonical, local vault**. Because Obsidian
> is a desktop app that stores plain Markdown files on your own disk, you set it
> up locally — the structure and conventions below are what make it usable as an
> agent-readable memory layer.

### Why Obsidian works as durable memory
- **Local-first plain Markdown.** Each note is one `.md` file in a folder (the
  *vault*). No proprietary database; the data is human- and machine-readable,
  greppable, diffable, and survives any vendor. An agent can read it with nothing
  more than filesystem access.
- **Links & backlinks.** `[[Note Name]]` wikilinks are the core organizing
  primitive; every link auto-creates a bidirectional **backlink**. Relationships
  become a graph an agent can traverse.
- **Graph view.** Visualizes the whole vault (or a note's local neighborhood).

### Organize by actionability: the PARA method
*(Tiago Forte. The source material called it "PAR" — the canonical method is
**PARA**, four categories. Organize by how actionable something is, not by topic.)*

- **Projects** — short-term efforts with a goal, a deadline, and a finish line
  (e.g., "Launch personal site," "Write Q3 research brief").
- **Areas** — ongoing responsibilities with a standard to maintain; they never
  end (e.g., "Health," "Finances," "Content pipeline").
- **Resources** — topics/interests collected for future use, no maintenance
  obligation (e.g., "AI & agents," "Competitor research").
- **Archive** — inactive items from the other three; cold storage, still searchable.

#### Example vault structure
```
MyVault/
├── 00 Inbox/                      # capture / unsorted (incl. fresh NotebookLM exports)
├── 1 Projects/
│   ├── Launch Personal Site/
│   │   ├── Launch Personal Site.md   # project MOC / index note
│   │   └── assets/
│   └── Q3 Research Brief/
│       └── Q3 Research Brief.md
├── 2 Areas/
│   ├── Content Pipeline/
│   ├── Health/
│   └── Finances/
├── 3 Resources/
│   ├── AI & Agents/
│   │   ├── MCP notes.md
│   │   └── Prompt patterns.md
│   └── Competitor Research/
├── 4 Archive/
│   └── 2025 Completed Projects/
└── _meta/                         # support files
    ├── Templates/
    ├── Attachments/
    └── MOCs/                      # Maps of Content (hand-curated hub notes)
```
*(Numeric prefixes are a common convention to force sort order — optional.)*

### Grounding personal & proprietary context
- **YAML frontmatter (Properties).** A metadata block fenced by `---` at the top
  of a note. Built-in keys: `tags`, `aliases`, `cssclasses`; add your own
  (`status`, `client`, `source`, `project`, `created`). Plugins like Dataview and
  Templater query these to build dynamic indexes — and agents can read them to
  understand a note's role. Example:
  ```yaml
  ---
  title: Q3 Research Brief
  tags: [project/active, content]
  status: drafting
  source: notebooklm
  created: 2026-06-07
  ---
  ```
- **Templates** — the core Templates plugin (or community **Templater** for
  dynamic logic) stamps consistent frontmatter for meeting/project/literature notes.
- **Daily Notes** — a dated capture surface (`YYYY-MM-DD.md`) for logs and journaling.
- **MOCs (Maps of Content)** — hand-curated index notes that link out to a topic's
  subtree; ideal as the **entry node an agent reads first** to discover everything
  about a subject. (Community convention, not an official feature.)

### Keeping the vault available everywhere
- **Obsidian Sync** — official, paid, end-to-end encrypted; best conflict handling
  and mobile support.
- **iCloud / Dropbox / Google Drive / OneDrive** — simplest if you already use one;
  weaker conflict handling ("dumb file sync"). *Google Drive doubles as the bridge
  to NotebookLM — see §6.*
- **Git** — great version history and diffs (often via the **Obsidian Git**
  plugin); not automatic, and mobile/merge friction.
- **Syncthing** — open-source P2P, no central cloud; you manage conflicts.

### In-app AI (optional, runs inside Obsidian)
- **Copilot for Obsidian** — chat/RAG over your notes (OpenAI/Anthropic/local).
- **Smart Connections** — local embeddings for semantic "related notes" (no API
  key, no external calls).
- **Text Generator** — template-driven LLM generation into notes.

---

## 5. Turning the vault into an "agent operating system" (MCP)

The point of the memory layer is that **multiple AI agents share it**. The clean
way to do this is the **Model Context Protocol (MCP)** over Obsidian's local API,
so any MCP-capable client — Claude Desktop, Claude Code, Cursor, VS Code,
ChatGPT desktop — operates on the same vault.

### Foundation plugin
- **Obsidian Local REST API** (`coddingtonbear/obsidian-local-rest-api`) — community
  plugin exposing HTTPS on `:27124` (and optional HTTP `:27123`), bearer-token
  auth, and a self-signed cert the client must trust. **v4.0.0+ ships a built-in
  MCP endpoint at `/mcp/`**, so a separate MCP server is increasingly optional.

### Standalone MCP servers (if you want one)
- **`MarkusPfundstein/mcp-obsidian`** — most popular, Python, `uvx`-installed.
  Tools: list files, read contents, search, `patch_content`, `append_content`,
  `delete_file`.
- **`cyanheads/obsidian-mcp-server`** — 14 tools: get/list/search notes, write/
  append/patch (surgical edits to a heading/block/frontmatter), manage frontmatter
  and tags, delete (with optional human confirm), open-in-UI, run commands.
  Folder-scoped permissions; multi-mode search. Requires Local REST API v4.0.0+.

### What this enables
Agents get full CRUD on notes, surgical section edits, tag/frontmatter management,
and full-text/structured search. The vault becomes **compounding context**: agents
write session logs, decisions, and summaries back into it and re-read them next
time, instead of being re-primed from scratch every chat. (This is a workflow
pattern you adopt, not a magic built-in feature.)

> **Security note:** an MCP server exposes vault read/write to local agents. Keep
> the bearer token private, scope folders where supported, and prefer
> human-confirm on destructive tools.

---

## 6. The bridge — moving data between NotebookLM and Obsidian

There is **no official consumer API**, so the bridge is a mix of "works today"
and "manual workaround." Be clear-eyed about which is which.

### NotebookLM output → Obsidian (✅ works today, semi-manual)
- **Browser-extension export** adds a real download button to NotebookLM and emits
  Markdown — e.g. "NotebookLM to LaTeX & MD," "NotebookLM Export Pro,"
  "Web Clipper for NotebookLM," "NotebookLM Ultra Exporter." Point the download
  directory at your vault's `00 Inbox/` and the export becomes a note instantly.
- **Copy-paste** as a fallback (NotebookLM output is already largely Markdown).
- **Media outputs** (Audio/Video Overviews, Mind Map PNG) download as media files,
  not text — drop them in `_meta/Attachments/` and link them; transcribe audio
  separately if you want searchable text.
- ⚠️ Extensions scrape an undocumented UI and break when Google changes it.

### Obsidian → NotebookLM as sources (✅ works today, manual re-sync)
- **Direct upload:** select `.md` files from the vault and drag them into
  NotebookLM. (Markdown upload strips images/tables — export the note to PDF first
  for rich notes: `Cmd/Ctrl+P → Export to PDF`.)
- **Google Drive as intermediary:** sync the vault into Drive, then add the Drive
  files as NotebookLM sources.
- **Aggregation pattern (popular):** a Google Apps Script (on a timer) concatenates
  all `.md` in a Drive folder into one Doc; NotebookLM points at that single
  source. The plugin **`romular21/obsidian-notebooklm`** packages this (watches
  vault folders, aggregates `.md` into a Google Doc, auto-updates the Doc).
- ⚠️ **The irreducible manual step:** NotebookLM snapshots a source when added and
  does **not** auto-refresh. After the vault changes you must click **Sync** /
  re-add in the NotebookLM UI. This is the single biggest break in any "live" loop.

---

## 7. The automated loop, end-to-end

The cycle, with each step honestly labeled **auto** / **manual**:

1. **Capture sources → NotebookLM.** *Manual* in the UI. *Automatable* only via
   the unofficial CLI `teng-lin/notebooklm-py` (`notebooklm create`,
   `source add …`).
2. **Synthesize into formats** (briefing, quiz, flashcards, mind map, audio,
   video…). *Manual* in the UI; `notebooklm-py` can `generate` these headlessly.
3. **Extract outputs.** *Semi-manual* via browser-extension export / copy-paste;
   `notebooklm-py download` is the strongest lever (reports → Markdown,
   flashcards/quiz → JSON/MD, mind map JSON, audio MP3, video MP4).
4. **Land in the PARA vault.** *Trivially automatable* — write files into the vault
   folder (e.g., `00 Inbox/`); an agent files them into PARA.
5. **Agents query & extend.** *Automatable* via MCP (§5) — read, search, write
   summaries, create links, update frontmatter/MOCs.
6. **Feed back.** Agent/NotebookLM outputs become new vault notes → re-aggregated
   → re-added/re-synced as NotebookLM sources → back to step 1.

> A **fully closed** automated loop requires accepting the unofficial
> `notebooklm-py` (Playwright/cookie auth) for the NotebookLM legs. With **official
> tooling only, steps 1–3 stay manual** — which is perfectly workable; you just run
> the loop by hand at the NotebookLM end and let MCP/agents automate the vault end.

**Reliable-today core (recommended starting point):** MCP read/write of the vault
+ manual `.md`/PDF upload to NotebookLM + browser-extension Markdown export back
into `00 Inbox/`. Add the unofficial CLI later only if you need full automation
and accept its fragility.

---

## 8. Setup checklist

**A. Stand up the memory layer (Obsidian)**
1. Install Obsidian; create a vault (e.g. `MyVault`).
2. Create the PARA folders from §4 (`1 Projects`, `2 Areas`, `3 Resources`,
   `4 Archive`, `00 Inbox`, `_meta`).
3. Enable core plugins: **Templates**, **Daily Notes**, **Graph view**,
   **Backlinks**.
4. Add a project-note template with frontmatter (§4) under `_meta/Templates/`.
5. Pick a sync method (§4) — Obsidian Sync, or Google Drive if you want the
   NotebookLM bridge for free.

**B. Make the vault agent-readable (MCP)**
6. Install the **Local REST API** plugin; copy the bearer token; trust the cert.
7. Either use its built-in `/mcp/` endpoint (v4.0.0+) or install a standalone
   server (`mcp-obsidian` or `obsidian-mcp-server`).
8. Register the MCP server in your agent client (Claude Desktop/Code, Cursor, …)
   and confirm the agent can list and read notes.

**C. Wire the synthesis layer (NotebookLM)**
9. Create a notebook; add your sources.
10. Install a NotebookLM export extension; set its download dir to the vault's
    `00 Inbox/`.
11. (Optional, advanced/fragile) set up `romular21/obsidian-notebooklm` for
    Drive aggregation, and/or `notebooklm-py` for headless generation.

**D. Run the loop**
12. Capture → synthesize in NotebookLM → export to `00 Inbox/` → have an agent
    file notes into PARA, link them, and write a summary/MOC → feed strong outputs
    back as new sources → repeat. Re-sync NotebookLM sources manually after vault
    changes.

---

## 9. Limitations & caveats (read before you commit)

- **No supported consumer NotebookLM API.** Everything programmatic on the
  NotebookLM side is either enterprise-only or unofficial/reverse-engineered and
  may break or raise TOS/account-auth concerns.
- **No auto-refresh of NotebookLM sources** — manual re-sync is unavoidable today.
- **Non-text outputs** (audio/video) don't become clean vault text without
  transcription.
- **Markdown fidelity loss** on upload to NotebookLM (images/tables stripped)
  unless you route via PDF/Google Doc.
- **Third-party churn** — export extensions and MCP servers change ports, names,
  and capabilities; re-verify before depending on them.
- **MCP exposes vault CRUD** to local agents — manage the token and permissions.
- **Tier numbers** (source/notebook caps) vary across sources — confirm on the
  live pricing page.

---

## 10. References

**NotebookLM**
- Add or discover sources — https://support.google.com/notebooklm/answer/16215270
- Deep Research & new file types (Google blog) — https://blog.google/innovation-and-ai/models-and-research/google-labs/notebooklm-deep-research-file-types/
- Video Overviews & Studio upgrades (Google blog) — https://blog.google/innovation-and-ai/models-and-research/google-labs/notebooklm-video-overviews-studio-upgrades/
- Data Tables (Workspace Updates) — https://workspaceupdates.googleblog.com/2025/12/transform-sources-structured-data-tables-notebooklm.html
- New content customization, Mar 2026 (Workspace Updates) — https://workspaceupdates.googleblog.com/2026/03/new-ways-to-customize-and-interact-with-your-content-in-NotebookLM.html
- Enterprise API (Google Cloud docs) — https://docs.cloud.google.com/gemini/enterprise/notebooklm-enterprise/docs/api-notebooks
- Does NotebookLM have an API — https://autocontentapi.com/blog/does-notebooklm-have-an-api

**Obsidian & PARA**
- Properties / frontmatter — https://help.obsidian.md/properties
- PARA method (Forte Labs) — https://fortelabs.com/blog/para/
- Local REST API plugin — https://github.com/coddingtonbear/obsidian-local-rest-api
- `cyanheads/obsidian-mcp-server` — https://github.com/cyanheads/obsidian-mcp-server
- `MarkusPfundstein/mcp-obsidian` — https://github.com/MarkusPfundstein/mcp-obsidian

**Bridge / automation**
- `romular21/obsidian-notebooklm` — https://github.com/romular21/obsidian-notebooklm
- `teng-lin/notebooklm-py` (unofficial) — https://github.com/teng-lin/notebooklm-py
- Apps Script sync method — https://www.makeuseof.com/obsidian-notebooklm-real-sync-powerful/
- NotebookLM → Obsidian Markdown (XDA) — https://www.xda-developers.com/notebooklm-to-obsidian-markdown/
