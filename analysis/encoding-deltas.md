# Feasibility-vs-Flight Encoding Deltas

The Voyager FDS computer was specified in October 1974 as a
"feasibility design" by Jack Wooddell at JPL. The spacecraft
launched in August/September 1977, three years later. Per Wooddell
himself (paraphrased in Tomayko 1988):

> *Note that the paper addresses a "feasibility design" and was
> written in 1974, three years before the Voyager launches. Since
> changes may have been made in the interval, the paper may not be
> a totally accurate description of the final, as-built FDS computer.*

This document catalogs every place we observe the **as-built flight
binary** (from David Cummings' 25 March 2025 keynote disassembly)
diverging from the **feasibility specification** (from Wooddell's
1974 memo, FF42 in the WSU archive).

These deltas represent real engineering decisions made between
1974 and 1977 that, as far as we can tell, are not catalogued
publicly anywhere else.

## Sources

- **Feasibility (1974)**: Wooddell, *MJS FDS Processor Architecture
  and Instruction Set*, MJS:2.64A. See `lib/wooddell-isa.md`.
- **Flight (1977 onward)**: David Cummings, *How We Diagnosed and
  Fixed the 2023 Voyager 1 Anomaly from 15 Billion Miles Away*, 18th
  Annual Flight Software Workshop, 25 March 2025. Direct
  disassembly listings of in-flight binary on slides 9, 13, 19, 25.

## Confirmed deltas

### Δ1 — ISZ bit-9 inverted

**Feasibility encoding** (Wooddell Figure 2):
```
ISZ:  primary=1001, bits 12,11,10 = 1,1,1, bit 9 = 0, bit 8 = 1
```

**Flight encoding** (Cummings slide 9):
```
9FCF  ISZ PCTR    bits 12,11,10 = 1,1,1, bit 9 = 1, bit 8 = 1
9F86  ISZ PILLCT  same pattern
9F8B  ISZ S96SI   same pattern (slide 25)
```

Wooddell says bit 9 = 0; flight has bit 9 = 1. **Bit 9 inverted
between feasibility and flight.**

This may have been a pre-launch correction to disambiguate ISZ
from DSZ, since Wooddell's table appears to give them identical
encodings (both 1001 1 1 1 0 1).

### Δ2 — AND register-pair encoding inconsistent with ADD/LXR

**Slide 9 of Cummings' keynote shows:**
```
0000  9FCF  ISZ PCTR        operand $CF
0002  93E0  AND PCTR,R1     operand $E0
0006  900F  ADD R1,PCTR     operand $0F
0028  91FF  LXR PCTR,PCTR   operand $FF
```

If Wooddell's documented "R_B in bits 9-6, R_A in bits 4-1" applies
uniformly, and the assembler convention is "destination first" (per
data-flow analysis of slide 9 — `JMI R1` at $0007 requires R1 to
contain the dispatch table address):

| Instruction | R_B | R_A | Implied PCTR | Implied R1 |
|---|:---:|:---:|:---:|:---:|
| `ADD R1, PCTR` (900F) | 0 | F | F | 0 |
| `LXR PCTR, PCTR` (91FF) | F | F | F | — |
| `AND PCTR, R1` (93E0) | E | 0 | **E** | 0 |

PCTR appears as register F in ADD/LXR but as register E in AND.

**Three possible explanations**, in declining order of likelihood:

1. **Listing error** (Cummings' own caveat from slide 23: *"Source
   code listing in Microsoft Word with errors"*). Line 0002's
   mnemonic in the Word doc may have been transcribed as
   `AND PCTR,R1` when the actual instruction touches a different
   register.

2. **Encoding evolved** between feasibility and flight in a way
   that broke the simple R_B/R_A field placement for some sub-ops.

3. **Hidden register-aliasing convention** — perhaps a "PCTR
   shadow" register at index E that the AND specifically targets.

Resolution requires more 9-group data points from the MIN CMROT
disassembly or a future-rev Wooddell paper.

### Δ3 — SUB bit-5 inverted

Discovered while validating MIN CMROT disassembly.

**Feasibility encoding** (Wooddell Figure 2):
```
SUB:  primary=1001, bits 12,11,10 = 0,1,0, bit 5 = 0
```

**Flight encoding** (MIN CMROT, Cummings keynote slide 14):
```
9551  SUB R(A),R(1)
```

Decoded: bits 12-10 = 0,1,0 (SUB ✓); R_B = A; bit 5 = 1 (≠ Wooddell); R_A = 1.

For the result `R(1) := R(1) - R(A)` to be SUB (not some other op),
we need this bit-5 = 1 reading to be canonical. **Bit 5 inverted
between feasibility and flight, same pattern as Δ1 (ISZ).**

Possible explanation: JPL added a bit-5-set marker for arith
instructions that take asymmetric operands (SUB is the only
non-commutative one in ADD/LXR/AND/LOR/SUB — order matters), to
disambiguate them from a class that ignores order.

### Δ4 — Slide 25 LXR with sub-op nibble 0

**Slide 25 shows:**
```
103E  9094  EHP1L1  LXR R20LC,R20LC   /ZERO 20-LINE COUNTER
```

Decoded under Wooddell's spec:
- bits 11-8 = `0001` for LXR (sub-op = 0)... wait, **0094**'s
  bits 11-8 = `0000` which would be ADD per Wooddell.

The mnemonic `LXR R20LC,R20LC` (XOR self = zero a register)
contradicts a sub-op `0000` interpretation as ADD.

Most likely another listing error in the Cummings Word document.
The "intended" encoding for `LXR R20LC,R20LC` would be `9194`
(sub-op = 1 for LXR). 91FF on slide 9 confirms LXR uses sub-op 1.

## Empirically validated (no delta)

| Mnemonic | Wooddell encoding | Flight hex | Match |
|---|---|---|---|
| ADD | sub-op 0 | 900F | ✓ |
| LXR | sub-op 1 | 91FF | ✓ |
| AND | sub-op 3 | 93E0 | ✓ (encoding); see Δ2 for operand puzzle |
| SUB | sub-op 2 | 9551 | ✓ (slide 25 / MIN CMROT) |
| SLC | bits 12,11 = 1,0; 10-bit VALUE | 9B20 (VALUE=800) | ✓ |
| SKP | bits 12-8 = 1,1,0,0,0 | 9C52, 9C40 | ✓ |
| ISP | bits 12-8 = 1,1,0,1,1 | 9D83 | ✓ |
| JMP | primary 0 | 002B, 002D, … | ✓ |
| SRB | primary 1 | 1C00, 1C08, 1C09 | ✓ |
| EXC | primary 2 | 2C08 | ✓ |
| WAT | primary 3 | 3FFD, 305D | ✓ |
| MLD | primary 4 | 4F83 | ✓ |
| MRD | primary 5 | 5BE5, 5F84 | ✓ |
| PWD | primary 7, bit 12 = 1 | 7829, 7820 | ✓ |
| ABS | primary 8 | 801F, 8009, 8000 | ✓ |
| OUT | primary F | F087, F097, F09F | ✓ |

## Pending (Wooddell-specified, no flight test data yet)

These encoders are written from the Wooddell spec but have no
matching flight hex to validate against:

- LOR (sub-op 11)
- SKZ (sub-op E with bits 9-8 = 0,0)
- DSP (sub-op 6 with bits 9-8 = 0,1)
- DSZ (sub-op 7 with bits 9-8 = 0,1) — note overlap with ISZ (Δ1)
- SKO, SKC
- All shifts: SRS, SLS, SRR, ARS, LRS, LLS, LRR
- MCX, SKE
- AML, AMR
- SWI

When MIN CMROT is fully disassembled (we currently have ~16% of
the 267 words from Cummings slide 14), more of these will get
empirical validation — and we'll likely find more deltas to add
to Δ1, Δ2, Δ3.

## Bigger picture

If this catalog reaches 5+ confirmed deltas, it becomes a
historically meaningful artifact — a record of the engineering
changes JPL made between Voyager's initial CPU design (Wooddell
1974) and what actually flew (1977 onward). To the best of our
research, this delta record does not exist publicly anywhere else.
