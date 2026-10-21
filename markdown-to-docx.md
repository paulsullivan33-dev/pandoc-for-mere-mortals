# Markdown to DOCX

The rest of the club does not read Markdown.
It reads Word. So the second everyday job is
the one that makes pandoc welcome in offices:

```
$ pandoc agenda.md -o agenda.docx
$ ls -l agenda.docx
-rw-rw---- 1 root nogroup 12055 agenda.docx

$ file agenda.docx
agenda.docx: Microsoft Word 2007+
```

Twelve kilobytes, and Word, LibreOffice, and
Google Docs will all open it. But "it opened"
is the weakest proof in this guide's
vocabulary. A DOCX is not a mysterious binary
— it is a **zip archive of XML parts**, and
everything this chapter claims can be checked
by opening the box.

## A DOCX is a zip

```
$ unzip -l agenda.docx
     1898  [Content_Types].xml
      722  _rels/.rels
     8926  word/document.xml
     1511  word/_rels/document.xml.rels
     5698  word/numbering.xml
    21011  word/styles.xml
     1388  word/footnotes.xml
      627  docProps/core.xml
     2494  word/fontTable.xml
      344  word/media/rId24.png
    ...
    56226      17 files
```

Seventeen parts (the listing is trimmed):
the document itself, its styles, its
numbering for lists, a theme, fonts — and
three parts worth opening.

**The text** lives in `word/document.xml`.
The agenda's table, which Markdown wrote as
pipes and dashes, is in there as Word table
markup, and its cell text greps straight
out:

```
$ unzip -p agenda.docx word/document.xml | grep -o 'Compost delivery deadline'
Compost delivery deadline

$ unzip -p agenda.docx word/document.xml | grep -o 'Plot assignment sign-ups'
Plot assignment sign-ups
```

**The front matter** landed in the document
properties, as chapter 4 promised — title
and author, in `docProps/core.xml`:

```
$ unzip -p agenda.docx docProps/core.xml | grep -o '<dc:title>[^<]*</dc:title>'
<dc:title>Spring Planting Planning Meeting</dc:title>

$ unzip -p agenda.docx docProps/core.xml | grep -o '<dc:creator>[^<]*</dc:creator>'
<dc:creator>Rosa Jimenez</dc:creator>
```

**The footnote** became a real Word footnote,
in its own part — not a paragraph faked at
the bottom of the page, but the genuine
mechanism Word's Insert Footnote makes:

```
$ unzip -p agenda.docx word/footnotes.xml | grep -o 'the gate still won'
the gate still won
```

**The image** is inside the box too, under
`word/media/`, and it is not a re-encode —
it is the same bytes, which a comparison
proves (chapter 11 returns to image
handling in full):

```
$ unzip -p agenda.docx word/media/rId24.png > extracted-from-docx.png
$ cmp extracted-from-docx.png chart.png && echo IDENTICAL
IDENTICAL
```

## What decides how it looks

Not pandoc's taste — Word **styles**. The
21 KB `styles.xml` in the listing maps the
document model's ideas (heading, paragraph,
table) onto named styles, and Word renders
those. To change the look of every document
you produce, you give pandoc a **reference
doc**: `--reference-doc=house.docx` takes
its styles from a DOCX you have styled in
Word once. It is the right tool and the
standard workflow; the lab does not exercise
it, because its captures would be this
machine's style choices rather than a
verifiable behaviour — so it is named here,
honestly unrun, and the default styles are
what every DOCX capture in this guide shows.

Check: `unzip -l agenda.docx` lists 17 parts
including `word/footnotes.xml` and a
`word/media/` image, and the two cell-text
greps print their lines. If they do, the
Word file is not merely present — it is
provably the agenda, in Word's own
vocabulary, ready for the club.
