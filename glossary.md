# Glossary

**Abstract syntax tree (AST)** — pandoc's
internal model of a document: headings,
paragraphs, lists, tables, and notes as
ideas, before any format renders them.
Printed by the `native` writer (chapter 2).

**Defaults file** — a YAML file of pandoc
options loaded with `-d`, so repeated jobs
share one spelling of the settings
(chapter 13).

**DOCX** — Word's document format: a zip
archive of XML parts (text in
`word/document.xml`, styles, footnotes, and
media alongside). The zip is inspectable
with `unzip` (chapter 5).

**Embed resources** — `--embed-resources`:
fold linked files, such as images, into an
HTML output as data URIs, making one
self-contained file (chapter 11).

**Engine (PDF)** — the outside program that
typesets a PDF on pandoc's behalf:
`pdflatex` by default, or WeasyPrint,
wkhtmltopdf, Typst, and others via
`--pdf-engine` (chapter 9).

**EPUB** — the standard ebook format: a zip
of XHTML chapters, a stylesheet, navigation,
and a package file (`content.opf`) holding
the book's metadata (chapter 8).

**Extract media** — `--extract-media=DIR`:
while converting *from* a container format,
write its embedded images out to files under
DIR, and point the output's references at
them (chapter 11).

**Fragment** — a conversion that is only a
document body — no `<head>`, title, or page
furniture. Pandoc's default HTML output;
`-s` makes it a page (chapters 2–3).

**Front matter** — the YAML block at the
top of a Markdown file (between `---`
lines) carrying metadata: title, author,
date, and more (chapter 4).

**Metadata** — facts about a document,
held apart from its body and spent by
templates and writers: titles on pages,
properties in DOCX files, catalogue entries
in EPUBs. Set in front matter, overridden
with `--metadata` (chapter 4).

**Reader** — the pandoc component that
parses one input format into the document
model. Chosen by extension or with `-f`.

**Reference doc** — `--reference-doc`: a
DOCX whose styles a new DOCX should use,
so Word output wears your house style
(chapter 5).

**Round trip** — converting to a format
and back (Markdown → DOCX → Markdown) to
see what the journey costs (chapters 6, 12).

**Standalone** — `-s`: wrap the converted
body in the writer's template so the output
is a complete document — a full page, a
titled deck, a book with a title page.

**Template** — the skeleton a standalone
output is poured into: where the title,
contents, and body go, and what the page
wears. Writers ship defaults; `--template`
substitutes your own.

**Track changes** — Word's revision record:
insertions, deletions, and comments stored
in the file. Pandoc's DOCX reader accepts
them silently by default; `--track-changes`
chooses accept, reject, or all (chapter 12).

**Writer** — the pandoc component that
renders the document model into one output
format. Chosen by the `-o` extension or
with `-t`.
