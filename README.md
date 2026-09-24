# Temple of Mercury — build kit

Everything needed to rebuild the whole corpus from source. Nothing here is
required to keep the practice; it is required only to reprint it.

## What builds what

| Command | Output |
|---|---|
| `python3 build.py book.src stage.pdf && python3 post.py stage.pdf "The_Book_of_the_Temple_of_Mercury_-_Tiered.pdf"` | The Book, 77 pages: six volumes and a concordance |
| `python3 model_content.py` | The Model Book → `model_new.pdf` |
| `python3 psalter.py` | The Mercurial Psalter |
| `python3 almanac.py` | A Mercurial Almanac 2027 |
| `python3 pocket.py` | The Way of Mercury, the A6 pocket book, 84 pages (pads itself to a multiple of four with ruled leaves) |
| `python3 seed.py` | The Seed → `The_Seed.pdf`, 9 pages, from Part Zero as kept in `book_tiered.src` |
| `python3 pocket.py a4` | The Way of Mercury, full-size edition: A4, two columns, 25 pages; part openers and the front and back matter flow in the columns instead of taking whole pages |
| `python3 gen_comp.py && python3 buildc.py comp.src "Dyadic_Neo-Enochian_-_Companion.pdf"` | The Companion, 15 pages |
| `python3 gen_site.py` | `index.html` — the site, with all six books embedded (The Way of Mercury in both editions) |

Run them in that order: `gen_site.py` embeds whatever PDFs are currently in
the kit directory (`OUT="."` in `gen_site.py`), so build the books first.
All scripts now write to the kit directory; copy the PDFs, `index.html`, and
the repository README to the site from there.

Requires Python 3 with `reportlab` and `pymupdf`. Fonts are in `fonts/`.

## The files

| File | What it is |
|---|---|
| `book.src` | **The source of truth.** The Book's entire text in a light markup. Edit this; everything else follows. |
| `build.py` | Typesetter for the Book. Two-column, ornamented, with the Tiered structure. |
| `post.py` | Second pass: part pages, running heads, page numbers, the table of contents. Also retitles the Part Five artwork page from "The Way of Ma'at" to "The Compass" (the title is baked into `assets/temple-artwork-source.pdf`; fixing the artwork would let that block be removed). |
| `buildmodel.py` | Typesetter for the single-column books: the Model Book, Psalter, and Almanac. Fonts, styles, ornaments, and the `build()` entry point they all import. |
| `model_content.py` | The Model Book's page assembly. Pulls the Rite, the Casting, the Ledger and the rest out of `book.src`; the Door, Dedication, invocations and Whom to Call On are held here as text, so **liturgy changes must be made in both places.** |
| `psalter.py` | The Psalter's layout. |
| `psalter_pt1.py` | The ten hymns. |
| `psalter_pt2.py` | Collects, convening prayers, antiphons, the litany, and the blessings. |
| `pocket.py` | Both editions of The Way of Mercury, from one text. Pulls the Calendar, the Rites of Passage, the full Door, the Dedication, Grounding to Air, Walking the Pillar, the Good Page, the Casting, Declarative Alignment, the Answered Page, the Returned Oath, the Invocations, the Signs and the Law from `book.src`, and the prayers, blessings and antiphons from `psalter_pt2.py`; the core chapters are held in the file itself. |
| `almanac.py` | The Almanac. Astronomical dates are typed in, from Fred Espenak's sky almanac; a new year needs a new set. |
| `gen_comp.py` | Generates `comp.src`, the Companion's text. The four workings' names are **derived, not typed** — edit this, never `comp.src`. |
| `buildc.py` | Typesetter for the Companion (a variant of `build.py`). |
| `dne.py` | The Enochian engine: grafting, name derivation, prayer assembly. |
| `great.json` | The Great Table. |
| `diagrams.py` | The Companion's table and subangle figures. |
| `orn.json` | Ornament paths. |
| `gen_site.py` | Builds the one-file site. Edit the `DOCS` list there to change titles, descriptions, or page counts. |
| `assets/`, `fonts/` | Images, EB Garamond, and `FreeSerif.ttf`, registered as `Sym` in `build.py` and `buildmodel.py` for glyphs EB Garamond lacks. The Mercury sign is written `<font name="Sym">☿</font>` wherever it appears. |

