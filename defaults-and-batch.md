# Defaults Files and Batch Jobs

Real pandoc use is repetitive: the same
committee, the same formats, the same flags,
every month. Two mechanisms keep that from
becoming folklore pasted from a notes file —
a **defaults file** that carries the
settings, and the shell loop that carries
the files. This closing chapter captures
both, and then states plainly what pandoc
is not.

## The defaults file: -d

A defaults file is YAML (the *YAML for Mere
Mortals* guide covers the format) naming
options in long form. The lab's is a
committee stamp — HTML, standalone, with a
table of contents, every time:

```
$ cat committee.yaml
# committee.yaml - a pandoc defaults file
to: html5
standalone: true
table-of-contents: true
metadata:
  lang: en
```

`-d` loads it:

```
$ pandoc -d committee.yaml agenda.md -o agenda-defaults.html
$ grep -o '<title>[^<]*</title>' agenda-defaults.html
<title>Fall Conference Planning Meeting</title>

$ grep -c 'id="TOC"' agenda-defaults.html
1
```

Title from the front matter, contents built,
neither flag typed. Everything in a defaults
file can also be typed — the file exists so
that a team shares one spelling of "how we
make the handout" instead of five memories
of it. Command-line flags still win over
the file, which still wins over pandoc's
built-ins; the precedence ladder is the
same shape as chapter 4's metadata rule.
Defaults files live, by default, in a
`defaults` folder inside pandoc's user data
directory (the one `pandoc --version`
prints) — or anywhere, if you give `-d` a
path, as the lab does.

## The batch: a loop with nothing to hide

Converting a folder is a shell loop — the
*Bash for Mere Mortals* guide teaches loops
properly; here it is at work:

```
$ for f in agenda.md reminder.md; do pandoc "$f" -o "${f%.md}.docx"; done; ls -l agenda.docx reminder.docx
-rw-rw---- 1 root nogroup 12081 agenda.docx
-rw-rw---- 1 root nogroup  9894 reminder.docx
```

Two sources, two Word files, each writer
chosen by the `-o` extension. Add `-d
committee.yaml`-style settings to the loop
and a month's worth of committee paper is
one command — the same command every month,
which is the entire point.

## What pandoc is not

The guide ends where its first chapter's
admission points. Pandoc is a converter,
and three expectations will disappoint if
you bring them to it:

**It is not a word processor.** It edits
nothing interactively and shows you nothing;
the document you see is always in another
tool. Pandoc's job begins at a finished
draft in some format and ends at that draft
in another.

**It is not a typesetter of last resort.**
The PDF chapter's engines and Word's styles
decide how output *looks*; pandoc decides
what it *says*. Fine typography is
available — through the template, the
reference doc, and the engine — and none of
it is automatic.

**It is not magic on messy input.** Chapter
2's mislabelled file converted faithfully
into garbage, because faithfulness to the
wrong reading is still the wrong reading.
Tracked changes are accepted *by default*
(chapter 12). Tables with merged cells,
text in floating boxes, formatting that
*means* something in the original — the
model approximates or drops them, and the
only defence is this guide's whole method:
convert, then look inside what came out,
before anyone else has to.

Used that way — one source of truth,
conversions as small checkable commands,
suspicion directed at the outputs — pandoc
is the rare tool that does exactly what its
one-line description says. Write once. Keep
the source. Convert on demand, and check.

Check: `pandoc -d committee.yaml agenda.md`
produces a page whose TOC grep counts 1
without a single content flag on the
command line — the settings travelled with
the file, which is where repeat business
belongs.
