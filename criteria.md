# Writing Criteria

Every guide in this repo is written to the same
standard. If you're adding or editing a guide —
human or AI — hold it to these.

1. **Plain words, real ideas.** Simple language,
   never dumbed-down concepts. Explain jargon on
   first use; collect terms in the glossary.
2. **No hype.** No marketing adjectives, no
   "revolutionary," no promises about what
   something will do for the reader.
3. **Honest about limits.** Say what the thing
   *can't* do. State uncertainty instead of
   smoothing it over. Validation notes include
   what remains unverified.
4. **No invented numbers.** Benchmarks, defaults,
   and specs are either measured or qualified with
   "depends on your setup." If it wasn't tested,
   say so.
5. **Checkable.** Commands are meant to be run,
   with "Check:" blocks showing what success
   looks like. Sources are linked, not asserted.
6. **Respect the reader's setup.** Call out
   Windows/Mac/Linux differences. Don't assume a
   GPU, a budget, or prior experience.
7. **Safety isn't skipped.** Real risks get
   stated plainly, not buried.
8. **Scoped.** Each guide says what it covers and
   points elsewhere for the rest. No guide
   pretends to be the whole subject.
9. **Structure over prose.** Tables, before/after
   examples, symptom → fix. Scannable beats essay.

## Verification notes for this guide

Standards 3 and 4 applied to this guide,
specifically. **pandoc 3.1.3** (the Ubuntu
build, `3.1.3+ds-2`, scripting engine Lua 5.4)
was installed via apt on the build machine
and ran every conversion; PDF proof used
**WeasyPrint 61.1** and **wkhtmltopdf
0.12.6** (Qt 5.15.13) as engines and
poppler's `pdfinfo`/`pdftotext` as
instruments. Working transcripts are kept
with the build (`pandoc-lab/transcripts/`,
01–13, mirroring the chapters, with
appendix captures on 02, 07, and 10).

**The lab.** One fixture document:
`agenda.md` (1,153 bytes) — an invented
garden club agenda with YAML front matter
(title, author, date), an H1 and three H2s,
a 4×3 pipe table, a bulleted list, a
numbered list, one link (to example.org),
one footnote, and one image. The image,
`chart.png` (344 bytes, 160×100), was drawn
pixel by pixel with a Python standard-library
script (three bars on an axis) so the lab
needs no downloads and no one's artwork.
The author is an invented person and the
table's owners are role titles (Chair,
Secretary, Treasurer, Coordinator); there
is no real person's data in the lab. The
scripts that run the lab are kept with it
(`run-lab.sh`, `run-lab2.sh`, `run-lab3.sh`).

**Verified by running, per chapter:**

- *Ch. 1:* `pandoc --version` (first three
  lines quoted); `--list-input-formats` and
  `--list-output-formats` counted on this
  build: **43 in, 64 out**; `pdf`, `native`,
  `json`, `revealjs`, and `typst` all present
  in the output list (the `pdf` entry is the
  engine promise ch. 9 cashes).
- *Ch. 2:* the default conversion prints a
  fragment (begins `<h1 id="agenda">`, no
  `<html`); `-f markdown -t html5` identical
  in kind; `-o agenda-fragment.html` writes
  1,883 bytes; the `.txt` copy reads as
  Markdown (fallback), the `.html`-mislabelled
  copy reads as HTML and produces the
  backslash-escaped wreckage quoted in the
  chapter; appendix captures add
  `-t native` (this build prints the list
  form: `[ Header 1 ( "agenda" , [] , [] )
  [ Str "Agenda" ] …`) and a stdin pipe
  (`printf … | pandoc` → fragment).
- *Ch. 3:* `-s` page (5,841 B) and `-s --toc`
  page (6,188 B); greps prove `<title>Spring
  Planting Planning Meeting</title>`, the
  `<h1 id="agenda">` anchor, the
  `<nav id="TOC">` block linking `#schedule`,
  and exactly 1 `<table`.
- *Ch. 4:* the front matter's title in both
  `<title>` and `<h1 class="title">`;
  `--metadata title=…` overrides it in the
  output only (grep proves the new title;
  the original page still greps the old);
  `--metadata subtitle="Working draft"`
  produces the `<p class="subtitle">` line.
