# The Command Grammar

Every pandoc command in this guide has the
same skeleton:

```
pandoc [options] input-file…
```

It reads the input, builds the document
model (chapter 1), renders it, and writes
the result to your terminal — standard
output — unless you send it to a file. Three
decisions ride on almost every command:
which **reader** (guessed from the input),
which **writer** (guessed from the output),
and whether the output is a *fragment* or a
*standalone* document. Learn to see those
three and you can read any pandoc command
line at a glance.

## The default: a fragment on the terminal

With no options at all, pandoc converts and
prints. The guide's lab document — a garden
club agenda in Markdown, shipped as
`examples/agenda.md` — comes back as an HTML
*fragment*: the body of a page, with no
`<html>`, no `<head>`, no title:

```
$ pandoc agenda.md
<h1 id="agenda">Agenda</h1>
<p>The club meets to assign the remaining tasks
for the spring planting. Bring your
calendars; most of what follows is dates, beds,
and who owns what.</p>
<h2 id="schedule">Schedule</h2>
<table>
...
```

A fragment is exactly what you want when the
output is headed for a template, a blog
engine, or another file that already has the
page furniture. It surprises everyone once:
pandoc's default is a piece of a document,
not a whole one. Chapter 3's `-s` is the
fix, and it is the flag beginners miss.

## Naming the reader and writer: -f and -t

Pandoc guesses both ends from file
extensions. Override the guess with `-f`
(from) and `-t` (to), using the names from
`--list-input-formats` and
`--list-output-formats`:

```
$ pandoc -f markdown -t html5 agenda.md
```

Sending output to a file with `-o` lets the
extension choose the writer, which is how
most real commands look:

```
$ pandoc agenda.md -o agenda-fragment.html
```

`agenda-fragment.html` is the capture above
in a file — 1,883 bytes of fragment. Nothing
about `-o` changes the conversion; it only
redirects it, and (without `-s`) it does not
make the fragment a page.

## When the guess is wrong

The guess trusts the extension completely,
and the lab demonstrates both directions.
The same Markdown saved as `notes.txt` is
still read correctly — pandoc's fallback for
an unknown extension is Markdown:

```
$ cp agenda.md notes.txt
$ pandoc notes.txt
<h1 id="agenda">Agenda</h1>
...
```

But save Markdown under an `.html` name and
pandoc reads it *as HTML*, faithfully
converting your markup-as-text into escaped
nonsense:

```
$ cp agenda.md mislabelled.html
$ pandoc mislabelled.html -t markdown
\-\-\- title: Spring Planting Planning Meeting
author: Rosa Jimenez date:
March 8, 2026 \-\-\- \# Agenda The club meets
to assign the remaining tasks …
```

Every heading, table pipe, and list marker
survives as literal text, backslash-escaped —
a perfect conversion of the wrong reading.
The lesson generalises beyond pandoc:
**the tool believes the label, not the
contents.** When a conversion comes out
garbled, suspect the guessed reader first
and pin it with `-f`.

## Looking at the middle

Because everything passes through the model,
you can print the model. `native` is the
model in pandoc's own notation — a list of
blocks, each built from inlines:

```
$ pandoc -t native agenda.md | head -14
[ Header 1 ( "agenda" , [] , [] ) [ Str "Agenda" ]
, Para
    [ Str "The"
    , Space
    , Str "club"
    , Space
    , Str "meets"
    , Space
    , Str "to"
    , Space
    , Str "assign"
```

`Header 1` with its identifier, a `Para` of
`Str` and `Space` pieces — further down are
the `Table`, the lists, the footnote, in the
same vocabulary. (The front matter is held
as metadata, apart from the blocks; chapter
4 spends it.) You will rarely print this for
its own sake, but remember it exists: when a
conversion loses something, the question
"did the model ever hold it?" is answerable
in one command, and chapter 12 leans on
that fact.

Check: `pandoc agenda.md` begins with
`<h1 id="agenda">` and contains no `<html`
line — a fragment. `pandoc -t native
agenda.md` begins with `[ Header 1`. If both
hold, you hold the grammar: reader in, model
in the middle, writer out.
