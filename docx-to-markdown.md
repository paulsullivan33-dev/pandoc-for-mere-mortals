# DOCX to Markdown: The Rescue

Now the direction that rescues people. The
only copy of something lives in a Word file —
a policy, last year's minutes, a chapter a
collaborator sent back — and you want it in
Markdown, where it can be versioned, diffed,
and published. Pandoc reads DOCX as happily
as it writes it:

```
$ pandoc agenda.docx -t markdown -o agenda-roundtrip.md
```

The lab can grade this conversion exactly,
because it knows the original by heart. The
output is complete and usable. It is also
not the document that went in, and the
differences are the lesson. The honest
scorecard, taken from the captured output
and the `diff` against `agenda.md` (which
exits 1 — differences found, as expected):

**Everything substantive survived.**
Headings are headings, at the right levels.
Bold is `**Friday**`; the italic is intact;
the list is a list; the link carries its URL.
The table's every cell is present — all four
rows, all three columns, every time and name.

**The table changed costume.** It came back
not as the pipe table that went in, but as a
*grid table* — pandoc's most capable
Markdown table syntax — with columns wrapped
to a text width:

```
  -----------------------------------------------------------------------
  Time                    Item                    Owner
  ----------------------- ----------------------- -----------------------
  9:00 AM                 Call to order and       Chair
                          introductions
```

Same facts, different notation, and the
column wrapping makes it harder to scan in
raw form even though it renders identically.
Neither syntax is wrong; they are not the
same file.

**The footnote survived, renumbered.**
`[^carry]` became `[^1]`, and its text moved
to the end, indented as a footnote body.
Nothing lost; the label you chose is gone.

**The front matter did not come back.**
There is no YAML block in the round trip at
all. The title and author are in the DOCX's
properties (chapter 5 greped them there),
but the DOCX reader does not restore them
as document metadata — the agenda's own
`# Agenda` heading is all the output has.
Re-type the three lines by hand; they are
in the source, and now you know to check.

**The image picked up baggage.** It returns
as a reference to `media/rId24.png` — a file
the plain conversion never wrote (chapter 11
shows the trap and its fix) — wearing its
dimensions as attributes in inches, to
sixteen digits:

```
![Visitors at the last three open
days](media/rId24.png){width="2.2222222222222223in"
height="1.3888888888888888in"}
```

And the image's caption, which Markdown
kept as alt text, also appears after it as
a plain paragraph.

## The verdict

The rescue direction works, and for its real
job — *the text is trapped in Word, get it
out* — it works well: nothing a reader would
miss is missing. What it does not do is
recreate your source. A round trip through
DOCX is a translation into Word's vocabulary
and back, and translations rephrase. Keep
the Markdown as the original whenever you
can; treat DOCX as an output. When you can't
— when Word is where the document truly
lives — convert without fear, then budget
ten minutes for the front matter, the image
attributes, and a table that changed its
clothes.

Check: `diff agenda.md agenda-roundtrip.md`
exits 1, and the hunks are the ones named
above — the missing YAML block, the grid
table, the image line with its inches — and
no hunk is missing a sentence of the agenda
itself.
