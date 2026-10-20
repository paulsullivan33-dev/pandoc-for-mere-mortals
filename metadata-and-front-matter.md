# Metadata and Front Matter

A document is more than its body text. It has
a title, an author, a date — facts *about*
the document that templates place on title
pages, headers, and ebook covers. Pandoc
calls these **metadata**, and the tidy way to
carry them in a Markdown source is a **YAML
front matter** block: three dashes, some
`key: value` lines, three dashes, at the very
top of the file. (YAML itself — quoting,
lists, the traps — has its own guide in this
series, *YAML for Mere Mortals*; this chapter
needs only flat keys.)

The lab agenda opens with one:

```yaml
---
title: Fall Conference Planning Meeting
author: Convention Committee
date: October 12, 2026
---
```

Pandoc holds those lines as **metadata** —
facts about the document, kept apart from
the body blocks chapter 2 printed. Writers
and templates then spend them. In the HTML page from
chapter 3, the title appears twice — in the
browser's `<title>` and in a title block at
the top of the page:

```
$ grep -o '<title>[^<]*</title>' agenda.html
<title>Fall Conference Planning Meeting</title>

$ grep -o '<h1 class="title">[^<]*' agenda.html
<h1 class="title">Fall Conference Planning Meeting
```

The author and date sit beside it in the
same block. In chapter 5's DOCX the title
and author land in the file's document
properties; in chapter 8's EPUB they become
the book's title page and its catalogue
entry. Same lines, spent several ways — that
is the point of keeping facts about the
document in the document, once.

## Overriding from the command line: --metadata

Sometimes the document is right and the
*occasion* differs: a committee copy that
must not circulate, a draft that needs
labelling. `--metadata` sets a value for one
run, and it beats the front matter:

```
$ pandoc -s --metadata title="Committee Copy - Do Not Circulate" agenda.md -o agenda-override.html
$ grep -o '<title>[^<]*</title>' agenda-override.html
<title>Committee Copy - Do Not Circulate</title>
```

The source file is untouched; the override
exists only in that output. Values the
document never set can be added the same way
— here, a subtitle the template knows how to
place:

```
$ pandoc -s --metadata subtitle="Working draft" agenda.md -o agenda-subtitle.html
$ grep -o '<p class="subtitle">[^<]*' agenda-subtitle.html
<p class="subtitle">Working draft
```

Two neighbours deserve naming, briefly and
honestly. `--metadata` feeds the document
model; `-V` (`--variable`) sets template
variables directly — page geometry for PDF,
a header include, template switches. And a
metadata *file* (`--metadata-file`) loads
the same keys from shared YAML, which suits
a committee that stamps many documents
alike. Both are everyday tools in real
pipelines; the lab's captures use front
matter and `--metadata`, and chapter 13's
defaults file is where the shared-settings
idea gets its full treatment.

The precedence rule worth memorising is the
one the captures prove: **command line beats
document**. Whatever a run sets with
`--metadata` replaces the front matter's
value for that run — silently, with no
warning — so an unexpected title in an
output is a clue to go read the command
that made it.

Check: `agenda-override.html` greps the
committee-copy title while `agenda.html`
still greps the original — the source's
front matter, verified unchanged by
converting it again, wins whenever the
command line stays silent.
