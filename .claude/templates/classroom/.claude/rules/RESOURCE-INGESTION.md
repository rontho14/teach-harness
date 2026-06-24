# Resource ingestion — turning a book into an agent-readable OKF bundle

When the learner provides a **book or other long-form source** (a PDF, EPUB, DOCX,
HTML, plain text, or markdown file), it must be ingested before it is used to
teach. The goal: the tutor should be able to cite and draw from the book **without
ever reading the whole file** — it navigates a structured bundle and opens only the
chunk it needs. We use **Google's Open Knowledge Format (OKF)** as that structure.

> Reference: *How the Open Knowledge Format can improve data sharing* —
> https://cloud.google.com/blog/products/data-analytics/how-the-open-knowledge-format-can-improve-data-sharing

## Hard rules

- **Markdown only.** Every ingested source ends up as markdown. If the learner
  supplies markdown, use it. Otherwise **convert it to markdown first** (see below).
- **Never read the whole book to teach.** After ingestion the tutor reads the
  bundle's `index.md`, then opens only the specific concept file(s) a lesson /
  exercise / exam needs. Loading an entire book into context is a process error.
- **Never alter or invent content.** Conversion preserves the author's text,
  headings, tables, and figures' captions. If conversion comes out garbled or
  lossy, **flag it to the learner and ask for a cleaner copy** — do not teach from
  corrupted text, and do not paper over gaps from memory.
- **One concept per file.** Chunk along the book's natural structure so each file
  is bounded and separately loadable.

## Step 1 — get it into markdown

- Already `.md` → keep it.
- `.docx` / `.epub` / `.html` → convert with `pandoc` (e.g.
  `pandoc input.epub -t gfm -o out.md`), which keeps headings, lists, and tables.
- `.pdf` → use a layout-aware extractor that preserves headings and tables
  (e.g. `pymupdf4llm`, `marker`); if only raw text is available, reconstruct the
  heading hierarchy carefully. Verify a few pages against the source before trusting
  the whole conversion.
- Keep the raw converted markdown out of the way (e.g. a `_raw/` subfolder); the
  OKF bundle in Step 2 is what the tutor actually uses.

## Step 2 — restructure into an OKF bundle

Create the bundle under **`resources/<book-slug>/`**. OKF is *just markdown files in
a directory*, one concept per file, with a hierarchical path as identity:

```
resources/how-to-measure-anything/
├── index.md                      # map of the whole book (read FIRST)
├── 01-intro/
│   ├── index.md                  # part/chapter overview
│   └── measurement-as-uncertainty-reduction.md
├── 02-calibration/
│   ├── index.md
│   ├── calibrated-probability-assessment.md
│   └── equivalent-bet-test.md
└── ...
```

### Each concept file
- **YAML frontmatter** (OKF: only `type` is required; include the rest when known):
  ```yaml
  ---
  type: Book Section            # e.g. "Book Chapter" | "Book Section" | "Concept"
  title: The Equivalent Bet Test
  description: One- to two-line summary of what this chunk covers (used for the index).
  tags: [calibration, estimation]
  resource: "Hubbard, How to Measure Anything, 3rd ed., ch. 5"  # original location
  timestamp: 2026-06-20T00:00:00Z
  ---
  ```
- **Markdown body** = the actual prose of that chunk, headings/tables preserved.
- **Cross-link** to related concept files with relative markdown links
  (`see [calibration](../02-calibration/calibrated-probability-assessment.md)`),
  turning the bundle into a navigable knowledge graph.

### `index.md` (progressive disclosure)
- Bundle-root `index.md`: full citation of the book + a table of contents that
  links to each concept file (or sub-`index.md`) with its one-line `description`.
  **This is the map the tutor reads first.**
- Each subdirectory gets its own `index.md` overview so the tutor can drill down a
  level at a time instead of scanning everything.

### Chunk sizing
- Target one **section** per file (split a long chapter at its H2s). Small enough to
  load a single idea; large enough to be self-contained. A weak/needed sub-concept
  should be reachable by opening one file.

## Step 3 — register it

- Add the book to `RESOURCES.md` under **Knowledge**, linking to the bundle's
  `index.md` (not the raw file), with a one-line "use for" note. See
  `RESOURCES-FORMAT.md`.
- When a lesson/exercise/exam cites the book, cite the **specific OKF concept file**
  plus the original location (chapter/page) — same integrity bar as all sources
  (see `QUESTION-RESEARCH.md`, `EXAM-RESEARCH.md`).
