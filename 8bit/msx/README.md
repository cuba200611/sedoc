MSX
===

References:
- [Wikipedia (en)][wpen]
- [Wikipedia (ja)][wpja]
- [msx.org wiki][mowiki] (MSX Resource Center)


Usage Notes
-----------

### Startup

- Use STOP to pause/restart output, Ctrl-STOP to break.
- Holding down `Shift` during the post-reset initialization sequence will
  avoid loading DOS, generally leaving with you with about 5384 more bytes
  of memory available to BASIC (around 28K instead of 23K).
- Holding down `Ctrl` will enable only one drive and leave you with about
  1558 bytes of free memory available to BASIC (around 25K instead of 23K).

### BASIC

Handy screen-editor keys:
- `^B`/`^F`: cursor back/forward one word
- `^N` move to EOL
- `^E`: erase to EOL
- `^U` erase current line
- `^R` enter/exit insert mode (cursor movement also exits)

See msx.org wiki [MSX Characters and Control Codes][codes] for a full list
of the control codes, which seem to work the same on input as output where
that makes sense.


Models
------

Memory figures are RAM/VRAM. ≥64K VRAM indicates MSX2 or MSX2+. "2C" indicates
two cartridge slots; "1C+E" one cartridge slot plus an expansion bus connector
(usu. for a unit with more slots); "+D" indicates a 3.5" floppy drive..

- __Sony HB-F1XD__:         64k/128k 2C+D RGB. Ext. PSU.
- __Panasonic FS-A1__:      64k/128k 2C RGB. Ext. PSU.
- __Toshiba HX-22__:        64k/16k 2C RGB (RP13A). RS-232. "Pasopia IQ"
- __Sanyo MPC-2 (Wavy2)__:  64k/16k 2C.
- __National CF-3000__:     64k/16k 2C. Box+keyboard. JP21 RGB.
                            Separate superimpose unit. No space for floppy drv.
- __Nationanl CF-2700__:    32k/16k 2C.
- __National CF-2000__:     16k/16k 2C.
- __Sony HB-55__:           16k/16k 1C+E. Cheap keyboard.
- __Sony HB-55P__:          16k/16k 2C. European version.
- __Casio PV-16__:          16k/16k 1C+E. Ext. PSU 10 V 800 mA.
                            Small chiclet keyboard.
- __Casio MX-10/MX-101__:   16k/16k. PSU 7 V 800 mA.
- __Casio PV-7__:            8k/16k 1C+E. No CMT. PSU; 10 V 800 mA center-neg.
                            Joypad keys plus 2nd set of cursor keys.

The __Casio PV-7__ and __Casio MX-10/MX-101__ have no CMT interface; they
need the FA-32 expansion unit (which also adds a 100 V PSU)  to add it.

#### Power Supplies

The Sony [AC-HB3][] (for HB-FX1D) and Panasonic [FS-AA51][] (for for FS-A1,
FS-A1mkII) external PSUs use the same 3-pin "mini-IEC" connector and supply
9 VDC 1.2 A and 18 VAC 170 mA. The boards themselves apparently use only
+5 VDC and ±12 VDC, the latter for the audio amplifier.


Standard Specifications
-----------------------

References:
- \[td1] [_MSX Technical Data Book_][td1], Sony, 1984.
  MSX1 hardwre and software.

MSX systems include [td1 p.8]:
- Z80A-compatible CPU at 3.579545 Mhz (NTSC color subcarrier freq)
- 32K of system ROM with the BIOS and MSX-BASIC
- 16K min. of RAM; grows downward from $FFFF
- VDP (Video Display Processr): TI TMS-9918A compatible
- PSG (Programmable Sound Generator): GI AY-3-8910 compatible
  1.7897725 Mhz (1/2 CPU clock).
- PPI (Programmable Peripheral Interface) Intel i8255 compatible.
- 0 as "on-board" slot; 1 or more cartridge slots
- CMT: 1200 bps 1200/2400 Hz; 2400 bps 2400/4800 Hz.
  0 = 1 cycle low, 1 = 2 cycles high. Standard JP DIN-8. [td1 p.15]

#### Keyboard