## Markup used in `book.src`

`@part`, `@ch`, `@sec`, `@h2`, `@p`, `@item`, `@num`, `@lab`, `@off`,
`@osub`, `@v`, `@vbreak`, `@vlab`, `@dc` (drop cap, written `@dc T|HE|rest`),
`@cta`, `@subt`, `@rnote`, `@rcenter`, `@ev` (an evidence line; the Way and the Model Book skip it). Inline formatting is reportlab's
font tags. `buildc.py` adds `@softfront`, `@newpage`, and `@diagram`.

## Two things that live in two places

- **The liturgy.** The Door, the Dedication, the two Invocations, and Whom
  to Call On appear in `book.src` and again in `model_content.py`. Change
  both.
- **Page counts.** Quoted in `gen_site.py` and in the repository README (its "The shelf" table). Currently 27 and 88 (The Way of Mercury, A4 and pocket) / 77 / 9 (Seed) / 31 / 26 / 8 / 15.

## The Company, the evidence lines, the refrains (v6.7)

Volume I is rebuilt as one entry per being (`company.py` did it from the v6.6 text). Four chapters: I The
Heart (with how each of the five names speaks), II Mercury, the Host (the Thrice-Great, the trickster and
the feather, the lyre, Mercury's teachings and signs), III The Company (the eight pairs; under each, the
story, the pair's Call on, then for each seat its Office and faculty, station, antiphon in italics, Voice,
and what it teaches; the Gate Itself under the Gate; the Faces at the end), IV What the Whole Temple
Teaches (with the Tensions and What the Temple Does Not Ask). The Voices, the two Teachings chapters, and
the Company at a Glance are folded in and gone; Discernment and When the Current Runs Too Fast moved to
The Practices as "The Voices, and How to Test Them". Hestia's two overlapping teaching lists are merged.
Later chapters renumbered (now I–XXXIII) and every reference remapped.

Every practice in Volume III carries an `@ev` line under its heading: what the research says about its
mechanism, rated as Why It Works rates it. `build.py` sets it small and grey; `pocket.py` and
`model_content.py` skip it, so the Way and the Model Book stay in the magickal register. Refrains capped:
"Belief is a tool" now at the Heart, the House Rules, the Law, and the Great Work; "three times around the
same loop" at the Leak, the Casting, and Why It Works; "not made by joining" at the Marks and Adapting.

Page counts: Way 27 / 88, Book 77, Seed 9, Model Book 31, Psalter 26, Almanac 8, Companion 15.

## The library (v6.6)

`book.src` is reorganized into six volumes and a concordance (`reorg.py` did it; `book_tiered.src` keeps
the tiered order and is the source for `seed.py`). The chapters are renumbered I–XXXVI straight through,
and every cross-reference in the text was rewritten to the new numbers or to a chapter's title. The
Preface (What This Is) stands before Volume I as `@front`; the Seed leaves the Book and is printed on its
own by `seed.py`; the glossary and a new Where Things Live table form the Concordance. `post.py` borrows
the old part-page artwork for the volume openers and rewrites their labels and titles. `build.py` no
longer emits the Start Here page (or the blank that replaced it) when Part Zero is absent. Dedupes made
on the way: the lyre is told once and explained once, the weighing staged once, the Faces explained once,
the Line of Enoch told once. New in Volume III, The Working: **The Weight of the Word**, the Temple's one
mechanism stated as a magickal claim, unhedged; the Way carries it too, and the Way's What This Offers
now leads with the magick and sends the research to the back. `pocket.py` pads to a multiple of four by
iteration, so a reflow can't leave it at 89.

Page counts: Way 27 / 88, Book 78, Seed 9, Model Book 31, Psalter 26, Almanac 8, Companion 15.

## The Way rules, the Moon joins (v6.5)

