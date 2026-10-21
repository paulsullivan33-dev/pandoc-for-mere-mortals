# What Survives: Fidelity

Chapter 1 made a promise-shaped admission:
everything passes through pandoc's document
model, and whatever the model has no box
for is what a conversion can lose. Chapters
6 and 7 graded two rescues anecdotally. This
chapter is the ledger, kept strictly from
the lab's own captures, and it ends with the
experiment that matters most to anyone who
converts other people's Word files.

## The scorecard

| The feature | Into DOCX | Back to Markdown | From the HTML page |
| --- | --- | --- | --- |
| Headings, structure | ✓ | ✓ | ✓ |
| Bold, italic | ✓ | ✓ | ✓ |
| Lists | ✓ | ✓ | ✓ |
| Links | ✓ | ✓ | ✓ |
| Tables | ✓ | content ✓, syntax changed | content ✓, syntax changed |
| Footnotes | ✓ real Word footnote | ✓ renumbered | ✓ with anchor cruft |
| Images | ✓ byte-identical inside | reference ✓, file needs `--extract-media` | reference ✓, figure stays raw HTML |
| Front matter | title/author → file properties | ✗ not restored | becomes a div, not metadata |
| Captions | ✓ | returns as a plain paragraph | ✓ inside the raw figure |

Read it as a map, not a grade: the model's
vocabulary — structure, emphasis, lists,
links, tables, notes, pictures — travels
well in every direction this guide tested.
The losses cluster exactly where chapter 1
said they would: in the facts *about* the
document (metadata) and in the other
format's machinery (Word styles, HTML
figures), which the model holds loosely or
not at all.

## The experiment: revisions and comments

The scorecard has a hole a table can't show.
Word documents in the wild carry *tracked
changes* and *comments* — the whole review
process, recorded in the file. What does a
conversion do with them? The lab answered
by manufacturing the situation honestly: it
unzipped chapter 5's DOCX and edited the XML
the way Word itself writes it — one tracked
insertion ("Order more twine for the bean
poles.", author A. Reviewer) and one
comment ("Did anyone confirm the projector
takes HDMI?") anchored to the projector
line — then rezipped. (The editing script
is kept with the lab; the point is that the
file is an ordinary reviewed DOCX.)

Converted with default settings:

```
$ pandoc agenda-tracked.docx -t markdown -o tracked-accept.md
$ grep -c "Order more twine" tracked-accept.md
1
$ grep -c "HDMI" tracked-accept.md
0
```

Read those two counts carefully, because
together they are the most important result
in this guide. The insertion is **in** —
count 1 — with no mark on it: pandoc's
default is to *accept* tracked changes, so
a sentence a reviewer merely proposed
becomes plain text, indistinguishable from
what the author wrote. And the comment is
**gone** — count 0 — silently. A conversion
run this way doesn't just lose information;
it rewrites who said what, and leaves no
trace that it did.

Pandoc does have the setting; it is simply
not the default:

```
$ pandoc --track-changes=reject agenda-tracked.docx -t markdown -o tracked-reject.md
$ grep -c "Order more twine" tracked-reject.md
0
```

Reject treats the insertion as never made.
And `all` keeps the review visible, which
is the setting a careful rescue wants:

```
$ pandoc --track-changes=all agenda-tracked.docx -t markdown -o tracked-all.md
$ grep -o "insertion" tracked-all.md
insertion
$ grep -o "Did anyone confirm the projector takes HDMI?" tracked-all.md
Did anyone confirm the projector takes HDMI?
```

In the output, both arrive as marked spans
that say what they are and who wrote them:

```
[Order more twine for the bean poles.]{.insertion author="A. Reviewer"
date="2026-10-20T09:00:00Z"}

[Did anyone confirm the projector takes HDMI?]{.comment-start id="1"
author="A. Reviewer" date="2026-10-20T09:05:00Z"}Check the projector
for the seed-starting slideshow.[]{.comment-end id="1"}
```

The comment's text sits at the start of the
range it annotates, bracketed by
`.comment-start` and `.comment-end` — the
model does have a box for it after all, but
only `all` asks the reader to unpack it.

## The rule

A conversion is a *reading* of a document,
not a copy of the file. If a DOCX may carry
an unfinished review — and in group
documents, it usually may — convert it with
`--track-changes=all` and read the spans
before you publish anything, or settle the
review in Word first. The default conversion
is not lying to you; it is answering a
different question ("the document, as if
accepted") than the one you probably meant
to ask.

Check: the three greps reproduce the three
counts — 1 and 0 by default, 0 on reject,
and both texts present under `all`. If your
numbers match, you have seen the one place
in this guide where a conversion changes
meaning by default, and you know the flag
that keeps it honest.
