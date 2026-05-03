# FDS Instruction Set — Wooddell 1974 Specification

Direct transcription of Figure 2 ("FDS INSTRUCTION SET") from:

> Wooddell, J. *MJS FDS Processor Architecture and Instruction Set.*
> JPL Interoffice Memorandum MJS:2.64A, 7 October 1974.
> Wichita State University Special Collections,
> James E. Tomayko Collection, MS 87-08 Box 37 Folder 42.

This is the **feasibility design** specification. Per Pietrobon's
quote of Wooddell (paraphrased through Tomayko's *Computers in
Spaceflight*, 1988):

> *Note that the paper addresses a "feasibility design" and was
> written in 1974, three years before the Voyager launches. Since
> changes may have been made in the interval, the paper may not be
> a totally accurate description of the final, as-built FDS computer.*

For deltas between this 1974 specification and the as-built flight
behavior we observe in Cummings' 2025 keynote, see
`docs/feasibility-vs-flight.md`.

## Bit numbering convention

Wooddell uses **1-indexed** bit numbering with bit 16 = MSB and
bit 1 = LSB. To translate to common 0-indexed (where bit 0 = LSB):

> 0-indexed bit *n* = Wooddell bit (*n* + 1)

For example, "bit 5" in this document is 0-indexed bit 4.

## Architecture (point 11 of Wooddell's introduction)

> *Special purpose registers: There are 16 general registers
> (GR1-GR16), 32 memory pointers (MP1-MP32), 8 index registers
> (IR1-IR8), and a line count register. **GR1, MP1, and IR1 refer
> to the same register.** All others are distinct. An additional
> 73 registers are available for use as counters. These registers
> reside in the top 128 memory locations.*

```
  16 GR + 32 MP + 8 IR – 2 overlaps + 1 line count + 73 counters = 128
```

The 128 special registers occupy memory addresses 0xF80–0xFFF
(the top 128 words of the lower 4K bank).

## Hardware registers

> *1. A 16-bit instruction buffer register (IB).
> 2. A 12-bit program address register (PR).
> 3. Two 16-bit working registers (R_A and R_B).*

Operations on the GP registers conceptually load values into R_A
and R_B, perform the operation, and write the result back. The
instruction encoding's `R_A` and `R_B` fields specify which GP
register to use as each operand.

## Instruction Set (Figure 2)

Bit positions 16-13 12 11 10 9 8 7 6 5 4 3 2 1 (Wooddell numbering).

### Memory and control flow

| Cycles | Mnemonic | bits 16-13 | bits 12-1 | Function |
|:------:|:---------|:----------:|:---------:|:---------|
| 2 | JMP | 0000 | ADDRESS | Jump |
| 3 | SRB | 0001 | ADDRESS | Save R_B Register |
| 2 | EXC | 0010 | ADDRESS | Execute |
| 4097 | WAT | 0011 | CYCLE COUNT | Wait |
| 3 | ABS | 1000 | DATA | Absolute Entry |
| 4 | MLD | 0100 | ADDRESS | Load Memory |
| 4 | MRD | 0101 | ADDRESS | Read Memory |

### I/O

| Cycles | Mnemonic | 16-13 | 12 | 11-9 | 8-6 | 5-3 | 2-1 | Function |
|:------:|:---------|:----:|:--:|:----:|:---:|:---:|:---:|:---------|
| 11 | SWI | 0110 | | R_ext | MSB,R_ext Dest. | R_ext | COUNT | Serial Data In |
| 4  | PWD | 0111 | 1 | DESTINATION (8) | | | SOURCE | Parallel Transfer |
| 11 | SWO | 0111 | 0 | R_ext | SOURCE | R_ext | COUNT | Serial Data Out |

### 9-group: Arithmetic and Logic (primary = 1001)

| Cycles | Mnemonic | 16-13 | 12 | 11 | 10 | 9-6 | 5 | 4-1 | Function |
|:------:|:---------|:-----:|:--:|:--:|:--:|:---:|:--:|:---:|:---------|
| 6 | ADD | 1001 | 0 | 0 | 0 | R_B | 0 | R_A | Add |
| 6 | LXR | 1001 | 0 | 0 | 0 | R_B | 1 | R_A | Exclusive OR |
| 6 | AND | 1001 | 0 | 0 | 1 | R_B | 0 | R_A | And |
| 6 | LOR | 1001 | 0 | 0 | 1 | R_B | 1 | R_A | Inclusive OR |
| 6 | SUB | 1001 | 0 | 1 | 0 | R_B | 0 | R_A | Subtract |

Sub-op selector for the arith/logic group is the 4-tuple
**(bit 12, bit 11, bit 10, bit 5)**:

| 12 | 11 | 10 | 5 | Mnemonic |
|:--:|:--:|:--:|:-:|:---------|
| 0 | 0 | 0 | 0 | ADD |
| 0 | 0 | 0 | 1 | LXR |
| 0 | 0 | 1 | 0 | AND |
| 0 | 0 | 1 | 1 | LOR |
| 0 | 1 | 0 | 0 | SUB |

### 9-group: Skip on Line Count

| Cycles | Mnemonic | 16-13 | 12 | 11 | 10-1 | Function |
|:------:|:---------|:-----:|:--:|:--:|:----:|:---------|
| 3 / 6 | SLC | 1001 | 1 | 0 | VALUE (10 bits) | Skip if Line Count ≠ value |

### 9-group: Skip class (primary = 1001, bits 12-11 = 1,1)