[td1 p.13] PPI PB0-PB7 are column inputs X0-X7. PC0-PC3 go to a decoder
that brings to ground one of 10 row outputs Y0-Y10.

#### Joysticks

[td1 p.25] Inputs have pullups to Vcc. AY-3-8910 IOA 0-5 and IOB 0-6 used
for interface.

    1-4 i   fwd/back/left/right
      5     +5 VDC max 50 mA
      6 io  TRG 1: only button on Type A joystick
      7 io  TRG 2: 2nd button Type B joystick
      8  o  output
      9     GND; short inputs to this to assert them

For paddles, postive pulse to pin 8 triggers monostable multivibrators
which should let pins 1-4,6-7 go high for 10-3000 μs before bringing them
low again.

#### Parallel Printer Output (optional)

[td1 p.19] Pins number 1-7 across top then 8-14 across bottom, from right
to left (??? confirm) looking into connector on computer.

        1 out  P̅S̅T̅B̅
      2-9 out  PDB0-PDB7
       10      n/c
       11 in   BUSY
    12-13      n/c
       14      GND


Optional Peripherals
--------------------

#### RS-232

[td1 p.20] i8251 USART, i8253 Programmable Interval Timer @1.8432 Mhz, 4K ROM.

    80 rw   8251 data port
    81 rw   8251 command/status port
    82 r    status sense for CTS, timer/counter 2, RI, CD
    82  w   interrupt mask register
    83      reserved for manufacturer use
    84 rw   8253 counter 0
    85 rw   8253 counter 1
    86 rw   8253 counter 2
    87  w   8253 mode register

Port $82 read. CTS is here because some 8251s have buggy CTS detection.

      7  CTS 0=asserted 1=negated
      6  i8253 Ch. 2
    5-2  reserved
      1  RI 0=asserted 1=negated (optional, must be present if CD present)
      0  CD 0=asserted 1=negated

Port $82 write: interrupt enables. All are 1=mask (initial value) and 0=enable.

    7-4  reserved
      3  i8253 timer 2 (optional)
      2  sync char/break detect (optional)
      1  Tx ready (optional)
      0  Rx ready

The 8253 input clock is 1.8432 Mhz. Timers 0, 1 and 2 are Rx clock, Tx
clock and free for applications, respectively. Divisors (x16) are 6=19200
bps, 12=9600, etc.

#### Floppy Disk

[td1 p.18] Interface has 16K ROM at $4000-$7FFF with MSX-DOS kernel, MSX
DISK BASIC extensions and a "physical disk I/O driver" supplied by the
manufacturer. No particular FDC is required.

The format is MS-DOS compatible. Standard physical media include 8" SD (128
bytes/sec) or DD (1024 bytes/sec) and 5.25"/3.5"/3" DD 512 bytes/sec.


Audio/Music
-----------

These are based around Yamaha FM synthesis chips often known as the OPL
(FM Operator Type-L) series, which started with the YM3526. It offers 9
channels of FM synthesis, or 6 channels of FM plus 5 of percussion. Some
versions of the chip (such as the Y8590) also offered GPIO and keyboard
scanning. [[opl3prog]] provides a brief overview of OPL programming.

#### MSX-AUDIO

[MSX-AUDIO] was available in three implementations. The Panasonic FS-CA1
offered full suport for the standard, the Philips NMS-1205 and Toshiba
HX-MU900 partial support. It uses the OPL1 [Y8590], which includes 4-bit
ADPCM sampling support (sample ram varies from 0-32KB), and has a keyboard
connector. The ROM includes [MSX-AUDIO BASIC] extensions.
[Detection/programming][maud prog]:
- $80-$84 contain signature `AUDIO`.
- Y8950 ports: $C0 reg number (delay 12 cyc), $C1 data (delay 84 cyc).
  $C2 and $C3 if second Y8950 present.
- Page 0: $0000-$2FFFF MBIOS, $3000-$3FFF 4 KB work RAM
- Page 1/2: $4000-$BFFF segment selected by writing $3FFF b1-b0:
  - 0: $4000-$6FFF BASIC extn, $7000-$7FFF RAM mirror
  - 1: $4000-$BFFF custom firmware
  - 2, 3: $4000-$BFFF ADPCM data 1, 2
