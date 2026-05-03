# MIN CMROT — structural analysis from the visible bytes

The Minimum Coded Command Routine (MIN CMROT) is the small program
JPL uplinked to Voyager 1 in March 2024 to recover telemetry after
the FDS memory anomaly. It originated in JPL Interoffice Memorandum
**VOYAGER SCT-98-008** (R. Ellis, 27 February 1998), titled
*Procedure for Loading VIM7 Flight Software Into the Flight Data
Subsystem*.

The full routine is **267 16-bit words**. Cummings keynote slide 14
shows the first 25 words and the last 19 words of its load-time
binary representation, with the middle ~223 words elided. From those
44 visible words we can infer the routine's structure even though
we don't have the source mnemonics.

## Visible bytes (Cummings keynote slide 14)

```
Words  0..  8 :  0E72  5F7A  9FC0  E401  9551  1F7A  8E83  9001  0FC0
Words  9.. 16 :  8000  4F7A  8001  4FD4  4FD3  4F7D  8F6F  4F74
Words 17.. 24 :  3FFD  0E7A  0EA3  0E82  0E82  0EAA  0E82  0E82
                 [ ... ~223 unrecorded words ... ]
Words 25.. 32 :  4F77  3003  0F70  7310  0F70  0F6A  1F74  0E82
Words 33.. 43 :  301F  0E82  0F6F  0391  5ED3  0000  8400  BA00  001A  0000  0000
```

