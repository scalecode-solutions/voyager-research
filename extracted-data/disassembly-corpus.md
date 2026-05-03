# Voyager FDS Disassembly Corpus

Every `(hex, mnemonic)` pair we have empirically observed in actual
Voyager flight binary, with citation to the original public source.

This is the ground truth our assembler and disassembler are
validated against.

## Citation key

| Key | Source |
|---|---|
| C-9 | Cummings keynote slide 9 (timer interrupt handler at 0x0000–0x002C) |
| C-13 | Cummings keynote slide 13 (SETMO routine at 0x053C–0x0543) |
| C-14 | Cummings keynote slide 14 (MIN CMROT object code excerpt) |
| C-19 | Cummings keynote slide 19 (corrupted block 0x1400–0x14FF) |
| C-25 | Cummings keynote slide 25 (Phase 1 patch examples) |
| C-vid-71100 | Cummings YouTube frame at ~39.5 min (EL-40 patch addresses) |
| C-vid-75600 | Cummings YouTube frame at ~42 min (skip / register-B warnings) |
| W-screen-06 | Waggoner !!Con screenshot 06_corrupted-block-disasm.png |

## The corpus

### Address 0x0000–0x002C (C-9): Timer interrupt handler

| Address | Hex | Mnemonic | Operand | V-number |
|:-------:|:---:|:---------|:--------|:--------:|
| 0000 | 9FCF | ISZ | PCTR | V5001160 |
| 0001 | 801F | ABS | 31 | V5001180 |
| 0002 | 93E0 | AND | PCTR,R1 | V5001190 |
| 0003 | 9C52 | SKP | CMODE | V5001200 |
| 0004 | 002B | JMP | GOUP | V5001210 |
| 0005 | 8009 | ABS | PTAB | ECR100073 |
| 0006 | 900F | ADD | R1,PCTR | V5001240 |
| 0007 | 0FC0 | JMI | R1 | V5001250 |
| 0008 | 11AF | CON | $X11AF (PROGID) | V5001260 |
| 0009 | 0028 | JMP | PILL (PTAB start) | V5001310 |
| 000A | 002D | JMP | P1 | V5001320 |
| 000B | 0185 | JMP | P2 | V5001330 |
| 0020 | 0486 | JMP | P23 | V5001540 |
| 0021 | 048F | JMP | P24 | V5001550 |
| 0022 | 0028 | JMP | PILL | V5001560 |
| 0028 | 91FF | LXR | PCTR,PCTR | V5001660 |
| 0029 | 9F86 | ISZ | PILLCT | V5001680 |
| 002A | 3FFD | WAT | 4095 | V5001760 |
| 002B | F087 | OUT | 7,SETJU | V5001800 |
| 002C | 0000 | JMP | UPINT | V5001810 |
| 1000 | F097 | OUT | 7,SETAU (UPINT) | V5060390 |

### Address 0x053C–0x0543 (C-13): SETMO routine

| Address | Hex | Mnemonic | Operand | V-number |
|:-------:|:---:|:---------|:--------|:--------:|
| 053C | 1C08 | SRB | LMRA (SETMO) | V5031230 |
| 053D | 9B20 | SLC | 800 | V5031240 |
| 053E | 0543 | JMP | SETMOA | V5031250 |
| 053F | 8000 | ABS | 0 | V5031270 |
| 0540 | 4F83 | MLD | SKIPPR | V5031280 |
| 0541 | 305D | WAT | 95 | V5031290 |
| 0542 | 2C08 | EXC | LMRA | V5031300 |
| 0543 | 5BE5 | MRD | MODE (SETMOA) | V5031320 |

### Address 0x1400–0x14FF (C-19, W-screen-06): Corrupted memory block

Format: `addr  pre-failure-hex  post-failure-hex (bit-5-stuck)  mnemonic  operand  V-number`

