# HTML to Markdown

The other rescue: a page exists as HTML — on
a website, in an old export, in an email —
and you want the text back in a form you can
edit and keep. Pandoc reads HTML as one of
its 43 input formats:

```
$ pandoc agenda.html -f html -t markdown -o agenda-from-html.md
```

The lab feeds it the standalone page from
chapter 3 — TOC and title block included —
because that is the honest test: real pages
come with furniture. The text all survives.
The furniture comes home with it, and the
grade is in the details.

**The title block became a structure.**
Where the front matter once was, the rescue
has a fenced div carrying the page's title
block, with the title demoted to a heading
with attributes:

```
::: {#title-block-header}
# Spring Planting Planning Meeting {#spring-planting-planning-meeting .title}

Rosa Jimenez

March 8, 2026
:::
```

Not metadata — text, wearing its HTML
plumbing. Restoring the YAML block is a
hand edit, as it was in chapter 6.

**The table of contents came along.** The
`--toc` nav is, to the reader, a list of
links — so a list of links is what you get,
each still carrying its `toc-` id:

```
-   [Agenda](#agenda){#toc-agenda}
    -   [Schedule](#schedule){#toc-schedule}
```

In a rescue you delete this block; pandoc
cannot know it was generated, because in the
HTML it is simply content. That is the
general rule of this direction: **everything
in the page is treated as the document**,
including the parts a machine put there.

**The body is clean, with residue.** The
table came back as a simple table — no
pipes, content complete. Headings, lists,
emphasis, and the link are plain Markdown.
But the footnote shows what HTML round
trips do at the joints. The reference is
decorated with the anchors HTML needed:

```
2.  Who carries the seed trays in for the spring
    sale.[^1^](#fn1){#fnref1 .footnote-ref role="doc-noteref"}
```

and the image, which HTML expressed as a
`<figure>`, stayed one — raw HTML carried
through verbatim rather than translated back
to Markdown image syntax:

```
<figure>
<img src="chart.png" alt="Visitors at the last three open days" />
<figcaption aria-hidden="true">Visitors at the last three open
days</figcaption>
</figure>
```

Nothing there is wrong — the Markdown writer
keeps raw HTML it cannot faithfully
re-express, and Markdown renders embedded
HTML fine. It is simply not the clean source
you started from.

## The verdict, and a tip

HTML → Markdown gives you a working draft
with everything in it, at the price of a
cleanup pass: strip the generated furniture,
unwrap the attribute cruft, re-add your
front matter. The plainer the HTML you feed
it, the lighter the *structural* part of
that pass. The lab's appendix runs the same
rescue on chapter 2's bare fragment instead
of the standalone page: the title block and
the contents list are simply absent — a grep
for `title-block` counts 0, and the output
opens at `# Agenda`. What does not improve
is the joint residue: the footnote still
wears its reference attributes and the
figure is still raw HTML, because that cruft
is not page furniture — it is how HTML
itself says those things, fragment or not.
When you control both ends, publish
fragments and skip the surgery; the small
cleanup at the joints, you do either way.

Check: `agenda-from-html.md` contains the
full agenda text, the `{#title-block-header}`
div, and the TOC as a link list — and
`diff agenda.md agenda-from-html.md` exits
1 with no sentence of the agenda missing
from the rescued side.
