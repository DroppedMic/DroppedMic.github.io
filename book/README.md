# Coaching the Lead Learner

Manuscript project for the book by Dr. Ramón M. González, founding
principal of MS/HS 223, The Laboratory School of Finance and Technology,
South Bronx.

## Production target

- Amazon KDP paperback and hardcover, 6 x 9 inches
- Kindle eBook (EPUB3)
- Target length 65,000 to 85,000 words, roughly 260 to 340 pages
- APA 7 citation style with end References section
- No em dashes or en dashes anywhere in the manuscript (house style)

## Layout

```
book/
  TABLE_OF_CONTENTS.md        approved outline
  metadata.yaml               pandoc metadata (title, author, isbn)
  references.bib              APA source library
  csl/apa.csl                 citation style
  manuscript/
    00_front_matter.md
    01_prologue.md
    02_chapter_01.md
    ...
    17_epilogue.md
    18_appendices.md
    19_references.md
    20_about_the_author.md
  build/
    paperback.pdf
    kindle.epub
  Makefile
```

## Build

```
make paperback     # 6x9 print-ready PDF
make kindle        # KDP-ready EPUB3
make all
make clean
```

## House style (do not violate)

1. No em dashes. No en dashes. Use commas, periods, semicolons, or
   recast the sentence.
2. Contractions welcome. First person singular. Second person direct
   address permitted when speaking to the practitioner reader.
3. Every chapter opens with a scene, not a thesis.
4. Every major claim is cited in APA in-text style.
5. No triadic abstraction lists. No "landscape" or "tapestry" or
   "journey" metaphors. No "in today's world." No "unpack." No "delve."
6. Variation in sentence length is required. A run of six balanced
   sentences is a red flag.
7. Names of real children are not used. Composite students are labeled
   in the Author's Note.
