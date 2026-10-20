# Markdown to HTML

The most common pandoc job in the world:
notes in Markdown, page on the web. Chapter 2
left the lab agenda as a fragment. This
chapter makes it a page — with a title, a
table of contents, and furniture — and proves
each addition in the output rather than
trusting the flags.

## Standalone: -s

`-s` (long form `--standalone`) wraps the
fragment in the writer's template: doctype,
`<head>`, the document's metadata placed
where the template puts it, and a default
stylesheet pandoc carries so the page is
readable without any work from you.

```
$ pandoc -s agenda.md -o agenda-bare.html
$ ls -l agenda-bare.html
-rw-rw---- 1 root nogroup 5873 agenda-bare.html
```

The checks
that matter are inside the page. The
document's front matter — chapter 4's
subject — supplies the title:

```
$ grep -o '<title>[^<]*</title>' agenda.html
<title>Fall Conference Planning Meeting</title>
```

And the headings carry the anchors the rest
of the web machinery expects:

```
$ grep -o '<h1 id="[^"]*">[^<]*' agenda.html | head -3
<h1 id="agenda">Agenda
```

That `id` is not decoration: it is what
links, scripts, and the table of contents
point at.

## A table of contents: --toc

`--toc` builds a contents list from the
headings and inserts it where the template
provides for it — in the default HTML
template, a `<nav>` before the body text:

```
$ pandoc -s --toc agenda.md -o agenda.html
$ ls -l agenda.html
-rw-rw---- 1 root nogroup 6220 agenda.html

$ grep -A6 'id="TOC"' agenda.html
<nav id="TOC" role="doc-toc">
<ul>
<li><a href="#agenda" id="toc-agenda">Agenda</a>
<ul>
<li><a href="#schedule" id="toc-schedule">Schedule</a></li>
...
```

Every entry links to a heading's `id`, and
the nesting follows the heading levels —
H1, then its H2s inside it. Nothing was
typed twice: rename a heading, re-run, and
the contents follow. The lab's table of
times and owners is in the page too, as a
real HTML table:

```
$ grep -c '<table' agenda.html
1
```

## What the page is — and isn't

The standalone page pandoc makes is
deliberately plain: one embedded stylesheet,
sane margins, no JavaScript. It is a good
document page and a poor imitation of your
website's design, and that gap is normal —
sites feed pandoc *fragments* (chapter 2)
into their own templates, or point pandoc at
a custom template and stylesheet with
`--template` and `--css`. Those are real
tools, named here so you know they exist;
this guide's lab uses the defaults
throughout, so what you see in these
captures is what a fresh install produces.

Check: your `agenda.html` greps
`<title>Fall Conference Planning Meeting</title>`,
contains one `<table`, and its `id="TOC"` nav
links `#schedule` to the Schedule heading. If
all three hold, the page is complete — and
every claim in it was checked in the output,
which is the habit this guide is really
teaching.
