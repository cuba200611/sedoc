Video on 8-bit Systems
======================

For connector information, see [conn/video](conn/video.md).

Video output is generally vertical sync for ~50-60 Hz frames or fields (two
interlaced fields per frame), horizontal synchronization signals for 260 or
more lines per frame/field, and image information encoded as a single (Y)
or three (RGB) luminance signals and, for non-RGB color systems,
additional encoded color information as either one or more separate signals
(chroma for S-video; Cb and Cr for component video) or combined with the
luma signal (CVBS/composite color video).

Separate sync signals are typically TTL; RGB signals may or may not be TTL
as well in digital RGB systems (8 colors, or 16 with an additional
intensity signal). Standard luminance signals are generally about a volt
into 75Ω; sync signals usually are reduced to similar levels when combined
with a luminance. CVBS standard levels (from [[wp cvbslev]]):

<img src='img/video-cvbs-levels.png'>

Systems for color or monochrome video with various degrees of multiplexing
(not specifying any particular sync rates or interlace) are, roughly:

- RGBHV: Three luminance signals and separate H and V sync signals
- RGBS: Composite (H+V) sync signal
- RGB: Sync on green (or sync unspecified)
- DRGB: 8-color digital RGB, usually TTL, and usually separate H and V sync.
- DRGBI: 16-color digital RGB with fourth "intensity" value.
- Component video: Y+sync Pr Pb (Y Cr Cb is the digital format)
- Y/C: Y+sync, chroma. Also "S-video," which has multiple meanings.
- CVBS (color video baseband signal): Y+sync+color, using one of several
  broadast systems (NTSC, PAL, etc.) to encode the color information.
- MVBS (monochrome video baseband signal): Y+sync

#### Vertical Sync

When identifying a video format, both [ITU BT.601] and [SMPTE 259M] append
the field rate directly to the format or the frame rate with a slash,
making "480i60" and "480i/30" the same thing.

Standard vertical field rates are almost invariably around 50-60 Hz, with a
frame rate half that because the output is interlaced. The SD vsync signal
goes low (-300 mV from black level) with the line 4 hsync pulse and returns
to high (black level) with the line 7 hsync pulse (falling edge). [[hdr
csync1]]

Odd-line (1,3,…) fields are indicated by starting vsync with hsync;
even-line (2,4,…) fields by starting and ending vsync half-way between two
hsync pulses (after hsync 266 and hsync 269 of the frame), thus giving
262.5 lines per field in NTSC video. See the [LMH1981] datasheet and [[hdr
csync1]] for sample waveforms.

The standard sytems also actually originate a composite sync that uses
equalization pulses around and during vsync; see "Composite Sync" below for
more information on this.

Many systems do a (non-standard as far as TV goes) progressive 262 or 263
line 60 FPS display (240p) by always starting vsync on a line boundary.
(This may leave "blank" scan lines between the rendered scan lines on the
monitor.) This also affects csync; see below. It seems that most CRT
monitors/TVs handle this, but many non-CRT monitors and digital capture
devices may not. Additionally, some devices may handle it correctly through
composite but not component inputs, and vice versa. [[hdr 240]] The [240p
test suite][240p ts] may be useful when analyzing 240p output and how
upscalers deal with it.

#### Horizontal Sync

Horizontal sync rates start around 15.7 kHz for ~200 displayed lines (see
progressive note below), and are increased when displaying more lines in
progressive signals. Common horizontal sync rates (kHz) are:

     480i   15.750    NTSC B/W: 262.5 lines × 2 × 60 Hz fields
     480i   15.734    NTSC color: as B/W at 59.940 Hz
     576i   15.625    PAL: 312.5 lines × 2 × 50 Hz fields
            21.8      EGA (350 line)
           ~24        400-line modes
            31.469    VGA

SD uses bi-level sync, with the leading edge of the horizontal sync pulse
(-300 mV from black level) indicating the start of the line. The pulse is
typically around 4.7 μs, but the trailing edge should be ignored. HD uses
tri-level sync going to -300 mV then +300 mV before returning to black
level; the sync reference point is the zero-crossing between the two. Some
cheaper HD systems will use the rising edge of SD sync too, causing
problems. Others always use the first falling edge. [[hdr jit]]

An individual line consists of _front porch_ before the sync, sync, _back
porch_ (which is used to set 300 mV black level and may include color
burst), and displayed data. See [Structure of a video signal][wp hline].
The displayed data may include a border before and after reading from a
frame buffer.

#### Composite Sync (csync)

Combining the sync signals produces composite sync (csync). Suppressing the
hsync during the vsync pulse would cause loss of horizontal sync during the
vertical sync interval; the solution is to return to black level for a
short period _before_ the falling edge of the hsync signal so that a
falling edge is still present at the right point, called _serrated pulses_
within the _broad pulses_ of the vsync. This is _not_ the same as doing an
XOR of the signals; that would delay the falling edge of hsync by the hsync
signal width; properly constructed csync retains exactly the hsync's
falling edges. [[hdr csync1]]