- *Ch. 5:* `agenda.docx` at 12,055 B, `file`
  reporting Microsoft Word 2007+;
  `unzip -l` shows **17 parts, 56,226 bytes**
  uncompressed; cell texts "Compost
  delivery deadline" and "Plot assignment
  sign-ups" greped from
  `word/document.xml`; `<dc:title>` and
  `<dc:creator>` greped from
  `docProps/core.xml` (the front-matter
  *date* is not written there — created and
  modified carry the build timestamp, a
  limit the chapter does not claim past);
  the footnote text greped from
  `word/footnotes.xml` (a real Word
  footnote); the image extracted from
  `word/media/rId24.png` and `cmp`-verified
  **byte-identical** to `chart.png`.
- *Ch. 6:* the DOCX → Markdown round trip
  quoted from `agenda-roundtrip.md` and the
  captured `diff` (exit 1): front matter
  absent; table complete as a **grid
  table** with wrapped columns; footnote
  present, renumbered `[^1]`; image present
  as `media/rId24.png` with
  `{width="2.2222222222222223in"
  height="1.3888888888888888in"}` attributes;
  caption repeated as a following paragraph.
  No sentence of the agenda is missing —
  the diff's hunks are exactly the ones the
  chapter enumerates.
- *Ch. 7:* the HTML rescue quoted from
  `agenda-from-html.md`: `::: {#title-block-header}`
  div with the title as an attributed
  heading; the TOC as a link list with
  `{#toc-agenda}` attributes; the table as
  a simple table, content complete; the
  footnote reference carrying
  `{#fnref1 .footnote-ref role="doc-noteref"}`;
  the figure as raw `<figure>` HTML;
  appendix: the same rescue on the bare
  fragment — `title-block` greps **0**, the
  TOC list absent, and the footnote/figure
  residue unchanged (the improvement is
  structural, and the chapter grades it as
  such).
- *Ch. 8:* `agenda.epub` at 6,299 B, `file`
  reporting EPUB document; `unzip -l` shows
  **10 files**, `mimetype` first and the
  image at `EPUB/media/file0.png` (344 B);
  `<dc:title id="epub-title-1">Spring
  Planting Planning Meeting</dc:title>`
  greped from `EPUB/content.opf`; the
  compost line greped from
  `EPUB/text/ch001.xhtml`.
- *Ch. 9:* the un-engined conversion fails
  as captured — `pdflatex not found. Please
  select a different --pdf-engine or install
  pdflatex`, **exit 47**; `typst` is absent
  from this machine's apt sources
  (`E: Unable to locate package typst`,
  captured by dry-run). **WeasyPrint run:**
  16,507 B, PDF 1.7, 2 pages A4, `pdfinfo`
  Producer "WeasyPrint 61.1" with Title and
  Author carried from the front matter;
  `pdftotext` finds the compost line
  (count 1) and the title block (the title
  on a single printed line, quoted as
  extracted). Its CSS warnings (four kinds,
  quoted in part) are WeasyPrint declining
  pieces of pandoc's default stylesheet.
  **wkhtmltopdf run:** 30,440 B, PDF 1.4, 2
  pages, Producer "Qt 5.15.13", **Title
  empty** in `pdfinfo`; compost line count
  also 1; Qt progress chatter on stderr,
  noted not quoted.
- *Ch. 10:* the reveal.js deck at 8,937 B;
  `class="reveal"` greps; `<section` counts
  3, ids `title-slide`, `agenda`,
  `footnotes`. Appendix capture proves the
  framework is linked, not contained:
  `https://unpkg.com/reveal.js@^4//dist/reveal.js`
  and siblings (CSS, theme, plugins).
- *Ch. 11:* the plain page's
  `<img src="chart.png"`; `--embed-resources`
  produces `src="data:image/png;base64`;
  the size inversion is the capture's own:
  embedded 5,380 B vs plain 5,841 B, with
  both effects (base64 added ≈ +460 chars;
  re-serialisation dropping the template's
  indentation) stated as the explanation —
  the first is arithmetic on the fixture,
  the second is visible in the files'
  formatting, and neither is generalised
  past what the lab shows. The media trap:
  the round trip references `media/rId24.png`
  while `ls media` fails (captured);
  `--extract-media=extracted` writes
  `extracted/media/rId24.png`, 344 B,
  `cmp` IDENTICAL, and the output's
  reference rewritten to match.