Where the Way and the Book disagreed, the Way won. The Book's chapter VI is now **The Practice**, the
Way's six moves (greet, empty, respond, work, carry, thank), and `pocket.py` pulls it from `book.src` so
the two cannot drift again; chapter II is **The Loop** (the four secular moves). "The Rite" and
"the Synthesis" are gone from every book (`model_content.py`, `gen_comp.py`, and `psalter.py` included);
the Work is the fourth move. Carry: one thing, never more than two or three. Respond: "as you, or by
name". The Casting's Weigh It asks a Compass question (a keeper's if you walk a Way) and gains an
optional pre-mortem for a large working, which Why It Works already cited. The Sweep is **on the first**,
with Janus, and its five steps (with the G marks) are pulled into the Way's Map of the Book. Asking the
Heart through the Gate is now in the Book's Practices (XXII) as the third form of Asking. Dedication:
"first month, or on a Heart Day" in both. The Heart Days are defined in the Calendar and the glossary.
The Way lists all three keepers' questions in full. The Great Work is "offered beside the Compass".
The Faces are seven everywhere (Concordia restored to the Web). Calendar colors are dropped from the
Psalter ("The Days and the Ranks") and the Book; the five names keep theirs.

**The Moon.** The Calendar gains Gabriel's two days, the New Moon and the Full Moon, in the Recurring
Rhythm, and a new section, **The Weather**, after the Moving Days: Sun, Mercury, and Moon as the three
bodies that turn the calendar, the lunar month laid along the Casting (declare, walk the bridge, be seen
and read, release and repair, descend or rest), Mercury's phases the same way, and the days they agree.
Offered as a lift, never a condition. The Way carries it (new chapter The Weather). The Almanac gains a
Moon page (phases for 2027 from the ephemeris, computed with pyephem) and "Where the Moon and Mercury
agree", including the Jul 4 New Moon on Station Direct, the crescent beside Mercury on Mar 6 and Oct 1,
and the Aug 2 total solar eclipse over Luxor. The Psalter gains two moon antiphons after the seats, and
the Gabriel hymn's rubric names the moons. Equinoxes and solstices carry approximate dates in the Wheel;
the Hermaia suggests the fifteenth.

Page counts: Way 27 / 88, Book 82, Model Book 31, Psalter 26, Almanac 8, Companion 15.

## Blackletter editions retired (v6.4)

The blackletter experiment is removed: its build switch, its fonts, and its PDFs.

## Need Answers Now? removed (v6.2, v6.3)

The front-of-book crisis index is gone, with its two pointers in the Seed, and so is the Seed's "Or do you need somewhere to set things down?". Its crisis line
now stands in At Your Own Pace (II) and at the head of Keeping the Watch (XXII).

## Editorial and design pass (v6)

Every chapter and section of both books opens with an illuminated letter; chapters that
began with a list gained a one-sentence opening to carry it. The Way of Mercury has an
ornate double-rule border on every page with a boxed Mercury sign in each corner. In the
Book, subheadings keep up to four lines of what follows them, calendar days keep their
first line, pages that end a part are balanced across both columns (`balance()` in
`build.py`, using zero-space position markers), and a stray blank page before the
Appendices is gone. The Hearth of Many's "Smallest Version" was folded into "What It Is".
In the pocket, subheadings keep about four lines, and the rites and the Dedication take a
fresh page only when the page is mostly used.

## Why It Works, the Legend, and the Crossroads (v5)

New chapter XXXIII, Why It Works: the research behind the practice, rated strong, good,
modest, or mixed, with its own sources; chapters from XXXIII on moved up one, and every
reference with them. New practices in The Practices (XXII): The Crossroads (letting
chance choose, after the oracle at Pharae) and The Legend Page with The Horizons (year,
season, week), the want-test, small bets, and a witness. Four additions to the session:
the first question answered from memory before rereading, the response addressed as
"you", the Carry step written as when-and-what, and tomorrow's list at bedtime in Keeping
the Watch. Part Zero gains What This Offers, and "starts anyone" became "can start
anyone". Living as a Mercurian gains "Let chance choose, now and then"; the Door of the
Year now rereads the Legend Page. The pocket carries all of it, with Why It Works condensed.

