# What Pandoc Is

You wrote the thing once — in Markdown, say.
Now it is needed as a web page for the site,
a Word document the group can mark up, a
PDF for the record, and maybe an ebook for
the member who reads everything on a tablet.
Four formats, one text. The usual way to get
there is to reformat by hand, four times, and
then keep four copies in step forever. That
way is the problem this tool exists to end.

**Pandoc** is a command-line converter for
documents. It reads a document in one format
and writes it in another — Markdown to HTML,
Word to Markdown, Markdown to EPUB, and
dozens more pairings. It has been maintained
since 2006 by John MacFarlane and is the
quiet engine inside a great many publishing
pipelines, static site tools, and "export
as…" menus. This guide teaches you to drive
it yourself.

One idea explains everything pandoc does,
including its failures, so it comes first:

> **Pandoc does not convert formats
> pairwise. It reads your document into its
> own internal model of a document — the
> *abstract syntax tree*, or AST — and then
> writes that model out in the target
> format.**

A heading in the model is a heading, full
stop: not a Word style, not an `<h2>` tag.
Forty-some **readers** know how to build the
model from their format; sixty-some
**writers** know how to render it. That is
why one command can cross between formats
that have never heard of each other. It is
also why chapter 12 of this guide exists:
anything your document contains that the
model has no box for is what a conversion
can drop, and the honest way to learn the
edges is to convert real documents and look
inside the results — which is what the lab
in this guide does, end to end.

## Checking your install

```
$ pandoc --version
pandoc 3.1.3
Features: -server +lua
Scripting engine: Lua 5.4
```

If you don't have it: `apt install pandoc`
on Debian/Ubuntu, `brew install pandoc` on a
Mac, or the installers from pandoc.org on
Windows. Everything in this guide was run on
the Ubuntu build of **3.1.3**; newer versions
add formats and polish, and the core grammar
in chapter 2 has been stable for years.

How many formats is "dozens"? Ask the tool,
not the brochure:

```
$ pandoc --list-input-formats | wc -l
43

$ pandoc --list-output-formats | wc -l
64
```

The lists themselves are worth one skim —
they include the famous (`docx`, `epub`,
`html`, `latex`), the workmanlike (`odt`,
`rtf`, `pptx`), and pandoc's own (`native`,
`json` — the model itself, written out;
chapter 2 puts it to work). One name in the
writers list is a promise rather than a
writer: `pdf`. Pandoc renders no PDF itself;
choosing it hands the document to a helper
program — an *engine* — and the difference
matters enough to earn chapter 9.

Check: `pandoc --version` prints a version
line first, and the two format lists give
counts in the forties and sixties on a
recent build (your exact numbers will differ
a little by version — the lists grow). If
both hold, your converter is ready, and
every later chapter's proof will read the
same way this one's does: run it, then look
inside what it made.
