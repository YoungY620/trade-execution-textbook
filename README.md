# From Signal to Execution — a textbook of optimal trade execution

How an investment view, or a piece of private still-underwater information,
becomes an actual trade: the theory, the history, the competing schools, the
differences between markets and situations, and the measurement loop that closes
it all. Bilingual (English at the site root, Chinese under `/zh/`).

**Read it: <https://youngy620.github.io/trade-execution-textbook/>**
中文版：<https://youngy620.github.io/trade-execution-textbook/zh/>

## Chapters

**Foundations** — 1 From signal to execution: the whole stack · 2 A short
history: from the floor to the algorithm · 3 Market microstructure: where
execution cost comes from

**Cost and measurement** — 4 Implementation shortfall and transaction cost
analysis

**The theory of optimal execution** — 5 The Almgren–Chriss framework ·
6 Beyond Almgren–Chriss: robustness, supply and demand, stochastic control ·
7 Market making and the stochastic-control view

**Micro-level execution** — 8 Order books, order types and stealth ·
9 Order routing and market fragmentation

**Learning and adaptation** — 10 Reinforcement learning and adaptive execution

**Scenarios** — 11 Cryptocurrency markets · 12 Scenarios side by side

**Practice** — 13 Measurement, calibration and the engineering loop ·
14 The literature: a full map and a reading path

Plus a shared [glossary](src/glossary.md) and a
[references and source material](src/sources.md) page listing everything
consulted — including what could not be obtained, and why.

## What is in here, and where it came from

Every claim is traced to a document that was actually read. The primary
literature is open access (arXiv, author pages, university repositories, official
regulatory filings). Where a work exists only as a commercial book, the chapters
rest on the author's own public lecture notes, course manuscripts, surveys and
publisher-authorised previews — and the source page says so, book by book.

Nothing in this book is written from an abstract alone, and commercial textbooks
were not obtained in full; the source page records exactly which substitute was
used for each, and which works could not be obtained at all.

## Layout

```
src/                       English book (site root)
zh/src/                    Chinese book (published under /zh/)
glossary.yml               glossary source of truth, merged from per-chapter fragments
tools/                     math protection, math restore, glossary build/merge
theme/head.hbs             MathJax 4 configuration
WRITING-GUIDE.md           the contract between chapter writers and the build
```

## Notes on the build

- **Math.** Markdown is parsed before MathJax runs, and that pass destroys TeX
  that matters here (`\{`, `_` after `}`, `\\` inside `aligned`).
  `tools/protect_math.py` is an mdBook preprocessor that replaces every math
  span with an opaque token before Markdown sees it; `tools/restore_math.py`
  restores it in the built HTML and in the search index. MathJax 4 is loaded
  with `output.displayOverflow: "linebreak"` so long display formulas wrap on
  narrow screens; long *inline* formulas cannot wrap, so they are written as
  display math instead.
- **Search.** mdBook's index keeps only ASCII tokens, so Chinese is not
  searchable out of the box. `bd-cjksearch.js` answers CJK queries by scanning
  the index's document store directly.
- **Glossary.** Per-chapter YAML fragments are merged into `glossary.yml`, which
  generates the appendix page and the hover-card data; the runtime wraps the
  first occurrence of each term on a page.

Build locally:

```sh
pip install pyyaml
bash build.sh          # -> _site/
```

---

Produced with the **course-textbook-builder** workflow
(<https://github.com/YoungY620/my-awesome-skills>).
