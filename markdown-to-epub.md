# Markdown to EPUB

Of all pandoc's outputs, the ebook is the
most satisfying per keystroke. One command
takes the same agenda and produces a real
EPUB — the format every e-reader and tablet
reads:

```
$ pandoc agenda.md -o agenda.epub
$ ls -l agenda.epub
-rw-rw---- 1 root nogroup 6299 agenda.epub

$ file agenda.epub
agenda.epub: EPUB document
```

Six kilobytes. And an EPUB, like a DOCX, is
a zip with rules — so the same habit applies:
open the box before you believe the book.

```
$ unzip -l agenda.epub
       20  mimetype
      252  META-INF/container.xml
     1581  EPUB/content.opf
     1351  EPUB/toc.ncx
     1069  EPUB/nav.xhtml
      645  EPUB/text/title_page.xhtml
     4102  EPUB/styles/stylesheet1.css
      344  EPUB/media/file0.png
     2478  EPUB/text/ch001.xhtml
    ...
    12003      10 files
```

Ten parts (one listing line trimmed). The
`mimetype` file sits first and stored
uncompressed, as the EPUB standard demands;
`META-INF/container.xml` tells readers where
the book's package file lives; and the rest
is recognisably a small website in a bag —
XHTML chapters, a stylesheet, a navigation
file, and the image.

The proofs are the same greps as ever. The
front matter became the book's catalogue
entry, in `EPUB/content.opf`:

```
$ unzip -p agenda.epub EPUB/content.opf | grep -o '<dc:title[^<]*</dc:title>'
<dc:title id="epub-title-1">Spring Planting Planning Meeting</dc:title>
```

The agenda's text is in the chapter file,
`EPUB/text/ch001.xhtml` — chapter breaks fall
at the top-level headings, so a longer
document becomes many chapter files by the
same rule:

```
$ unzip -p agenda.epub EPUB/text/ch001.xhtml | grep -o 'Compost delivery deadline'
Compost delivery deadline
```

And the chart is inside at `EPUB/media/`
— 344 bytes in the listing, the image
itself, listed rather than described.

What pandoc built from your three front
matter lines deserves a second look: a
**title page** (`title_page.xhtml`) carrying
title, author, and date, plus the navigation
(`nav.xhtml` and the older-format `toc.ncx`)
generated from the headings — the same
headings-to-contents machinery as chapter
3's `--toc`, spent in a different format.
Write the structure once, in Markdown, and
every container pandoc targets lays it out
in its own native way.

That is the whole chapter, because EPUB is
where pandoc's model pays off most cleanly:
the format wants exactly what the model
holds — titled sections, styled text, a
manifest, and metadata — and nothing the
model lacks. If your documents need to reach
e-readers, this is the cheapest complete
win in the guide.

Check: `unzip -l agenda.epub` lists 10 files
with `mimetype` first, the `dc:title` grep
prints the agenda's title, and the chapter
grep finds the compost line. If all three
hold, the book is real — open it in any
reader and it will simply work.