But because the signals are interlaced, it's even trickier because it
introduces a falling edge half-way between the two horizontal sync signals.
This is fixed in the NTSC standards by introducing double-rate and narrower
"equalization pulses" for the three lines before, three lines during and
three lines after the vsync interval; effectively it looks like the hsync
runs at double rate with a narrower pulse during this time. This is
typically decoded with a one-shot (monostable multivibrator) to recover the
hsync and an integrator (low-pass filter) to recover the vsync. [[hdr
csync1]].

Nonstandard progressive SD video (240p) often drops the equalization pulses
and may also use standard-width hsync pulses during vsync. Being
non-standard, there are many variations of what exactly goes on here. [[hdr
csync2]] describes various methods of combining sync and what kind of
variations they produce:
- AND gate: suppresses hsync during vsync. CRTs usually recover sync within
  the first few lines (often the non-displayed lines). More modern systems
  using a digital PLL may lose the signal completely unless it has good
  "coast" functionality.
- XNOR: delays hsync per first paragraph above. Digital PLLs may or may not
  have a range wide enough to capture the shifted falling edges. XNOR may
  generate glitches when both inputs change at the same time (sometimes
  fixed with an RC filter).

[[TB476]] discusss how to try to recover from poor csync implementations.

#### Input Conditioning

The [LMH1981] datasheet discusses input AC coupling considerations for
video (p. 14) and suggests 1 μF for DC-coupled inputs and 0.01 μF for
AC-coupled inputs.

The [LM1881] datasheet notes that for a 75 Ω input, 620 Ω in series with
the source and 510 pF to ground will form a lowpass with a corner of 500
kHz, easily passing the sync but reducing subcarrier by almost 18 dB,
though delaying the output of the LM1881 by 40-200 ns.

For converting TTL sync to video-level, RetroRGB suggests using a 470R (or
sometimes 330R) in series, creating a voltage divider of 0.13 (0.18) with
the 75Ω input impedence.


Common Resolutions and Timings
------------------------------

Individual timings are given as _i/t_, where _i_ is the number of dots or
lines and _t_ is the (approximate) time. Line or frame time sets are given
as groups of individual timings: total, front porch, sync, back porch and
optionally displayed data, which may include the border or be broken down
into _sb+dd+eb_ for start border, (user) display data and end border.

    dotclk MHz  Resolution  Name [source]
    hfreq  kHz      dots/μs      front       sync      back
    vfreq   Hz      lines/ms     front       sync      back
    ───────────────────────────────────────────────────────────────────
    14.31818    640×200     PC98 "15 kHz" [tim pc98]
    15.98       896/62.58    64/4.47    64/4.47  128/8.94
    61.23       261/16.33    15/0.94     8/0.50   38/2.38

    21.0526     640×400     PC98 "24 kHz" [tim pc98]
    24.83       848/40.28    64/3.04    64/3.04   80/3.8
    56.42       440/17.72     7/0.28     8/0.32   25/1.01

    25.175      640×400     PC98 "31 kHz" [tim pc98]
    31.47       800/31.78    14/0.58    64/2.54  82/3.24
    70.05       449/14.28    13/0.41     2/0.064 35/1.09

Fujitsu FM-7:
- Hsync: 4 μs wide (no change around vsync).
- Vsync: 2.4 μs before hsync, 506 μs wide.  
  Covers 8 hsync pulses, including the one that starts just after vsync.
- CVBS: XOR sync. No equalization pulses.  
  No color burst. Colors produce eight equally-spaced luminance steps.
- RGB: Vpp = 6.8 V into MΩ, 2.1 V into 75Ω (one chan. only).  
  Series resistor into PVM (Ω=V) and comparsion of white with composite:
  - 820=0.46    noticably dimmer, gray not white
  - 680=0.46    as 820 (???)
  - 470=0.56    a bit dimmer, near monochrome gray 1 below white
  - 330=0.66    slightly brighter
  - 75=0.90     much brighter
- Measurement notes:
  - Vpp above is actually Vampl measurement from the scope, due to heavy
    ringing on the RGB lines.
  - There were a lot of consistency problems with the series resistor
    tests. Possibly the voltage changes when not using the same resistor on
    all three outputs.

National/Panasonic JR-200:
- Horizontal (in CVBS): 63.6 μs; ?/5.5 ?/4.5 ?/9 ?/44.7.
  No change around vsync.
- Vertical (in CVBS): 264?/16.32 5/318 3/191, 24/1526, 24+192+16/?
  - Vsync leading edge is synchronous with hsync.
  - Covers 3 hsync pulses, counting the end rise as hsync too.
- CVBS: XOR sync. No equalization pulses. No color burst during vsync.
- RGB: Vpp = 4 V into MΩ, .65 V into 75Ω (one chan. only).  
  Series resistor into PVM (Ω=V) and comparsion with composite output:
  - 470=0.46    brighter, but composite white was gray, and yellow was orange.
  - 680=0.46    same as 470!?
  - 820=0.40    R,G,B dimmer; white still brighter (not gray)
  - 330=0.68    W,G brighter; red similar, blue dimmer

