# PDF and the Engine Question

Here is the surprise this guide has been
saving: **pandoc does not make PDFs.** There
is no PDF writer rendering the document
model. Ask for a PDF and pandoc prepares the
document, hands it to an outside program —
an **engine** — and the engine does the
typesetting. Which engine you have, and
which you choose, decides how the PDF looks,
what it weighs, and whether your metadata
survives the trip. The lab proves each part
of that, starting with the failure.

## The default assumes LaTeX

With no engine named, pandoc reaches for
the classic road — it writes LaTeX and calls
`pdflatex`. On a machine without a TeX
installation, the whole conversion is one
honest error (exit code 47):

```
$ pandoc agenda.md -o agenda.pdf
pdflatex not found. Please select a different --pdf-engine or install pdflatex
```

The LaTeX road is the oldest and, for
academic work with mathematics and
bibliographies, still the royal one — but a
TeX installation is measured in gigabytes,
and the lab deliberately did not install one
to force this chapter. Instead it took the
lighter engines the machine's package
sources offer, which between them cover most
mortals' documents:

- **WeasyPrint** — renders pandoc's HTML
  output with a print-oriented CSS engine.
- **wkhtmltopdf** — the older WebKit-based
  HTML-to-PDF tool.
- **typst** — the modern, fast typesetter
  pandoc can also drive. On this machine it
  is a captured absence:
  `E: Unable to locate package typst` — its
  absence from one distro's sources says
  nothing about the tool, but the ledger
  records it, because this guide only claims
  what it ran.

## WeasyPrint: the metadata makes it

```
$ pandoc --pdf-engine=weasyprint agenda.md -o agenda.pdf
WARNING: Ignored `text-rendering: optimizeLegibility` at 15:7, unknown property.
WARNING: Expected a media type, got '(max-width: 600px)'
WARNING: Invalid media type ' (max-width: 600px) ' the whole @media rule was ignored at 18:5.
WARNING: Ignored `overflow-x: auto` at 108:7, unknown property.
...
```

The warnings are WeasyPrint reading pandoc's
default HTML stylesheet and declining a few
properties it doesn't implement — a small,
visible honesty about what "HTML to print"
means across engines. The document itself is
complete:

```
$ ls -l agenda.pdf
-rw-rw---- 1 root nogroup 16561 agenda.pdf

$ file agenda.pdf
agenda.pdf: PDF document, version 1.7

$ pdfinfo agenda.pdf
Title:           Fall Conference Planning Meeting
Author:          Convention Committee
Creator:         pandoc
Producer:        WeasyPrint 61.1
Pages:           2
Page size:       595.276 x 841.89 pts (A4)
```

Three things to see. `Producer` names the
engine — your receipt for who typeset the
file. The front matter **travelled**: the
PDF's own Title and Author are the agenda's,
not a filename guess. And the content is
provably inside, by extraction:

```
$ pdftotext agenda.pdf - | grep -c 'Banquet headcount deadline'
1

$ pdftotext agenda.pdf - | head -4
Fall Conference Planning
Meeting
Convention Committee
October 12, 2026
```

The title block extracted whole — wrapped
across two printed lines, as titles are —
followed by author and date, then the
agenda itself.

## wkhtmltopdf: same document, different PDF

```
$ pandoc --pdf-engine=wkhtmltopdf agenda.md -o agenda-wkhtml.pdf
$ ls -l agenda-wkhtml.pdf
-rw-rw---- 1 root nogroup 30700 agenda-wkhtml.pdf

$ pdfinfo agenda-wkhtml.pdf
Title:
Creator:         wkhtmltopdf 0.12.6
Producer:        Qt 5.15.13
Pages:           2
```

Same source, same pandoc, same two pages of
agenda — the banquet line extracts with the
same count of 1. But this file is nearly
twice the size (30,700 against 16,561 bytes),
an older PDF version (1.4 against 1.7), and
its **Title is empty**: the front matter
did not cross into the document properties
on this road. Neither engine is "wrong" —
they are different typesetters with
different habits, and the command line's
`--pdf-engine` is where the difference is
chosen. (The run also prints Qt's progress
chatter to the terminal — harmless, and
another engine personality trait.)

## The lesson

Pandoc's half of a PDF is the document —
structure, text, metadata. The engine's half
is the page — fonts, breaks, margins,
fidelity to the stylesheet. When a PDF
disappoints, ask which half failed before
blaming either: missing content is pandoc
territory; ugly or metadata-less output is
the engine's. Try the engines you have —
they are one flag apart — and keep the one
whose `pdfinfo` tells the story you want
told.

Check: `pdfinfo agenda.pdf` names your
engine as Producer and shows your front
matter as Title, and `pdftotext` finds the
agenda's own lines. If the Producer is a
program you never installed, you have just
learned which default your machine ships —
the first thing this chapter exists to teach.
