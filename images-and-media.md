# Images and Media

Every format in this guide handles images
its own way, and the differences are where
conversions quietly break — the page that
ships without its chart, the rescue that
points at files nobody wrote. The lab's
chart is a 344-byte generated PNG
(`examples/chart.png`), small enough that
every claim below is checkable to the byte.

## In HTML: a reference, until you say otherwise

Chapter 3's standalone page does not contain
the chart. It contains a pointer to it,
exactly as the Markdown had:

```
$ grep -o '<img src="[^"]*"' agenda-bare.html
<img src="chart.png"
```

Move the page without its sibling file and
the image is a broken icon. `--embed-resources`
folds the file into the page as a data URI —
the image becomes text inside the HTML:

```
$ pandoc -s --embed-resources agenda.md -o agenda-embedded.html
$ grep -o 'src="data:image/png;base64' agenda-embedded.html | head -1
src="data:image/png;base64
```

Now the surprise, kept because measurements
outrank folklore: the embedded page is
*smaller* than the plain one — 5,380 bytes
against 5,841:

```
$ ls -l agenda-bare.html agenda-embedded.html
-rw-rw---- 1 root nogroup 5841 agenda-bare.html
-rw-rw---- 1 root nogroup 5380 agenda-embedded.html
```

Both effects are real. The image added its
base64 weight (344 bytes of PNG become about
460 characters of text), and producing an
embedded page makes pandoc re-emit the whole
HTML, which strips the template's generous
indentation. On this fixture the second
effect won. On a photographic image — where
base64 dwarfs any whitespace — the first
effect wins by a landslide. The lesson is
the guide's refrain: embed when a file must
travel alone, and read the sizes rather than
assuming them.

## In DOCX and EPUB: the image moves in

The container formats carry the image inside
themselves — chapter 5 found it at
`word/media/rId24.png` in the DOCX, byte for
byte identical to the source (`cmp` said
IDENTICAL), and chapter 8 found it at
`EPUB/media/file0.png` in the book. Nobody
embeds or extracts by hand in those
directions; the zip is the suitcase and the
image is packed.

## Coming back out: the media trap

The return trip has the sharpest edge in
this guide. Chapter 6's rescue converted
the DOCX to Markdown, and the output
cheerfully references the image:

```
$ grep -o 'media/rId24.png' agenda-roundtrip.md
media/rId24.png
```

But the plain conversion never wrote any
such file:

```
$ ls media
ls: cannot access 'media': No such file or directory
```

The reference dangles. Any pipeline that
rescues DOCX files without knowing this
ships documents with broken pictures,
confidently. The fix is one flag,
`--extract-media`, which writes the media
out as it converts — into the directory you
name:

```
$ pandoc agenda.docx -t markdown --extract-media=extracted -o agenda-extracted.md
$ find extracted -type f | sort
extracted/media/rId24.png

$ ls -l chart.png extracted/media/rId24.png
-rw-rw---- 1 root nogroup 344 chart.png
-rw-rw---- 1 root nogroup 344 extracted/media/rId24.png

$ cmp extracted/media/rId24.png chart.png && echo IDENTICAL
IDENTICAL
```

Same 344 bytes out that went in — through
Markdown, into Word's zip, and back to a
file, the chart never changed. And the
converted Markdown now points at the path
that exists (`extracted/media/rId24.png`),
because pandoc rewrites the reference to
match where it put the file.

One wart from chapter 6 belongs in this
ledger too: on extraction, pandoc annotates
the image with its size as `{width="…in"
height="…in"}` attributes, computed at 72
pixels to the inch and printed to sixteen
digits. Harmless, occasionally useful, and
not something your source said — delete it
in the cleanup pass if it bothers you.

Check: `cmp` on the extracted image prints
nothing and your `echo IDENTICAL` follows —
the picture survived both directions, and
you know the one flag that makes the return
trip safe.
