Microprocessor/Microcontroller/Electronics Development Tools
============================================================


Schematics/EDA/CAD
------------------

- [KiCad]: EDA for schematics and PCBs.
- [CircuitLab.com][clab]: In-browser editing/simulation. Also
  publishes a free electronics textbook.


Cross-assemblers
----------------

All of these can be built for Linux unless otherwise mentioned; most
have Windows/DOS/etc. as well.

#### Multi-platform

"+" appended to a CPU name indicates support for additional in that
family, e.g., "6800+" adds some of 6801/6805/68HC11, 6809+ adds 6309.
"Z80gb" is the Sharp LR35902 in the Game Boy.

- [The Amsterdam Compiler Kit][tack]
  - [Features]: linker/relocation; no macros?
  - [CPUs][tack-cpus]: 6500 6800 6805 6809 8080/5 Z80 68000+ PDP11 16-bitters
  - MCUs: 6805
  - Missing: 1802 65816
  - Note: Also C/Pascal/Modula-2/Occam compilers for some CPUs

- [AS]  v1.42bld148 2019-07-20
  - [Features][as-doc]: macros, good local syms (2.9), structs/unions
    symbol export, in-file code switch.
  - [CPUs][as-cpus]: 1802 6502 65816 6800+ 68000+ 6809+ 8008 8085 8086
    TI Z80 Z180+ many-many-more
  - MCUs: 8051+ AVR PIC many-many-more
  - No official source repo; try [KubaO's repo][Kuba0/asl].

- [ASxxxx][] ([Downloads][asx-dl])  v5.30 update 1 2019-03-10
  - [Features][asx-doc]: linker/relocation, macros
  - [CPUs][asx-cpus]: 1802 6502+ 6800+ 6809 8085 Z280 Z80 Z80gb AVR PIC ...
  - MCUs: AVR PIC ST8
  - Missing: 6309 65816

- [naken_asm][] ([GitHub][naken-gh]): November 2, 2019
  - Features: very simple; has simple disassembler and simulator for some archs
  - CPUs: 1802 6502+ 65816 6800+ 68000 6809 Z80 AVR8 PIC Propeller ...

- [SB-Assembler][sbasm] ([GitHub][sbasm-gh]): V3.03.02 (2019-05-05)
  - Features: Python; macros.
  - [CPUs][sbasm-cpus]:
    1802, 6502, 6800, 6804, 6805, 6809, 68HC11, 8008, 8080/5, Z80+
  - MCUs: 8048, 8051, AVR, PIC, ST6, ST7, ACE1101, ACE1202, SC/MP

- [WLA DX][] ([GitHub][wla-gh]) v9.9 2019-08-15
  - Families: 6502 6800 6801 6809 Z80 GB-Z80
  - Missing: 1802

#### 6502 Families

- [ACME Cross assembler][acme] ([GitHub copy][acme-gh])
  - CPUs: 6502 6510, 65c02, 65816

- ca65 from the [cc65][] ([GitHub][cc65-gh]) C compiler suite.


Disassemblers
-------------

- Jeff Tranter's Python disassemblers ([GitHub][jt-gh])
  - Families 6502, 65816, 6800, 6811
  - No custom label support


Other Software
--------------

The [SRecord] suite (Debian `srecord` package) includes `srec_cat`,
`srec_cmp` and `srec_info` programs for manipulating, comparing and
describing various EPROM load file formats, including Motorola
S-Record, Intel hex, "ASCII hex" and many other formats. Manipulation
includes filters to generate CRCs, modify addresses and so on.


Assembler Pseudo-op Notes
-------------------------

WDC _65816 Programming Manual_ §5 p.66 "The Assembler Used in This Book":

    ORG addr
    GEQU        global equate
    DS n        define storage; _n_ bytes
    DC arg      define constant, arg starts with:
                    A=addr, I1=1-byte int, H=hex bytes, C=char string

Leventhal's _6502 Assembly Language Subroutines_ p.viii:

    *=          set location counter
    =, .EQU
    .BLOCK n    reserve _n_ bytes of storage
    .BYTE n     form byte-length data; n=byte value
    .DBYTE n    form word MSB first
    .WORD n     form word in machine order
    .TEXT


<!-------------------------------------------------------------------->
[clab]: https://www.circuitlab.com/
[kicad]: kicad.md

[Kuba0/asl]: https://github.com/KubaO/asl.git
[as-cpus]: http://john.ccac.rwth-aachen.de:8000/as/cpulist.html
[as-doc]: http://john.ccac.rwth-aachen.de:8000/as/as_EN.html
[as]: http://john.ccac.rwth-aachen.de:8000/as/
[asx-cpus]: http://shop-pdp.net/ashtml/asxdoc.htm
[asx-dl]: http://shop-pdp.net/ashtml/asxget.php
[asx-doc]: http://shop-pdp.net/ashtml/asmlnk.htm
[asxxxx]: http://shop-pdp.net/ashtml/asxxxx.htm
[naken-gh]: https://github.com/mikeakohn/naken_asm
[naken_asm]: http://www.mikekohn.net/micro/naken_asm.php
[sbasm-cpus]: https://www.sbprojects.net/sbasm/crosses.php
[sbasm-gh]: https://github.com/sbprojects/sbasm3
[sbasm]: https://www.sbprojects.net/sbasm/
[tack-cpus]: http://tack.sourceforge.net/about.html
[tack]: http://tack.sourceforge.net/
[wla dx]: http://www.villehelin.com/wla.html
[wla-gh]: https://github.com/vhelin/wla-dx

[acme-gh]: https://github.com/meonwax/acme
[acme]: https://sourceforge.net/projects/acme-crossass/
[cc65-gh]: https://github.com/cc65/cc65
[cc65]: https://cc65.github.io/

[jt-gh]: https://github.com/jefftranter/6502/tree/master/disasm

[SRecord]: http://srecord.sourceforge.net/