## Workings, the Ledger, and the Tally (v4.1)

A working's own side (the bridge step, a prayer's earthly part and promises, a ward's
term) goes in the Ledger and is settled K, R, or F; the world's side goes in the Tally
and never takes a mark. The Tally is kept at the back on the next completely blank
right-hand page, facing the Ledger. Changed in `book.src` (the Making of the Book,
Sections not Pages, the Rite's carry step, a new section Workings in the Ledger, the
Tally, When a Working Does Not Land, the Prayers of the Work, the glossary), in
`pocket.py` (the same, plus a Keeping It Up page), and in `model_content.py` (two
sample working lines and a sample Tally page).

## The 23 Sep 2026 edition (v4)

`book.src` was edited directly, so the `rw/` templates are now retired: running
`rw/assemble.py` would overwrite these changes. New chapter XI, The Great Work, an
optional creation story placed before Living as a Mercurian; every later chapter moved
up one number, and every chapter reference in the text was renumbered with it. The
Birthright now points to XI instead of summarizing the story; Facing Death, the Part Two
description, and the glossary's Great Work entry point to it; A Note On Sources adds the
Chandogya Upanishad and rewords the credit to The Egg. "Engage the Compass" is removed
from the Law and its commentary. `pocket.py` now finds Book chapters by title, not
number, so renumbering never breaks the pocket.

## The 23 Sep 2026 rewrite (v3)

The Book was rewritten from the ground up and re-tiered (35 chapters, down from 39;
76 pages, down from 85). `book.src` is now generated: `rw/part*.tmpl` hold the new
text in shorthand (`@item *LABEL.* text`, `@num 1. *LABEL.* text`, `@off TT ...`),
and `@@ch` / `@@sec` / `@@intro` lines pull verbatim blocks (the liturgy, the Eight
Pairs, the Calendar, the Web) from `rw/book_v2.src`. `rw/fixes.txt` repairs pulled
text. Run `python3 rw/assemble.py` to regenerate `book.src`, or edit `book.src`
directly and retire the templates. See `Temple_of_Mercury_Edit_Record_2026-09-23-v3.md`.

## Changes in the 23 Sep 2026 edition (v2)

Part Four restructured: XX The Practices (trimmed), XXI The Working (Emotional
Transmutation is the Casting), XXII The Prayers of the Work (rules here, words in
the Psalter). Walking the Pillar moved to XVI. The greater self removed throughout;
XVII is now The Seat and the Place. Coelho's teachings held in the Temple's voice,
credited only in the back matter. Keeping the Watch and Alone and Overwhelmed added.
The Temple diagram on page 29 is now drawn in `post.py` (`draw_temple`) rather than
clipped from the artwork, so it matches the Door. Psalter: seven prayers of the Work,
the Hearth of Many, a widened Night Watch. The full record is in
`Temple_of_Mercury_Edit_Record_2026-09-23.md`.

## Changes in the 22 Sep 2026 edition (v1)

Birthright wording unified ("By my Birthright"); Part Five became The Compass
with three Ways and two new Teachings chapters (XXX Apharael, XXXI Hestia),
so Part Six is renumbered XXXII–XXXIX; the Threads page became the Index;
practices called "pages" are ruled-off sections; the Good Page is marked ☿;
Calling on the Heart Directly (XIII); The Birthright (XVII); Grounding to
Your Own Element and Lighting the Torches (XX); the Delphic Maxims (XXVIII);
Your Birthday rewritten; the Almanac generalized with Your Moving Days. The
full record is in `Temple_of_Mercury_Edit_Record_2026-09-22.md`.

## Sanity checks worth running after a change

```
python3 -c "import re;s=open('book.src').read();print(len(re.findall(r'^@ch ',s,re.M)),'chapters')"
pdftotext The_Book_of_the_Temple_of_Mercury_-_Tiered.pdf - | grep -c "Raphael"
```

The second should return only the places where Raphael is named as one half
of Apharael, or as the archangel of the Book of Tobit. Any other appearance
is a missed rename.