- *Ch. 12:* the scorecard is ch. 6–8's
  evidence tabulated, nothing new asserted.
  The tracked-changes experiment
  (`run-lab2.sh`): `agenda.docx` was
  unzipped, its XML edited by a kept script
  (`docx-edit/edit.py`) to add a tracked
  insertion (`<w:ins … author="A. Reviewer">`,
  "Order more twine for the bean poles.") and
  a comment ("Did anyone confirm the
  projector takes HDMI?") anchored to the
  projector line, then rezipped (14,022 B;
  `file`: Microsoft OOXML). Default
  conversion: insertion grep count **1**,
  comment grep count **0**.
  `--track-changes=reject`: insertion count
  **0**. `--track-changes=all`: both texts
  present, as `{.insertion …}` and
  `{.comment-start …}` / `{.comment-end}`
  spans carrying author and date, quoted
  verbatim from `tracked-all.md`.
- *Ch. 13:* the defaults file is quoted
  verbatim; `-d club.yaml` output
  greps the front-matter title and counts
  1 `id="TOC"`; the batch loop converts
  `agenda.md` and `reminder.md` to DOCX
  (12,055 B and 9,902 B).

**Capture hygiene (declared trims):**
chapters quote the lines a claim rests on
and elide the rest with `…` or `...`;
`unzip -l` listings are trimmed to the parts
discussed, with totals kept. Conversion
commands are occasionally re-sequenced into
teaching order (all quoted output is the
final lab run's own). The harness echoes
commands into transcripts without shell
quoting, so transcript command lines drop
quote marks around multi-word arguments;
chapters print the same commands in the
reader's form. ZIP entry timestamps are the
lab runs' own and will differ on yours; the
sizes quoted are stable for this fixture
and pandoc build except where zip metadata
drifts them by a byte or two (the EPUB
measured 6,301 B on a repeat run and
6,299 B on the lab run itself — the lab
run's number is the one the chapter
prints).

**Documented-only (not run), and why:**

- *Styling depth:* custom templates,
  `--css`, and the `--reference-doc`
  workflow. Named in ch. 3 and ch. 5; their
  captures would record this machine's
  taste, not a checkable behaviour, so the
  lab standardised on a fresh install's
  defaults everywhere.
- *The LaTeX PDF family* (`pdflatex`,
  `xelatex`, `lualatex`, ConTeXt) **and
  beamer**: no TeX is installed, and
  installing one to force a capture would
  cost gigabytes to prove what the captured
  exit-47 error already states honestly.
- *typst as a PDF engine:* not packaged in
  this machine's apt sources (captured);
  on a machine that has it, it is one
  `--pdf-engine` flag, same as the two
  engines run here. Pandoc's `typst`
  *writer* is likewise named nowhere as
  taught — it was not exercised.
- *Bibliographies and citations*
  (`--citeproc`, CSL): a real pandoc
  subsystem, named in the README's scope
  and left for want of a citation fixture,
  not for any doubt it works.
- *Lua filters and programmatic document
  surgery:* named in the README's scope;
  the guide's own tracked-changes
  experiment is the closest it comes, and
  that is lab scaffolding, not a taught
  workflow.
- *The other writers* (ODT, PPTX, RTF, the
  wiki and outliner formats, and the rest
  of the 64): not exercised. The grammar in
  ch. 2 and the inspection habits transfer;
  their individual fidelities are not
  claimed.
- *Self-hosting the reveal.js assets* (a
  local `revealjs-url`): the default's CDN
  link is captured instead (ch. 10
  appendix).
- *Installation on other platforms* is
  described, not performed — pandoc was
  installed once here, via apt.

Specimen discipline, for the record: the
lab's garden club is invented, its author
is an invented person, its owners are role
titles, its venue is nowhere, and its
one link points at example.org. The chart
is generated locally; the tracked-changes
reviewer "A. Reviewer" is a string the lab
typed into XML it manufactured and says so.
Nothing in the guide required the network
beyond the one-time package installs.
