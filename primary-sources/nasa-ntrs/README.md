# NASA NTRS — Voyager engineering papers and reports

Documents pulled from NASA's Technical Reports Server (NTRS),
NASA history publications, and related public archives. All are
US Government works in the public domain.

## What's in this folder

- `tomayko-1988-ch6-2-voyager.html` — *Computers in Spaceflight:
  The NASA Experience*, Chapter 6, Section 2 ("Distributed
  Computing On Board Voyager and Galileo / Voyager"). By
  James E. Tomayko, NASA SP-4317, March 1988. Captured from
  the [Wayback Machine](https://web.archive.org/web/20231123211500/https://history.nasa.gov/computers/Ch6-2.html)
  snapshot of the original `history.nasa.gov` page (the live URL
  has since been redirected to NTRS-only access).

The companion plain-text extraction is at
[`/transcribed/tomayko-1988-ch6-2-voyager.txt`](../../transcribed/tomayko-1988-ch6-2-voyager.txt).

## Why this matters

Tomayko's chapter is the single most authoritative *historical*
reference on Voyager's computer systems written outside JPL. He
interviewed the original engineers (Wooddell, Rice, Greenberg, Kohl,
Heacock, Lingon, etc.) and wrote in 1986–88, while the second
generation of programmers were still preparing Voyager 2 for the
Neptune encounter. The chapter contains technical and historical
details that don't appear anywhere else publicly:

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

These facts are now reflected in `analysis/voyager-team-history.md`
and other derived docs in this repository.

## Use status

NASA SP-4317 is a US Government publication in the public domain.
Redistributable for any purpose with appropriate attribution.

## Recommended citation

> Tomayko, James E. (1988). *Computers in Spaceflight: The NASA
> Experience*, Chapter 6, Section 2: "Voyager." NASA SP-4317.
> NASA History Series, Office of Management, NASA Headquarters,
> Washington, DC.