| Address | Hex | Mnemonic | Operand | V-number |
|:-------:|:---:|:---------|:--------|:--------:|
| 13DE | 1C00 | SRB | UMRA (GETEID start) | V5079310 |
| 1400 | 7829 | PWD | RMFC,R2 | V5079770 |
| 1461 | 1C09 | SRB | LMRAN (MOD15 start) | V5081080 |
| 1469 | 9C40 | SKP | R1 | (Phase-1 example) |
| 146A | 9D83 | ISP | SKIPPR | (Phase-1 example) |
| 146B | 7820 | PWD | R1,R2 | (Phase-1 example) |
| 146D | F09F | OUT | 7,SETAD (COPY start) | V5081380 |
| 1494 | F09F | OUT | 7,SETAD (SAFECA) | V5081990 |
| 14B0 | 5F84 | MRD | AUTOCF (PLSCL start) | V5082500 |
| 14BF | 5BED | MRD | PLSCWB | (Phase-1 example) |
| 14C0 | 1C31 | SRB | NEWPLS | (Phase-1 example) |
| 14C1 | 04C8 | JMP | GSNOCA | (Phase-1 example) |
| 14CA | 1C08 | SRB | LMRA (PLSCHK start) | V5082830 |
| 14F0 | 37AF | CON | $X37AF (PLSTBL) | V5083250 |
| 14F1 | ADFA | CON | $XADFA | V5083260 |
| 14F2 | FA7F | CON | $XFA7F | V5083270 |
| 14F3 | AFB0 | CON | $XAFB0 (PLTZ; '0' not used) | V5083280 |
| 14F4 | F09F | OUT | 7,SETAD (ULINK) | V5083400 |
| 14F8 | 03DE | JMS | GETEID (XGETEI) | V5083470 |
| 14F9 | 050A | JMP | UVECTR | V5083480 |
| 14FA | 0231 | JMS | CMROT (XCMROT) | V5083490 |
| 14FB | 050A | JMP | UVECTR | V5083500 |
| 14FC | 02B6 | JMS | CHSUM (XCHSUM) | V5083510 |
| 14FD | 050A | JMP | UVECTR | V5083520 |
| 14FE | 02EF | JMS | LTS (XLTS) | V5083530 |
| 14FF | 050A | JMP | UVECTR | V5083540 |

### Phase-1 patch addresses (C-vid-71100): EL-40 jump cleanup

| Address | Hex | Mnemonic | Operand |
|:-------:|:---:|:---------|:--------|
| 11BD | 00D6 | JMP | ELP21 |
| 11BE | 00D4 | JMP | ELP22 |
| 11BF | 01E8 | JMP | ELP23 |
| 11C0 | 01EB | JMP | ELP23 |

### Slide 25 (C-25): EHP1 routine

| Address | Hex | Mnemonic | Operand |
|:-------:|:---:|:---------|:--------|
| 103C | 9F8B | ISZ | S96SI |
| 103D | 0054 | JMP | EHP1Z |
| 103E | 9094 | LXR | R20LC,R20LC ⚠ anomaly |

⚠ The 9094 LXR encoding contradicts our otherwise-confirmed
LXR sub-op = 1. Likely a Word-listing typo per Cummings'
explicit caveat that the listing has errors. See
`analysis/encoding-deltas.md` for discussion.

### MIN CMROT object code excerpt (C-14)

44 visible 16-bit words out of the routine's 267 total. Address
of load-point not shown on the slide.

```
0E72  5F7A  9FC0  E401  9551  1F7A  8E83  9001  0FC0
8000  4F7A  8001  4FD4  4FD3  4F7D  8F6F  4F74
3FFD  0E7A  0EA3  0E82  0E82  0EAA  0E82  0E82
[ ... ~223 unrecorded words ... ]
4F77  3003  0F70  7310  0F70  0F6A  1F74  0E82
301F  0E82  0F6F  0391  5ED3  0000  8400  BA00
001A  0000  0000
```

## Subroutines in the corrupted block (C-vid-82800)

From the Statistics-by-Armen-Arslanian slide. Subroutines that
lived in the failed 0x1400–0x14FF range:

| Subroutine | Pieces | Notes |
|---|---|---|
| GETEID | 3 (split for relocation) | starts at 0x13DE, extends past 0x1460 |
| MOD15 | 2 | starts at 0x1461 |
| COPY | (NOT relocated) | NOPed out — `JMP COPY` replaced with `WAT 344` |
| SAFECA | 1 | at 0x1494 |
| PLSCL | 3 | starts at 0x14B0 |
| PLSCHK | 1 | at 0x14CA |
| PLSTBL | 1 | at 0x14F0 (data, not code) |
| ULINK | 1 | at 0x14F4 |
| XGETEI / XCMROT / XCHSUM / XLTS | 1 (jump table) | 0x14F8–0x14FF |

Patch statistics:
- EH-12 space available: 41%
- EH-12 space used by relocated EL-40 code: 39%
- Modified instructions: 17%

## How this corpus is used

The Forth assembler at
[scalecode-solutions/voyager-fds-forth](https://github.com/scalecode-solutions/voyager-fds-forth)
contains a test (`tests/encoder-test.fs`) that, for every entry in
this corpus where an encoding is fully understood, asserts that
calling our encoder with the documented operand produces the
documented hex byte. As of repo commit 017ec9a, **40 / 40** test
cases pass.

Every additional flight-binary fragment we recover (from new MIN
CMROT decoding, new keynote slides, future memory dumps released
publicly) gets added here and to the encoder test, expanding the
empirically-validated subset of the FDS instruction set.
