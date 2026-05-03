# Pending: Wichita State Special Collections

## Contact

- **Brent Adams** and Special Collections team
- Email: specialcollections@wichita.edu
- Phone: (316) 978-3590
- Address: Ablah Library, Lower Level, 1845 Fairmount St., Wichita, KS 67260-0068

## Status

Not yet contacted. Draft email below.

## What we're requesting

Materials from the **James E. Tomayko Collection of NASA Documents,
MS 87-08, Box 37**:

| FF# | Title | Why we want it |
|---|---|---|
| FF 6 | Initial FDS Processor Design | Earliest FDS spec (Wooddell era) |
| FF 8 | Response to FDS Flight Software Design | Engineering review record |
| FF 10 | FDS Flight Software Verification Procedure | How JPL tested the FDS code |
| FF 11 | FDS Flight Software Verification Procedure (Ellis, May 1978) | R. Ellis was author of the 1998 SCT-98-008 procedures we already cite |
| FF 13 | MJS 77 spacecraft memory usage and FDS Program Development Priorities | Memory budget rationale |
| FF 14 | FDS Processor Changes for Memory Utilization | The 8K-vs-4K transition |
| FF 16 | FDS Programming documentation | Possibly the missing assembler reference |
| FF 17 | In-Flight Reprogramming of FDS | Procedure for the patches we observe |
| FF 18 | "Reprogramming" the FDS in Flight | (sister doc) |
| FF 19 | MJS FDS Program Sequence Error Protection | Fault-protection logic |
| **FF 22** | **Design of CMOS Processor for Flight Data Subsystem (Wooddell, c.1974)** | **The full feasibility-design paper that Pietrobon obtained — our highest-priority single document** |
| FF 23 | Functional Requirement MJS77 — CCS Software | CCS analogue of FF 43 |
| FF 26 | General Command Format and MJS77 Flight Software Configuration Management | Format reference |
| FF 27 | CCS Flight Software Test Plan | Validation procedures |
| FF 30 | "CCS-AACS Handshaking or The Case of the Neurotic Computer" | Inter-subsystem protocol |
| FF 32 | Computer Command Subsystem MJS77 Software Products | Software inventory |
| FF 34 | CCS, FDS and AACS Simulation | The simulator JPL says it doesn't have anymore |
| FF 40 | Functional Requirement MJS77 — CCS Hardware | Hardware spec for CCS |

## Draft email

```
Subject: Voyager FDS / CCS materials request — open-source preservation project

Hi Brent and the Special Collections team,

I'm writing to request scans of additional folders in the
Tomayko Collection of NASA Documents (MS 87-08), Box 37,
following the path of Steven Pietrobon (FF 22) and
Zane Hambly (FF 41–43).

I'm a software engineer (no JPL or contractor affiliation)
working on a public open-source preservation project that
aggregates the engineering record of the Voyager 1 / 2 spacecraft
into a single citable archive. Repositories:

  github.com/scalecode-solutions/voyager-research  (the archive)
  github.com/scalecode-solutions/voyager-fds-forth  (a Forth-language
                                                     assembler/emulator
                                                     that consumes it)

The materials I'd like to request are:

  Box 37 FF 22 — "Design of a CMOS Processor for Use in the
                  Flight Data Subsystem of a Deep Space Probe"
                  (Wooddell, c.1974)

  Box 37 FF 6, 10, 11, 13, 14, 16, 17, 18, 19, 23, 26, 27,
              30, 32, 34, 40

…with FF 22 being the highest priority single document, since
it would let me cross-validate the empirical FDS instruction
encoding against the original feasibility-design specification.

I'm happy to pay scanning fees, follow any access procedures
you have, and credit Wichita State Special Collections (and you
specifically, if appropriate) in the resulting public materials.

I realise this is a substantial request. If it's easier to
prioritise, I'd be grateful for FF 22 first, and the rest as
your team has bandwidth to handle.

Thank you for everything you and Dr. Tomayko have already
preserved. The fact that we can do this work at all is because
of you.

Best regards,
Travis [...]
[email] [...]
[phone, if comfortable]
```

## After they respond

When materials arrive:
1. Place in `primary-sources/wsu-tomayko/`
2. OCR via `pdftotext -layout`, store in `transcribed/`
3. Update `wsu-tomayko/README.md` with the new contents
4. Cite their delivery date in `correspondence/`
5. Send a thank-you note
