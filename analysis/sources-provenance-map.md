# Source Provenance Map

Every architectural fact, instruction encoding, register, and address
range used in this project traces back to one of the public documents
listed here. No materials under JPL DISCREET access restriction or
otherwise non-public are used.

## Tier 1 — JPL primary sources (public)

### Cummings, D. *How We Diagnosed and Fixed the 2023 Voyager 1 Anomaly from 15 Billion Miles Away.*
- 18th Annual Flight Software Workshop, 25 March 2025, Seattle, WA.
- 39-slide PDF; CUI//SP-EXPT marking with explicit reviewer note that
  the document does not contain export-controlled CUI.
- Provides:
  - Complete timer interrupt handler disassembly (slide 9, addr 0x0000–0x002C)
  - SETMO routine disassembly (slide 13, addr 0x053C–0x0543)
  - MODE/CMODE values for all 10 data modes (slide 12)
  - Corrupted block disassembly with both pre- and post-failure values (slide 19, addr 0x1400–0x14FF)
  - MIN CMROT 267-word object code (slide 17)
  - JPL's own challenges quote: "no source code files / no assembler /
    no simulator / no testbed" (slide 23)
  - Patch 1 + Patch 2 mechanism: lists of `<address, value>` pairs (slide 31)

### Waggoner, B. *Saving Voyager 1!*
- August 2024 video presentation.
- First public showing of the corrupted memory block. Cummings'
  slides reproduce / extend Waggoner's listings.

### JPL Interoffice Memo VOYAGER SCT-98-008 (Ellis, R., 27 Feb 1998)
- *Procedure for Loading VIM7 Flight Software Into the Flight Data Subsystem (Revised)*
- Reproduced in Cummings slide 17.
- Contains the MIN CMROT macro call: 267 words of FDS object code,
  hand-traceable back to source.

## Tier 2 — Wichita State Special Collections (public, by request)

The James E. Tomayko Collection of NASA Documents at Wichita State
University Special Collections (Ablah Library) holds Tomayko's research
materials from his 1988 NASA history *Computers in Spaceflight*. Brent
Adams and the WSU Special Collections team respond to email requests
for digitised copies.

Contact: specialcollections@wichita.edu, (316) 978-3590.

### MS 87-08 Box 37 FF 22 — Wooddell, J. *Design of a CMOS Processor for Use in the Flight Data Subsystem of a Deep Space Probe.*
- Undated original; Wooddell dated it "~1974" from memory in
  correspondence cited by Tomayko (1988, p. 338).
- Source for the FDS register file structure (16/31/7/74 split) and
  most of the instruction set.
- Obtained by Steven Pietrobon and summarised in his NSF post
  #2640632 (12 Nov 2024).
- *Not* yet in this repo's `docs/`; requestable from WSU.

### MS 87-08 Box 37 FF 41 — *Computer Command Subsystem, Flight Software Design Description, Vol. I, Rev. G.*
- JPL document 618-235, 22 Jan 1980. 205 pages.
- Distribution list includes L. Zottarelli (the last original Voyager
  programmer; retired ~2016).
- Provides the 26 routine names of the CCS flight software (TRAPS,
  COINTS, AACSIN, TLMDRV, CHKSUM, OUTDRV, MEMLOD, GLBCNT, CMDPRC,
  ERROR, CMDLOS, IRSPWR, PWRCHK, RFLOSS, TARMEX, ANTUPD, DTRPRC,
  SCNPLT, TRNSUP, DMLOAD, LHRST, VARABL, PARM2A, PARM2B, PARM3A,
  PARM3B, DSSCAN), per-routine flow diagrams, command formats, and
  the ECR-numbered patch log 1977–1983.
