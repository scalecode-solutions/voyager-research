# FDS Instruction Set Reference

Compiled from Steven Pietrobon's NASASpaceflight Forum post
[#2640632](https://forum.nasaspaceflight.com/index.php?topic=9476.msg2640632#msg2640632)
(12 November 2024), derived from Jack Wooddell's *Design of a CMOS
Processor for Use in the Flight Data Subsystem of a Deep Space Probe*
(1974, undated original, dated by Wooddell from memory; obtained from
the James E. Tomayko Collection of NASA Documents at Wichita State
University Special Collections, Box 37 FF 22).

Cross-referenced with the FDS architecture memo MJS:2.64A
(Wooddell, 7 Oct 1974) — `MS 87-08 B37 FF42` in the same collection.

## Architecture summary

- **Word size:** 16 bits
- **Clock:** 806.4 kHz; memory access at 403.2 kHz (2.48 µs/cycle)
- **Memory:** 8K × 16-bit, organised as two 4K banks
- **Address:** 12-bit base, 13th bit set independently for code and
  data via OUT instructions (banking)
- **Hardware registers:** A, B, C (3, internal to CPU)
- **Special registers:** 128, located at addresses 0x0F80–0x0FFF
  (top 128 words of lower bank), broken down as:
  - 16 general-purpose registers
  - 31 memory pointers
  - 7 index registers
  - 74 counters
- **Interrupt:** single 2.5 ms timer; forces PC to 0x0000

## Instructions

| Mnemonic | Cycles | Function | Confirmed encoding |
|----------|--------|----------|--------------------|
| **I/O** | | | |
| OUT | 3 | Initialise output pulse | `Fxxx` |
| PWD | 4 | Parallel word transfer | `7xxx` |
| SWO | 11 | Serial word out | |
| SWI | 11 | Serial word in | |
| **Memory reference** | | | |
| AMR | 6 | Auto-indexed memory read | |
| AML | 6 | Auto-indexed memory load | |
| MRD | 4 | Direct memory read | `5xxx` |
| MLD | 4 | Direct memory load | |
| SRB | 3 | Store return address (B → memory) | `1xxx` |
| **Branching** | | | |
| JMP | 2 | Jump (saves PC+1 to B, then jumps) | `0xxx` |
| JMS | 2 | Jump and save (alias of JMP) | `0xxx` |
| SLC | 3/6 | Skip if line counter ≠ value | |
| SKP | 3 | Skip on positive | |
| ISP | 4 | Increment and skip on positive | |
| DSP | 4 | Decrement and skip on positive | |
| SKZ | 3 | Skip on zero | |
| ISZ | 4 | Increment and skip on zero | |
| DSZ | 4 | Decrement and skip on zero | |
| SKO | 3 | Skip on overflow | |
| SKE | 3 | Skip if index equals value | |
| EXC | 2 | Execute | |
| SKC | 3 | Skip on carry | |
| **Shifting** | | | |
| LRS | 9 | Long right shift (arithmetic) | |
| LLS | 9 | Long left shift | |
| LRR | 9 | Long right rotate | |
| SRS | 6 | Short right shift | |
| SLS | 6 | Short left shift | |
| SRR | 6 | Short right rotate | |
| ARS | 6 | Short right shift (arithmetic) | |
| **Arithmetic / logic** | | | |
| ADD | 6 | Add | |
| SUB | 6 | Subtract | |
| AND | 6 | Logical AND | |
| LOR | 6 | Logical OR | |
| LXR | 6 | Logical XOR | |
| **Constants** | | | |
| ABS | 3 | Load value (absolute data entry from program) | `8xxx` |
| **Timing** | | | |
| WAT | 2–4097 | Wait (pause) | |
| **Index modification** | | | |
| MCX | 4 | Conditionally modify index (set if zero) | |

## Subroutine convention

The FDS has no call stack and no return-from-subroutine instruction.
A subroutine call works as follows:

1. Caller executes `JMS subname` (opcode 0).
   This saves PC+1 (the 12-bit return address) into hardware register B
   and jumps to `subname`.
2. The subroutine's first instruction is typically `SRB local_var`
   (opcode 1), which writes the contents of B into a memory word.
   Because B is 12 bits and the word is 16, the upper 4 bits are zero
   — making the saved word *itself* a `JMP <return_addr>` instruction.
3. To return, the subroutine `JMP`s to the location where it stored
   the return address. Executing the stored word as an instruction
   jumps back to the caller.

This is genuine self-modifying code by design. There is no other way
to return from a subroutine on this CPU.

## Memory bank switching

The 13th address bit for instruction fetch and the 13th address bit
for data access are independent and are toggled by `OUT` instructions:

| Encoding | Mnemonic | Effect |
|----------|----------|--------|
| `F087` | `OUT 7,SETJU` | Set instruction-fetch bit to 1 (upper 4K) |
| `F08F` | `OUT 7,SETJD` | Set instruction-fetch bit to 0 (lower 4K) |
| `F097` | `OUT 7,SETAU` | Set data-access bit to 1 (upper 4K) |
| `F09F` | `OUT 7,SETAD` | Set data-access bit to 0 (lower 4K) |

The instruction-fetch bit doesn't take effect until the next `JMP`
(per Pietrobon, similar to PDP-8 behaviour) — preventing wild
execution at the same offset in the wrong bank.

## Open questions

These are explicitly noted by Pietrobon and remain to be derived from
test data:

- **ABS** — paper says "stores the given 12-bit number" but doesn't
  say where. Probably one of the 16 general registers in memory.
- **AMR / AML** — feasibility design didn't include these; final
  design did. Probably auto-increment a memory pointer after access.
- **EXC** — purpose unclear; classified as a branch instruction in
  the paper.
- **Sub-opcode bits within `8xxx` and `9xxx` groups** — these primary
  opcodes hold multiple mnemonics (ABS/ADD/AND/SKP/ISZ/...). The
  bits that distinguish them are not yet derived.

## References

- Pietrobon, S. NSF post #2640632, 12 Nov 2024.
  [link](https://forum.nasaspaceflight.com/index.php?topic=9476.msg2640632#msg2640632)
- Wooddell, J. *Design of a CMOS Processor for Use in the Flight Data
  Subsystem of a Deep Space Probe.* Undated (~1974).
  Wichita State, Tomayko Collection, Box 37 FF 22.
- Wooddell, J. *MJS FDS Processor Architecture and Instruction Set.*
  JPL Interoffice Memo MJS:2.64A, 7 Oct 1974.
  Wichita State, Tomayko Collection, MS 87-08 B37 FF42.
- Tomayko, J. E. *Computers in Spaceflight: The NASA Experience.*
  NASA SP-4317, 1988. Chapter 6, Section 2 (Voyager).
