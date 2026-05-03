# David Cummings — *How We Diagnosed and Fixed the 2023 Voyager 1 Anomaly from 15 Billion Miles Away*

## Source

- **Speaker:** David M. Cummings, JPL
- **Venue:** 18th Annual Flight Software Workshop, hosted by Stoke
  Space, 25 March 2025, Seattle WA
- **YouTube ID:** `YcUycQoz0zg`
- **NTRS citation:** [23548079930626](https://ntrs.nasa.gov/citations/23548079930626)

## What's in this folder

- `keynote-slides.pdf` — the 39-slide PowerPoint export, distributed
  as a public conference handout. Each page carries the marking
  *"This document has been reviewed and determined not to contain
  export controlled CUI."*
- `frames/` — 161 still frames extracted from the YouTube video at
  scene-change points and 30-second intervals. Used to verify the
  PDF transcription and to capture content (e.g., the
  Statistics-by-Armen-Arslanian slide) that the PowerPoint→PDF
  flatten-pass elided.

The full video is **NOT** redistributed here (YouTube content
copyright). Watch it directly at the YouTube link above.

## Why this matters

This 2025 keynote contains the first publicly disassembled
fragments of actual Voyager 1 flight binary that JPL has released:

- The cyclic executive at addresses 0x0000–0x002C (slide 9)
- The SETMO routine at 0x053C–0x0543 (slide 13)
- The corrupted memory block at 0x1400–0x14FF, with both pre- and
  post-failure values (slide 19)
- 44 visible bytes of the MIN CMROT object code (slide 14)
- MODE/CMODE values for all 10 data modes (slide 12)

Together these enable empirical validation of the FDS instruction-set
encoding against the *as-built flight* version (as opposed to the
1974 feasibility-design specification in `wsu-tomayko/FF42.pdf`).

## Use status

Conference presentations distributed publicly, with explicit non-CUI
marking, may be used and excerpted for research and commentary
(fair use). We treat the slide contents as quotable and the frames
as research excerpts.