(Indices here are sequential in the visible portion, NOT memory
addresses; we don't know where MIN CMROT was loaded.)

## Disassembled

Each word decoded by the
[voyager-fds-forth](https://github.com/scalecode-solutions/voyager-fds-forth)
disassembler (`fds-disasm.fs`):

```
$000  $0E72  JMP   $E72
$001  $5F7A  MRD   $F7A                      ; read special reg
$002  $9FC0  ISZ   [reg+$40]                 ; counter at $F80+$40 = $FC0 (R1)
$003  $E401  LRS/ARS  raw=401                ; long arithmetic right shift
$004  $9551  SUB   R(A), R(1)                ; R_A := R_A - R_B
$005  $1F7A  SRB   $F7A                      ; save R_B to special reg
$006  $8E83  ABS   $E83                      ; load constant
$007  $9001  ADD   R(0), R(1)                ; R(1) += R(0)
$008  $0FC0  JMP   $FC0                      ; indirect through R1 (= JMI)
$009  $8000  ABS   $000                      ; load 0
$00A  $4F7A  MLD   $F7A                      ; mem[F7A] := R_A   (= 0)
$00B  $8001  ABS   $001                      ; load 1
$00C  $4FD4  MLD   $FD4                      ; mem[FD4] := R_A   (= 1)
$00D  $4FD3  MLD   $FD3                      ; mem[FD3] := R_A
$00E  $4F7D  MLD   $F7D                      ; mem[F7D] := R_A
$00F  $8F6F  ABS   $F6F                      ; load $F6F
$010  $4F74  MLD   $F74                      ; mem[F74] := $F6F  (pointer init?)
$011  $3FFD  WAT   4093                      ; wait
$012  $0E7A  JMP   $E7A                      ; subroutine call
$013  $0EA3  JMP   $EA3                      ; subroutine call
$014  $0E82  JMP   $E82                      ; subroutine call
$015  $0E82  JMP   $E82                      ; (same target again)
$016  $0EAA  JMP   $EAA                      ; subroutine call
$017  $0E82  JMP   $E82
$018  $0E82  JMP   $E82
                 [ ... gap ... ]
$025  $4F77  MLD   $F77                      ; (R_A whatever it currently holds)
$026  $3003  WAT   3                          ; short wait
$027  $0F70  JMP   $F70                      ; JMI through reg at $F70?
$028  $7310  SWO   raw=310                   ; SERIAL WRITE OUT — telemetry !!
$029  $0F70  JMP   $F70                      ; JMI through reg again
$02A  $0F6A  JMP   $F6A
$02B  $1F74  SRB   $F74                      ; save R_B to F74
$02C  $0E82  JMP   $E82
$02D  $301F  WAT   31
$02E  $0E82  JMP   $E82
$02F  $0F6F  JMP   $F6F
$030  $0391  JMP   $391
$031  $5ED3  MRD   $ED3
$032  $0000  JMP   $000                      ; loop back to start?
$033  $8400  ABS   $400
$034  $BA00  SKE   raw=A00                   ; skip if equal (comparison)
$035  $001A  JMP   $01A
$036  $0000  JMP   $000
$037  $0000  JMP   $000
```

## Inferred structural anatomy

```
┌────────────────────────────────────────────────────────────────────┐
│ MIN CMROT (267 words total; ~16% recovered visible)                │
├────────────────────────────────────────────────────────────────────┤
│ $00.$08   Entry / dispatch                                         │
│           JMP through dispatch table; ISZ R1 counter; arithmetic   │
│           on R1 to compute table-entry address; indirect jump      │
│                                                                    │
│ $09.$10   State initialization                                     │
│           Sequence of (ABS const ; MLD addr) pairs initializing    │
│           special registers F7A, FD4, FD3, F7D, F74 to known       │
│           values. F74 gets the pointer $F6F (likely a self-ref     │
│           or buffer-pointer setup).                                │
│                                                                    │
│ $11       WAT 4093                                                 │
│           Long delay (~10 ms at 403 kHz cycle).                    │
│                                                                    │
│ $12.$18   Subroutine call chain                                    │
│           Series of JMPs to upper-memory subroutines in the        │
│           $E__ range. The $0E82 address recurs four times,         │
│           suggesting a single utility routine called repeatedly.   │
│                                                                    │
│ [gap]     ~223 unrecorded words                                    │
│                                                                    │
│ $25.$2F   Telemetry emission                                       │
│           - Init via MLD F77                                       │
│           - Brief WAT 3                                            │
│           - Indirect call (JMI through F70) - twice                │
│           - SWO instruction at $28: actually transmits telemetry   │
│           - More JMI calls (F70, F6A) for routing                  │
│           - SRB to F74 (save return)                               │
│                                                                    │
│ $30.$33   More subroutine work                                     │
│           Branches and waits and reads (MRD ED3).                  │
│                                                                    │
│ $34.$37   Loop / termination                                       │
│           SKE compares; JMP $01A (back into the routine);          │
│           JMP $000 (loop to top); padding.                         │
└────────────────────────────────────────────────────────────────────┘
```

## What this confirms

1. **MIN CMROT is centered on `SWO` at offset $28** — that single
   instruction is the moment the spacecraft actually transmits a
   telemetry frame. Cummings called it "Hello World" telemetry, and
   the SWO is the "world" being said.

2. **The routine is a continuous emission loop.** `JMP $000` at $32
   and again at $36-37 make this clear. The ABS/MLD/SKE/JMP $01A
   fragment at $33-35 looks like a conditional re-entry that skips
   the initial state-init when looping (otherwise you'd reset state
   every cycle).

3. **Heavy use of JMI through special-register pointers** ($F70,
   $F74, $F6A, etc.). This is consistent with how Voyager engineers
   reused common subroutines — store the routine address in a known
   register, JMP through it. Indirect jump tables are the language's
   mechanism for what we'd call function pointers.

4. **The $0E82 address being called 4+ times** in the visible
   chunk strongly suggests a *checksum / parity / framing* utility
   that gets invoked multiple times per telemetry frame. Without
   the actual source we can't name it, but the call frequency is
   diagnostic.

## What we still don't have

The 223 missing words contain:
- The body of the dispatch routine (likely a table of addresses)
- The subroutines at $E72, $E7A, $EA3, $E82, $EAA targeted by JMPs
- The state-machine that drives the SWO transmission
- Any conditional logic (SKP/ISP/SKZ/ISZ) - we see only one SKE
- Probably the full set of wait counts and timing constants

Cummings stated that JPL spent "by mid January" disassembling and
reverse-engineering MIN CMROT to identify the SWO instructions for
telemetry. With our disassembler, the visible 44 bytes confirm
their analysis but don't recover the missing 223. Recovery would
require either:

1. JPL releasing the full hex listing (low probability — they'd
   need to clear it as not-export-controlled)
2. Watching for additional public material (future Cummings or
   Waggoner talks may show more)
3. Acquiring the full SCT-98-008 memorandum from JPL Archives
   (the original source contains the full byte sequence)

Item 3 is the realistic path; the document is in the JPL Archives
as part of the Voyager Project Collection (we have its finding aid
in `primary-sources/jpl-archives/`).
