# Pandoc for Mere Mortals

You wrote it once. Now your group wants
it as a Word document, the website wants it
as a page, the record wants it as a PDF, and
someone who reads on a tablet wants an ebook.
The usual way to satisfy them is to reformat
by hand, four times, and then keep four
copies in step forever. **Pandoc** is the way
out: a single command-line converter that
reads a document in one format and writes it
in another — 43 input formats and 64 output
formats on the build machine — by passing
everything through one internal model of a
document.

This guide teaches pandoc the way it is
actually used: **write once, convert on
demand, and check what came out.** Every job
is proven the same way — the command, then a
look *inside* the result: a DOCX unzipped
and its text grepped out of the XML, an EPUB
opened to its catalogue entry, a PDF read
back by a text extractor. "It converted" is
something you verify, not something you
hope.

The lab is one small document, shipped in
`examples/`: a community garden club's
spring agenda (`agenda.md`) with YAML front
matter, a
table, lists, a link, a footnote, and one
small generated chart. Every conversion in
the guide runs on it, so the outputs can be
compared against a source you can hold in
your head — and the produced DOCX and EPUB
are shipped beside it, ready to be unzipped
before you convert anything yourself.

A note on how this guide was verified.
Every command quoted was run on pandoc
**3.1.3** (the Ubuntu build) and captured,
and the guide keeps the surprises the lab
produced, because they are the lessons: a
DOCX rescue that references picture files
nobody wrote until `--extract-media` is
named; an embedded-resources web page that
came out *smaller* than the plain one; a PDF
engine that quietly dropped the document's
title from its properties while its rival
kept it; and a default conversion that
silently *accepts* tracked changes and
deletes comments — chapter 12's experiment,
and the most important result in the guide.
`criteria.md` keeps the full ledger,
including what was not run and why.

## What this guide covers

- [**What pandoc is**](what-pandoc-is.md) —
  the universal converter, its readers and
  writers, and the model everything passes
  through
- [**The command grammar**](the-command-grammar.md) —
  fragments and files, `-f` and `-t`, the
  extension guess and how it goes wrong
- [**Markdown to HTML**](markdown-to-html.md) —
  a standalone page: title, stylesheet, and
  a table of contents that builds itself
- [**Metadata and front matter**](metadata-and-front-matter.md) —
  the YAML block that carries a document's
  facts, and `--metadata`, which overrules
  it for one run
- [**Markdown to DOCX**](markdown-to-docx.md) —
  the Word file, opened up: text, footnotes,
  and pictures inside the zip
- [**DOCX to Markdown, the rescue**](docx-to-markdown.md) —
  trapped text set free, graded honestly
  against the original
- [**HTML to Markdown**](html-to-markdown.md) —
  the other rescue, furniture and all
- [**Markdown to EPUB**](markdown-to-epub.md) —
  a real ebook in one command, catalogued
  and chaptered
- [**PDF and the engine question**](pdf-and-engines.md) —
  pandoc makes no PDFs; engines do. Two are
  run, one absence is captured, and the
  differences are measured
- [**Slides from Markdown**](slides.md) —
  a reveal.js deck from the same source, and
  what the small file doesn't contain
- [**Images and media**](images-and-media.md) —
  references, embedded data, and the return
  trip's one sharp edge
- [**What survives**](what-survives.md) —
  the fidelity ledger, ending with tracked
  changes and comments under all three
  settings
- [**Defaults files and batch jobs**](defaults-and-batch.md) —
  shared settings in a file, a folder of
  documents in a loop, and what pandoc is
  not
- [**Glossary**](glossary.md) — the terms,
  collected in one place

## What it doesn't cover

Styling in depth — custom templates, your
own CSS, and the `--reference-doc` workflow
for house-styled Word files are named where
they belong, not exercised; the lab's
captures are a fresh install's defaults.
Bibliographies and citations (pandoc's
citeproc is a guide of its own), Lua filters
and programmatic document surgery, and the
fifty-odd writers the lab never touches —
ODT, PowerPoint, the wiki formats — are
likewise named, not taught. Beamer slide
decks and the LaTeX PDF road are covered as
far as an absent `pdflatex` allows, with the
failure captured rather than concealed.
Where a neighbouring tool owns the subject —
YAML for the front matter, Bash for the
loops — the chapter points there and moves
on.

## How to use this guide

Read chapters 1–4 in order; they install
the model, the grammar, and the metadata
habit everything else stands on. After
that, chapters stand alone — go to the
conversion you need. Work from inside
`examples/`, with the agenda beside you, and
unzip or extract every output at least once:
that reflex, more than any single flag, is
the skill this guide is teaching.
