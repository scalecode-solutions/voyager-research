# NASA NTRS — Voyager engineering papers and reports

Documents pulled from NASA's Technical Reports Server (NTRS),
NASA history publications, and related public archives. All are
US Government works in the public domain.

## What's in this folder

### `tomayko-1988-full.pdf` ★ canonical
- *Computers in Spaceflight: The NASA Experience*
- James E. Tomayko, NASA SP-4317, March 1988
- 406 pages, complete book with all figures
- NTRS document ID: **19880069935**
- Sourced from NASA NTRS

### `tomayko-1988-ch6-2-voyager.html`
- Just Chapter 6 Section 2 ("Voyager"), HTML rendition
- Captured from the [Wayback Machine](https://web.archive.org/web/20231123211500/https://history.nasa.gov/computers/Ch6-2.html)
  snapshot of the original `history.nasa.gov` page (the live URL
  has since been redirected to NTRS)
- Faster to grep than the full PDF; all of its content is also
  in the full PDF

The companion plain-text extractions are at:
- [`/transcribed/tomayko-1988-full-book.txt`](../../transcribed/tomayko-1988-full-book.txt) (17,939 lines, full book)
- [`/transcribed/tomayko-1988-ch6-2-voyager.txt`](../../transcribed/tomayko-1988-ch6-2-voyager.txt) (266 lines, Voyager chapter only)

## Why this matters

Tomayko's book is the single most authoritative *historical*
reference on NASA spacecraft computers. He interviewed the
original engineers across multiple programs (Apollo, Skylab,
Shuttle, Viking, Voyager, Galileo) and wrote in 1986–88 while
many of them were still active. The book is the canonical
secondary source for facts that don't appear elsewhere publicly.

For our project specifically, the **Voyager** content is
**Chapter 6 Section 2** (pages ~171-210 of the PDF). It contains:

- Specific power codes between AACS and CCS (code 37 = heartbeat;
  code 66 = "the Omen", disaster-imminent signal)
- HYPACE execution-rate scheduling (10 / 20 / 60 / 240 ms periods)
- The 16-second-after-separation V2 AACS auto-switch incident
- Context for why CMOS volatile memory was chosen and how the
  power-retention argument (separate DC line) won approval
- Wooddell's design history, including the USC graduate paper
  that became the de-facto FDS specification
- The fact that an earlier FDS readout-register failure (1981)
  produced the same "stuck bit" symptom as the 2023 anomaly
- 18 software loads were uplinked to V1 during the Jupiter
  encounter alone

These facts are reflected in `analysis/voyager-team-history.md`
and other derived docs in this repository.

The PDF additionally has:

- **Figure 6-1**: Voyager spacecraft with RTG and scan platform
- **Figure 6-2**: FDS hardware package
- **Box 6-1**: HYPACE Operation (the four-rate executive)
- **Box 6-2**: Voyager FDS Computer Architecture
- **Box 6-3**: FDS Computer Executive (P-period scheduling)

…all of which the HTML rendering may have linked to but didn't
inline.

## Use status

NASA SP-4317 is a US Government publication in the public domain.
NTRS document 19880069935. Redistributable for any purpose with
appropriate attribution.

## Recommended citation

> Tomayko, James E. (1988). *Computers in Spaceflight: The NASA
> Experience*. NASA SP-4317. NASA History Series, Office of
> Management, NASA Headquarters, Washington, DC.
> NTRS: 19880069935.
> Available at: https://ntrs.nasa.gov/citations/19880069935
