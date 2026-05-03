# voyager-research

**A consolidated, citable archive of public-domain and openly-licensed
Voyager 1 / 2 spacecraft engineering documentation, with source
provenance, transcriptions, and derived analysis.**

This repository exists because the engineering record of Voyager — the
deepest-running computer system in human history — is preserved in
fragments scattered across:

- JPL contractor archives (mostly accessible by appointment)
- One library at Wichita State University (Tomayko Collection)
- Personal websites and forum posts of retired engineers
- Conference talks recorded on YouTube
- NASA's PDS data products

If any one of those fragments goes offline, that piece of Voyager's
engineering record gets harder to recover. By aggregating the
publicly-available subset into a single, version-controlled, citable
archive, we increase its survival probability.

## Sister project

The hands-on code that consumes this archive — a Forth-language
assembler, disassembler, and CPU emulator for the Voyager Flight Data
Subsystem — lives at:

> [scalecode-solutions/voyager-fds-forth](https://github.com/scalecode-solutions/voyager-fds-forth)

The two repositories are designed to work together. This one preserves
*what we know*; that one operationalises *what we can do with it*.

## Repository layout

```
primary-sources/                 Original documents as obtained
├── wsu-tomayko/                 Wichita State, Tomayko Collection MS 87-08
├── jpl-archives/                JPL Archives finding aids
├── jpl-cummings-keynote-2025/   David Cummings, FSW Workshop Mar 2025
├── jpl-waggoner-iicon-2024/     Bruce Waggoner, !!Con Aug 2024
├── nasa-dsn-rsc-11-6/           DSN Radio Science Software Interface Spec
└── nasa-ntrs/                   NASA Technical Reports Server papers

transcribed/                     Plain-text extractions from the PDFs/videos
extracted-data/                  Disassembly listings, byte tables, etc.
analysis/                        Our derived works (CC-BY-4.0)
correspondence/                  Logs of records requests we've made
```

Each `primary-sources/` subdirectory has its own README explaining
the materials, their origin, and their licence/use status.

## Provenance and licence

**Primary sources** are stored as the originating institution made them
available. All are believed to be redistributable for research and
educational use:

- **Wichita State Tomayko Collection PDFs** — scanned by WSU Special
  Collections; previously published in the public
  [voyager-fds-emulator](https://github.com/Zaneham/voyager-fds-emulator)
  GitHub repository with the explicit permission of WSU.
- **JPL634 finding aid** — JPL Archives publishes finding aids
  publicly.
- **Cummings 2025 keynote** — distributed as a public conference
  presentation with the CUI review marking *"This document has been
  reviewed and determined not to contain export controlled CUI."*
- **Waggoner !!Con 2024 transcript and screenshots** — auto-captions
  retrieved via yt-dlp (research/commentary fair use); screenshots
  used as research excerpts.
- **DSN RSC-11-6 specification** — NASA-PDS public documentation.

**Our derived analysis** in `analysis/` and the project structure
itself are licensed CC-BY-4.0 (see [LICENSE](LICENSE)).

We **do not** redistribute:

- Full video files of copyrighted talks (we link to the originals)
- Materials with explicit redistribution restrictions
- Anything under JPL DISCREET access control

## How to cite this archive

When using the materials here in academic or technical work, please
preserve the original source citation (each PDF and document has its
JPL document number, paper DOI, or equivalent identifier) and
optionally add this archive as a secondary reference:

> Voyager Research Archive (2026). github.com/scalecode-solutions/voyager-research

## Status

Initial scaffold pushed May 2026. Active. Incomplete by design — this
is meant to grow as more public documents are aggregated.

## Contributing

If you have publicly-redistributable Voyager engineering documents
that aren't here, or noticed an error in our transcriptions or
analysis, open an issue or pull request.

If you have access to materials that **can't** be publicly
redistributed but you can confirm their existence, open an issue
documenting their location and access procedure — that's still
valuable to record.

## Acknowledgements

This archive stands on the shoulders of:

- **Dr. James E. Tomayko** (Carnegie Mellon, 1949–2006), whose
  personal archiving in the 1980s preserved what JPL itself has now
  lost.
- **Brent Adams and the Wichita State Special Collections team**, who
  digitise and email scanned documents to anyone who asks.
- **Steven Pietrobon** (Small World Communications), who reverse-
  engineered the FDS instruction set publicly.
- **Jack Wooddell, Richard J. Rice, Stan Lingon, Larry Zottarelli,
  and the original Voyager engineering team**, whose 1970s
  documentation is what we're aggregating.
- **David Cummings, Bruce Waggoner, and the 2024 Voyager 1 anomaly
  team**, whose public talks revealed disassembled flight code that
  hadn't been published before.
- **Zane Hambly**, whose voyager-fds-emulator was the precedent for
  hosting this material on GitHub.
- **GEONius** (anonymous web author), whose exhaustive synthesis of
  all of the above is the most thorough single Voyager-architecture
  reference on the public web.

This archive aims to reduce the chance that any of their work gets
lost again.