Apple IIc:
- Horizontal (in CVBS): 63.6 μs: `_/7.5 _/3.9 _/13.2 _/39.0`
- Vertical (in CVBS): 262 lines; `32/_ 4/_ 34/_ 192/_`
  - hsync on falling edge during vblank; not XOR
- CVBS:
  - Color burst during vsync

PET (built-in monitor; luma switches on/off around 2.5 V):
- Explanation, schematics and some timings from [Andre][pet andre].
- [PET CRTC] spreadsheet.
- Further info and mods in [this thread][pet f6 623]. Includes support for
  double horizontal scan rate (40 kHz instead of 20 kHz lines @60 Hz) for
  higher resolution.

#### References

- [[tim opt]] Classic Console Upscaler Wiki, ["Optimal timings"][tim opt].
  Timings for many consoles and a few computers from NES through Wii.
- [[tim NES]] Nes Dev Wiki, ["NTSC Video"][tim nes]. Detailed Nintendo
  Entertainment System/Famicom timings and C++ code for emulation.
- [Technical details of Display out [PC98]][tim pc98]. Waveforms and timing
  details for 15/24/31 kHz modes.


References
----------

Datasheets and Application Notes:
- Maxim APP 734 [Video Basics][man734]. Fundementals of analog video.
  Good glossary but little detail.
- TI [LM1881] Video Sync Separator. Classic chip to extract csync, vsync
  odd/even and burst/back porch from a composite video signal. Includes
  info on filtering color information from composite video.
- TI [LMH1981] Multi-Format Video Sync Separator. Includes waveforms for
  various SD and HD video and sync signals.
- Analog Devices [ADV7170] Digital PAL/NTSC Video Encoder. Provides lots of
  insight into how to generate video signals.
- Renesas, Technical Brief \[TB476] [Regenerating HSYNC from Corrupted SOG
  or CSYNC during VSYNC][TB476]. (SOG = Sync On Green.) Includes schematic
  and board layout for sync regeneration device that "cleans up" the sync
  for cheap digital monitors.
- Renesas [ISL4089] DC-Restored Video Amplifier. Used in [[TB476]] to
  remove sync from green signal.

HD Retrovision articles and posts:
- \[hdr csync1] [Engineering CSYNC - Part 1: Setting the Stage][hdr csync1]
- \[hdr csync2] [Engineering CSYNC - Part 2: “Falling” Short][hdr csync2]
- \[hdr jit] [Sync Jitter][hdr jit]
- \[hdr 240] [More information than you need about “240p” video][hdr 240].

Other sources
- \[scanlines] [Scanlines Demystified / Special Downscaling Edition
  Q&A][scanlines]. Extensive 240p discussion and pics; equipment
  information.
- \[240p ts] Classic Console Upsaker wiki, [240p test suite][240p ts].



<!-------------------------------------------------------------------->

<!-- Common resolutions and timings -->
[tim NES]: http://wiki.nesdev.com/w/index.php/NTSC_video
[tim opt]: http://junkerhq.net/xrgb/index.php?title=Optimal_timings
[tim pc98]: https://radioc.web.fc2.com/column/pc98bas/pc98dispout2_en.htm

<!-- References -->
[240p ts]: http://junkerhq.net/xrgb/index.php?title=240p_test_suite
[ISL4089]: https://www.renesas.com/jp/ja/www/doc/datasheet/isl4089.pdf
[ITU BT.601]: https://en.wikipedia.org/wiki/Rec._601
[LM1881]: https://www.ti.com/lit/ds/symlink/lm1881.pdf
[LMH1981]: https://www.ti.com/lit/ds/symlink/lmh1981.pdf
[SMPTE 259M]: https://en.wikipedia.org/wiki/SMPTE_259M
[TB476]: https://www.renesas.com/us/en/www/doc/tech-brief/tb476.pdf
[adv7170]: https://www.analog.com/media/en/technical-documentation/data-sheets/ADV7170_7171.pdf
[hdr 240]: https://www.hdretrovision.com/240p
[hdr csync1]: https://www.hdretrovision.com/blog/2018/10/22/engineering-csync-part-1-setting-the-stage
[hdr csync2]: https://www.hdretrovision.com/blog/2019/10/10/engineering-csync-part-2-falling-short
[hdr jit]: https://www.hdretrovision.com/jitter
[man734]: https://pdfserv.maximintegrated.com/en/an/AN734.pdf
[pet andre]: http://6502.org/users/andre/petindex/crtc.html
[pet f6 623]: http://forum.6502.org/viewtopic.php?f=1&t=6231
[pet_crtc]: http://inchocks.co.uk/commodore/PET/PET_CRTC.xls
[scanlines]: http://scanlines.hazard-city.de/
[wp cvbslev]: https://en.wikipedia.org/wiki/Composite_video#Signal_components
[wp hline]: https://en.wikipedia.org/wiki/Analog_television#Structure_of_a_video_signal
