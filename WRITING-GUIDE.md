# Writing guide — *From Signal to Execution*

Read this before writing a chapter. It is the contract between the chapter
writers and the build.

## What you are producing

Two parallel files per chapter, one English and one Chinese, for an mdBook
textbook published as a website.

- English: `<repo>/src/<file>.md`
- Chinese: `<repo>/zh/src/<file>.md`

The file names are identical on both sides. Both files must be complete and
self-contained; the Chinese file is **not** a line-by-line translation of the
English one. Write each as if it were the original: use the terminology and
sentence rhythm a Chinese technical reader expects, and keep the same technical
content, section order and heading depth.

`<repo>` is `/Users/moonshot/dev/working/trade-execution-textbook`.

## Sources — and only sources

This book is a **synthesis**, so unlike a paper reading there is no single
authoritative text. What you may use is:

- `materials/notes/cluster-*.md` — dense English extractions written by
  earlier readers, one section per work. These are your map.
- `materials/papers/<cluster>/*.pdf` — the full texts those notes came from.
  **Read the PDF for anything you state precisely.** Every formula, theorem
  statement, parameter value and number you put on the page must be checked
  against the PDF itself (`pdftotext` the relevant pages), not taken from a
  note. Notes are a finding aid; the paper is the evidence.
- Official documents and author lecture notes, as recorded in the notes files.

Rules:

- Do not import outside knowledge. Do not invent numbers, dates, mechanisms or
  citations. If a claim you want to make is not supported by the material,
  do not make it — and say so in your report.
- Do not soften or strengthen what a source claims. If a source is uncertain,
  stay uncertain; if two sources disagree, say so and give both.
- Where the prompt for your chapter names a claim that must be verified (for
  example a widely repeated saving figure), report explicitly whether you could
  verify it and where.
- Commercial books (O'Hara; Hasbrouck; Harris; Cartea–Jaimungal–Penalva;
  Guéant; Lehalle–Laruelle; Johnson; Kissell) were **not** obtained. Where a
  chapter needs their content, it must rest on the authors' own public papers,
  lecture notes and surveys that are in `materials/`, and say so. Never
  describe a book's contents from memory.

## Structure

- One `#` title per chapter, matching the title in `SUMMARY.md` exactly.
- Use `##` and `###` for internal structure.
- End every chapter with a final `## Key points` section (Chinese:
  `## 本章要点`) holding 5–8 bullets, each a complete sentence stating a
  conclusion the chapter established.
- Immediately before Key points, add `## Sources` (Chinese: `## 本章依据的文献`):
  a short list of the works this chapter actually drew on, each with author,
  year, title and a link where one exists.
- Length: 1400–3800 words of English; 2200–6500 characters of Chinese. This is a
  synthesis textbook, so chapters that carry many works run long by design; the
  floor matters more than the ceiling. Dense is good. Do not pad, and do not
  open with "in this chapter we will".

## Mathematics

`$…$` for inline math and `$$…$$` for display math, with display math on its own
lines separated by blank lines. A preprocessor protects the math from the
Markdown parser, so `\\`, `\{`, `_`, `&` and `^` inside math are safe — write
real TeX.

Two rules:

- **Never use a bare `$` for a currency amount.** Write `USD 5`, `5 USD`, or
  `5 dollars`. A bare `$` becomes a math delimiter.
- Inline math must not contain a blank line, and must not start or end with a
  space.

`\coloneqq`, `\textsc{}`, `\mathcal{}`, `\mathbb{}` and `\operatorname{}` are
available (see `theme/head.hbs` for the full macro list). Follow the source's
notation; do not re-letter a source's symbols.

## Figures and tables

This book has no figures of its own yet. Do not reference an image unless it
already exists in `src/assets/`. Re-typeset tables from a source as Markdown
tables and check every cell against the PDF.

## Glossary

Each writer appends the terms it introduced to **its own** fragment file, so
concurrent writers never touch the same file:

- `materials/glossary/<chapter-file-name>.yml`

Format, one list entry per term:

```yaml
- term: Implementation Shortfall
  term_zh: 实施差额
  pinyin: shi
  brief: One sentence, at most 140 characters, explaining the term in plain words.
  brief_zh: 一句话释义，不超过 60 字。
  detail: Optional extra paragraph shown in the glossary appendix.
  detail_zh: 可选的补充段落。
  chapter: "4.2"
  first_seen: Chapter 4
```

`pinyin` is the pinyin of the first Chinese character, used to group the Chinese
glossary alphabetically. Pick 6–12 terms per chapter — the ones a reader could
not be expected to know, not every noun. Terms are matched case-insensitively in
the rendered page and each is linked at most once per page, so short common
words are a bad choice; use the full term ("implementation shortfall", "adverse
selection", "dark pool").

The main agent merges the fragments afterwards.

## Voice

- Plain technical prose in complete sentences. Use a list only where the source
  enumerates, or where the chapter is a comparison table by nature.
- No emoji, no exclamation marks, no "in this section we will…", no "it is worth
  noting that", no closing paragraph that repeats the chapter.
- Bold only for a term being defined.
- Do not write "the paper says"; write the content. Attribute only when
  provenance matters (who argued what, which venue did what, a correction).
- Explain the mechanism, not just the result. A reader wants to be able to
  re-derive or apply what you wrote.

## Ground rules

- Write only your two chapter files and your glossary fragment. Do not touch
  `SUMMARY.md`, `book.toml`, other chapters, `glossary.yml`, or shared JS/CSS.
- Do not run `mdbook build`; the main agent builds and verifies.
- Report when done: the two file paths, word/character counts, and any place
  where a source was ambiguous, looked wrong, or was missing.