Operand is a 7-bit address into the 128 memory locations
(top 128 = special registers).

| Cycles | Mnemonic | 12 | 11 | 10 | 9 | 8 | Function |
|:------:|:---------|:--:|:--:|:--:|:-:|:-:|:---------|
| 3 | SKP | 1 | 1 | 0 | 0 | 0 | Skip On Positive |
| 4 | ISP | 1 | 1 | 0 | 1 | 1 | Increment & Skip on Pos. |
| 4 | DSP | 1 | 1 | 0 | 0 | 1 | Decrement & Skip on Pos. |
| 3 | SKZ | 1 | 1 | 1 | 0 | 0 | Skip On Zero |
| 4 | ISZ | 1 | 1 | 1 | 0 | 1 | Increment & Skip on Zero |
| 4 | DSZ | 1 | 1 | 1 | 0 | 1 | Decrement & Skip on Zero |
| 3 | SKO | 1 | 1 | 1 | 1 | 0 | Skip On Overflow |
| 3 | SKC | 1 | 1 | 0 | 1 | 0 | Skip On Carry |

(ISZ and DSZ appear identical in Wooddell's table — likely an
additional bit elsewhere disambiguates them. **Empirical flight
hex of ISZ shows bit 9 = 1, not 0** — see `feasibility-vs-flight.md`.)

### 9-group: Shifts (primary = 1001, bits 12-11 = 0,1)

| Cycles | Mnemonic | 12 | 11 | 10 | 9-6 | 5 | 4-1 | Function |
|:------:|:---------|:--:|:--:|:--:|:---:|:-:|:---:|:---------|
| 6 | SRS | 0 | 1 | 0 | COUNT | 1 | R_A | Short Right Shift |
| 6 | SLS | 0 | 1 | 1 | COUNT | 0 | R_A | Short Left Shift |
| 6 | SRR | 0 | 1 | 1 | COUNT | 1 | R_A | Short Right Rotate |

### Long shifts (primary = 1110, 1101, 1100)

| Cycles | Mnemonic | 16-13 | 12-9 | 8-6 | 5 | 4-1 | Function |
|:------:|:---------|:-----:|:----:|:---:|:-:|:---:|:---------|
| 6 | ARS | 1110 | COUNT | R_A | Cnt.LSB | R_A | Short Arith Right Shift |
| 9 | LRS | 1110 | COUNT | R_B | Cnt.LSB | R_A | Long Arith Right Shift |
| 9 | LLS | 1101 | COUNT | R_B | Cnt.LSB | R_A | Long Left Shift |
| 9 | LRR | 1100 | COUNT | R_B | Cnt.LSB | R_A | Long Right Rotate |

### Index modification and conditional jump

| Cycles | Mnemonic | 16-13 | 12 | 11-5 | 4-1 | Function |
|:------:|:---------|:-----:|:--:|:----:|:---:|:---------|
| 4 | MCX | 1011 | 0 | VALUE | INDEX | Cond. Modify Index |
| 3 | SKE | 1011 | 1 | VALUE | INDEX | Skip if Equal |

### Discrete output

| Cycles | Mnemonic | 16-13 | 12-9 | 8-5 | 4-1 | Function |
|:------:|:---------|:-----:|:----:|:---:|:---:|:---------|
| 3 | OUT | 1111 | BLOCK | 5 LINES | INDEX | Discrete Output |

### Auto-indexed memory access

| Cycles | Mnemonic | 16-13 | 12 | 11-5 | 4-1 | Function |
|:------:|:---------|:-----:|:--:|:----:|:---:|:---------|
| 6 | AML | 1010 | 0 | SOURCE | MP | Auto Index Memory Load |
| 6 | AMR | 1010 | 1 | DEST. | MP | Auto Index Memory Read |

## Total instruction count

By this table:

- 7 in the simple-operand group (JMP, SRB, EXC, WAT, ABS, MLD, MRD)
- 3 I/O (SWI, PWD, SWO)
- 5 arith/logic (ADD, LXR, AND, LOR, SUB)
- 1 line-count skip (SLC)
- 8 skip-class (SKP, ISP, DSP, SKZ, ISZ, DSZ, SKO, SKC)
- 7 shifts (SRS, SLS, SRR, ARS, LRS, LLS, LRR)
- 4 misc (MCX, SKE, OUT, plus AML/AMR which is technically 2)

Total: ~36 distinct mnemonics, matching Pietrobon's count.

## Sub-op selector summary for primary 1001 (the 9-group)

The "sub-op" within the 9-group depends on which class:

- **bits 12-10 = 0,0,0**: ADD/LXR (bit 5 selects)
- **bits 12-10 = 0,0,1**: AND/LOR (bit 5 selects)
- **bits 12-10 = 0,1,0**: SUB
- **bits 12-10 = 0,1,0** with bit 11 different: shifts (SRS/SLS/SRR)
- **bits 12-10 = 1,0,0**: SLC
- **bits 12-10 = 1,1,0**: SKP/ISP/DSP family (bits 9, 8 select)
- **bits 12-10 = 1,1,1**: SKZ/ISZ/DSZ/SKO/SKC family (bits 9, 8 select)

For our Forth assembler, this means each 9-group mnemonic corresponds
to a specific bit pattern in bits 11-8 (0-indexed) when the operand
is appropriately formatted. See `fds-asm.fs` for the empirical
encoders that produce the right hex bytes against our flight test
vectors.
