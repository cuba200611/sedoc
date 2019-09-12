Microprocess/Microcontroller/Electronics Development Tools
==========================================================


Multi-Platform Cross-assemblers
-------------------------------

All of these can be built for Linux unless otherwise mentioned; most
have Windows/DOS/etc. as well.

"+" appended to a CPU name indicates support for additional in that
family, e.g., "6800+" adds some of 6801/6805/68HC11, 6809+ adds 6309.
"Z80gb" is the Sharp LR35902 in the Game Boy.

- [AS]  v1.42bld148 2019-07-20
  - [Features][as-doc]: macros, good local syms (2.9), structs/unions
    symbol export, in-file code switch.
  - [CPUs][as-cpus]:
    1802 6502 65816 6800+ 68000+ 6809+ 8008 8085 8086 TI Z80 Z180+
  - MCUs: 8051+ AVR PIC ...

- [ASxxxx][] ([Downloads][asx-dl])  v5.30 update 1 2019-03-10
  - [Features][asx-doc]: linker/relocation, macros
  - [CPUs][asx-cpus]: 1802 6502+ 6800+ 6809 8085 Z280 Z80 Z80gb AVR PIC ...
  - MCUs: AVR PIC ST8 
  - Missing: 6309

- [WLA DX][] ([GitHub][wla-gh]) v9.9 2019-08-15
  - Families: 6502 6800 6801 6809 Z80 GB-Z80
  - Missing: 1802


Disassemblers
-------------

- Jeff Tranter's Python dissassemblers ([GitHub][jt-gh])
  - Families 6502, 65816, 6800, 6811
  - No custom label support



<!-------------------------------------------------------------------->
[as-cpus]: http://john.ccac.rwth-aachen.de:8000/as/cpulist.html
[as-doc]: http://john.ccac.rwth-aachen.de:8000/as/as_EN.html
[as]: http://john.ccac.rwth-aachen.de:8000/as/
[asx-cpus]: http://shop-pdp.net/ashtml/asxdoc.htm
[asx-dl]: http://shop-pdp.net/ashtml/asxget.php
[asx-doc]: http://shop-pdp.net/ashtml/asmlnk.htm
[asxxxx]: http://shop-pdp.net/ashtml/asxxxx.htm
[wla dx]: http://www.villehelin.com/wla.html
[wla-gh]: https://github.com/vhelin/wla-dx

[jt-gh]: https://github.com/jefftranter/6502/tree/master/disasm
