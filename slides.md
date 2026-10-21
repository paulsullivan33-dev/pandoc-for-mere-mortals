# Slides from Markdown

A talk is a document with the paragraphs
taken out. Pandoc treats slides as one more
writer: the same Markdown, rendered for a
slide framework instead of a page. The
popular target is **reveal.js**, the HTML
slide library, and asking for it is one
flag:

```
$ pandoc -s -t revealjs agenda.md -o agenda-slides.html
$ ls -l agenda-slides.html
-rw-rw---- 1 root nogroup 8937 agenda-slides.html

$ grep -o 'class="reveal"' agenda-slides.html
class="reveal"
```

The proof that slides came out is in the
structure: the deck is a stack of `<section>`
elements — pandoc counted 3 top-level ones
in the capture, a title slide generated from
the front matter, one opening at the `# Agenda`
heading, and a footnotes slide collecting the
agenda's footnote:

```
$ grep -c '<section' agenda-slides.html
3

$ grep -o '<section id="[^"]*"' agenda-slides.html | head -5
<section id="title-slide"
<section id="agenda"
<section id="footnotes"
```

Heading levels do the layout work: a
level-1 heading opens a horizontal slide,
level-2 headings stack vertically beneath
it, and the content of each section is its
slide's body. The agenda's table slide and
lists fall out of the structure the document
already had — which is the attraction: the
deck and the handout are one source, so they
cannot disagree.

Two honest limits, both visible from the
capture. First, the 8,937-byte file does
not contain reveal.js itself: the deck links
the framework from a public CDN, so
presenting from it needs the network (or a
local copy, which `-V revealjs-url=` can
point at — a documented option the lab does
not exercise). A slide file this small is a
set of instructions, not the projector.
Second, pandoc gives you slides, not a
slide *design*: the default theme is plain,
and everything a designer would adjust is
available and unadjusted.

The other slide road is **beamer**, pandoc's
LaTeX-based slide writer, which produces a
PDF deck — and so belongs to chapter 9's
engine story: no PDF engine, no beamer deck.
It is named here so the map is complete, and
ledgered as untested on this machine.

Check: `agenda-slides.html` greps
`class="reveal"`, and its sections open with
a title slide made from your front matter.
If they do, the deck exists — the same
agenda, standing up in front of a room
instead of lying on a page.