- Also see [Hardware][maud hw] and [拡張BIOS][maud bios].

#### MSX-MUSIC

[MSX-MUSIC] was a later but inferior system using the cheaper OPLL
[YM2413], which is still partially compatible (only 1 user instrument; no
ADPCM). It was built into most MSX2+ systems. The original cart is the
Panasonic SW-M004 FM-PAC; there were a few others, including stereo
versions. [Detection/programming][mmus prog]
- Scan for internal first, then external:
  - Internal: $4018-$401F signature `APRLOPLL` (also clone carts);
    I/O ports usable; no memory-mapped I/O.
  - External: $401C-$401F signature `OPLL` (FM-PAC);
    use memory mapped I/O or enable ports by setting $7FF6 b1 = 1.
  - Also see [this thread][mmus detect]
  - Some modern MSX-AUDIO ROMs may emulate MSX-MUSIC BIOS, e.g., `AUD1OPLL`,
    `AUD3OPLL`, `AUD4OPLL` (Moonsound); FM-BIOS must be used for these.
  - Writing to $7FF0-$7FFF on Panasonic MSX2+ will disable MSX-MUSIC ROM
- YM2413 ports:
  - $7C (mem $7FF4) register index (delay 12 cyc),
  - $7D (mem $7FF5) data (delay 84 cyc)
- FM-BIOS:
  - Routines: $4110 WRTOPL, $4113 INIOPL, $4116 MSTART, $4119 MSTOP,
    $411C RDDATA, $411F OPLDRV, $4122 TESTBGM
  - Handlers: $5000 statement, $5003 interrupt, $5006 stop bgm,
    $5009 enable and reset OPLL

#### Moonsound

The Moonsound uses an OPL4 [YMF-278B-F] offering 16-bit wavetable and FM
synthesis.

#### Konami SCC

The [Konami SCC][] ([tech info][scc tech])  was a custom sound chip
providing 5 channels of wavetable synthsis. It was included in some Konami
game carts and in a standalone cart for use with disk games. There was also
an improved SCC-1 version.

### General Notes

BiFi's Weblog post [Detection of FM sound chips][bifi] may provide further
useful information on detecting MSX-AUDIO, MSX-MUSIC (both real and emulated
by MSX-AUDIO with recent ROM updates) and MoonSound.



<!-------------------------------------------------------------------->
[mowiki]: https://msx.org/wiki
[td1]: https://archive.org/stream/MSXTechnicalHandbookBySony#page/n5/mode/1up
[wpen]: https://en.wikipedia.org/wiki/MSX
[wpja]: https://ja.wikipedia.org/wiki/Msx

[FS-AA51]: https://www.msx.org/wiki/Panasonic_FS-AA51
[AC-HB3]: https://www.msx.org/wiki/Sony_AC-HB3

<!-- Audio/Music -->
[Konami SCC]: https://www.msx.org/wiki/Konami_SCC
[MSX-AUDIO BASIC]: https://www.msx.org/wiki/MSX-AUDIO_BASIC
[MSX-AUDIO]: https://www.msx.org/wiki/MSX-AUDIO
[MSX-MUSIC]: https://www.msx.org/wiki/MSX-MUSIC
[Y8590]: https://en.wikipedia.org/wiki/Yamaha_Y8950
[YMF-278B-F]: http://www.msxarchive.nl/pub/msx/docs/datasheets/opl4.pdf
[bifi]: http://bifi.msxnet.org/blog/index.php?entry=entry110809-114719
[maud bios]: http://map.grauw.nl/resources/datapack/Vol2-4.1MSX-AUDIOHardware.pdf
[maud hw]: http://map.grauw.nl/resources/datapack/Vol2-4.3MSX-AUDIOExtendedBIOS.pdf
[maud prog]: https://www.msx.org/wiki/MSX-AUDIO_programming
[mmus detect]: https://www.msx.org/forum/msx-talk/development/how-to-detect-sound-chips-without-bios
[mmus prog]: https://www.msx.org/wiki/MSX-MUSIC_programming
[opl3prog]: http://www.fit.vutbr.cz/~arnost/opl/opl3.html
[scc tech]: http://bifi.msxnet.org/msxnet/tech/scc.html