- Already on local disk at
  `~/Github/voyager/voyager-fds-emulator/docs/MS 87-08 B37 FF41.pdf`
  (via Zane Hambly's emulator repo).
- Note: *Volume II* (the actual assembly listings) is referenced in
  this document but not in the public scan. Vol II would be the
  artifact of historical authenticity; Vol I is the engineering spec
  this project implements against.

### MS 87-08 Box 37 FF 42 — Wooddell, J. *MJS FDS Processor Architecture and Instruction Set.*
- JPL Interoffice Memo MJS:2.64A, 7 Oct 1974. 10 pages.
- The FDS architecture memo. Source for clock rate, register file,
  DMA channels, I/O counts, special-register address range.
- Already on local disk.

### MS 87-08 Box 37 FF 43 — *MJS77 Flight Equipment Flight Data Subsystem Hardware.*
- JPL document MJS77-4-2006-1A, 20 March 1978. 46 pages.
- FDS hardware functional requirements. Confirms 8K × 16 memory,
  hybrid module construction (2K × 16-bit per module).
- Already on local disk.

### Other Box 37 folders cited by Pietrobon
- FF 16 — *FDS Programming* (per Pietrobon)
- additional FDS-related folders exist; complete inventory pending
  WSU finding-aid request.

## Tier 3 — JPL Archives (public, by request)

The JPL Archives holds the master collection (JPL634, *Voyager Project
Collection, 1966–1990*, 258 Hollinger boxes). Most of the collection is
*not* under DISCREET restriction; outside researchers may request
digitised copies.

Contact: archives@jpl.nasa.gov.

### JPL634, Series 16 (Voyager CCS Documents, 1967–1977)
- **Folder 45** — *Computer Command Subsystem Flight Software Design
  Description, Assembly Language Listings, Rev. C, 4 Jan 1977.*
  This is the **earlier CCS analogue of FF41** with the assembly
  listings appended. Not yet requested.
- **Folder 547** — *Voyager CCS Flight Software Validation*, Aug 1977.
- **Folder 527** — *CCS + AACS Memory blueprints*, Apr 1975.

### JPL634, Series 1
- **Folder 144** — *Voyager FDS Flight Software Description, Vol. 2*,
  PD 618-236, 2 May 1977.
- **Folder 145** — *Voyager FDS Flight Software Description, Vol. 3*,
  PD 618-236, 2 May 1977.
- **Folder 46** — *AACS Software Changes for Post-Launch Load*, Dec 1977.

The 1988 Rev. A reissue of PD 618-236 is catalogued as JPL D-1772
(P. A. Barros, January 1988); cited in the PDS Ring-Moon Systems
Node references.

## Tier 4 — Synthesis sources (public, online)

### Pietrobon, S. NSF Forum post #2640632 (12 Nov 2024)
- The 36-instruction FDS opcode compilation reproduced in
  `lib/pietrobon-isa.md`.

### GEONius. *Voyager Spacecraft and Fortran 5.*
- http://www.geonius.com/writing/other/voyager.html
- Most thorough public synthesis of the Voyager flight-software
  architecture; combines Tomayko, Pietrobon, Cummings, Waggoner, and
  primary JPL documents into a single readable reference.

### Tomayko, J. E. *Computers in Spaceflight: The NASA Experience.*
- NASA SP-4317, March 1988. Chapter 6, Section 2.
- Public PDF on NTRS: 19880069935.
- Background on architecture rationale, register count, programming
  approach. Cites the Wooddell paper (Box 37 FF 22).

### Hackaday: *The Computers Of Voyager* (May 2024)
- Public-facing technical writeup; explicitly notes that the Wooddell
  CMOS Processor paper is "much harder to come by" and that the
  detailed instruction set is held only at WSU and JPL.

### All About Circuits: *The Brains of the Voyager Spacecraft* (July 2017)
- Confirms CCS = 18-bit, 6-bit opcode, 12-bit address, 64 instructions.

## Tier 5 — Inspiration (public, code)

### Zaneham/voyager-fds-emulator
- https://github.com/Zaneham/voyager-fds-emulator
- C-language FDS emulator built from FF41–FF43 scans. Apache-2.0.
- Same source documents, different implementation language.
- Acknowledges Brent Adams at WSU for digitising the FF41–FF43
  scans on request.

### aaroncummings/strange-loop-2019
- https://github.com/aaroncummings/strange-loop-2019
- Reference list from "Uptime 15,364 days — The Computers of Voyager"
  (Strange Loop 2019). 30+ NTRS / AIAA citations.
