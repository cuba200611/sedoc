Apple II Monitor Symbol Table
=============================

Source code for the monitor (modified and ported to a different
assembler) is available in [Jeff Tranter's 6502 repo][a2monsource].

[a2monsource]: https://github.com/jefftranter/6502/blob/master/asm/Apple%5D%5BMonitor/appleiimonitor.s

Autostart ROM symbol table reference. Copied from
[pp.174-176][a2ref-174] of the _Apple II Reference Manual_.
Many errors due to OCR and manual copy/reformat from PDF.

    0000  LOCO
    0001  LOC1

    0020  WNDLFT
    0021  WNDWDTH
    0022  WNDTOP
    0023  WNDBTM
    0024  CH
    0025  CV
    0026  GBASL
    0027  GBASH
    0028  BASL
    0029  BASH
    002A  BAS2L
    002B  BAS2H
    002C  H2
    002C  LMNEM
    002D  RMNEM
    002D  V2
    002E  CHKSUM
    002E  FORMAT
    002E  MASK
    002F  LASTIN
    002F  LENGTH
    002F  SIGN
    0030  COLOR
    0031  MODE
    0032  INVFLG
    0033  PROMPT
    0034  YSAV
    0035  YSAV1
    0036  CSWL
    0037  CSWH
    0038  KSWL
    0039  KSWH
    003A  PCL
    003B  PCH
    003C  AIL
    003D  A1H
    003E  A2L
    003F  A2H
    0040  A3L
    0041  A3H
    0042  A4L
    0043  A4H
    0044  A5L
    0045  A5H
    0045  ACC
    0046  XREG
    0047  YREG
    0048  STATUS
    0049  SPNT
    004E  RNDL
    004F  RNDH

    0095  PICK
    0200  IN

    03F0  BRKV
    03F2  SOFTEV
    03F4  PWREDUP
    03F5  AMPERV
    03F8  USRADR
    03FB  NMI
    03FE  IRQLOC

    0400  LINE1
    07F8  MSLOT

    C000  IOADR
    C000  KBD
    C010  KBDSTRB
    C020  TAPEOUT
    C030  SPKR
    C050  TXTCLR
    C051  TXTSET
    C052  MIXCLR
    C053  MIXSET
    C054  LOWSCR
    C055  HISCR
    C056  LORES
    C057  HIRES
    C058  SETANO
    C059  CLRANO
    C05A  SETAN1
    C05B  CLRAN1
    C05C  SETAN2
    C05D  CLRAN2
    C05E  SETAN3
    C05F  CLRAN3
    C060  TAPEIN
    C064  PADDLO
    C070  PTRIG
    CFFF  CLRROM

    E000  BASIC
    E003  BASIC2

    F800  PLOT
    F80C  RTMASK
    F80E  PLOT1
    F819  HLINE
    F81C  HLINE1
    F826  VLINEZ
    F828  VLINE
    F831  RTS1
    F832  CLRSCR
    F836  CLRTOP
    F83B  CLRSC2
    F856  GBCALC
    F871  SCRN
    F87F  RTMSKZ
    F88C  INSDS2
    F89B  IEVEN
    F8A5  ERR
    F8A9  GETFMT
    F8C2  MNNDX2
    F8C9  MNNDX3
    F8D4  PRNTCP
    F8DB  PRNTBL
    F8F5  NXTCOL
    F8F9  PRMN2
    F910  PRADR1
    F914  PRADR2
    F926  PRADR3
    F92A  PRADR4
    F930  PRADR5
    F938  RELADR
    F940  PRNTYX
    F941  PRNTAX
    F944  PRNTX
    F94A  PRDL2
    F94B  PRBLNK
    F94C  PRBL3
    F953  PCADJ
    F954  PCADJ2
    F956  PCADJ3
    F95C  PCADJ4
    F961  RTS2
    F962  FMT1
    F9A6  FMT2
    F9B4  CHAR1
    F9BA  CHAR 2
    F9C0  MNEML
    FA00  MNEMR
    FA40  IRQ
    FA4C  BREAK
    FA59  OLDBRK
    FA62  RESET
    FA6F  INI TAN
    FA81  NEWMON
    FA9B  FIXSEV
    FAA3  NOFIX
    FAA6  PWRUP
    FAA9  SETPG3
    FAAB  SETPLP
    FABA  SLOOP
    FAC7  NXTBYT
    FAD7  REGDSP
    FADA  RGDSP1
    FAE4  RDSP1
    FAFD  PWRCON
    FB05  DISKID
    FB09  TITLE
    FB11  XLTBL
    FB19  RTBL
    FB1E  PREAD
    FB25  PREAD2
    FB2E  RTS2D
    FB2F  INIT
    FB39  SETTXT
    FB40  SETGR
    FB47  GBA5CALC
    FB4B  SETWND
    FB5B  TABV
    FB60  APPLE 1
    FB64  SETCOL
    FB65  STITLE
    FB6F  SETPWRC
    FB78  VIDWAIT
    FB79  SCRN2
    FB88  KBDWAIT
    FB94  NOWAIT
    FB97  ESCOLD
    FB9B  ESCNOW
    FBA5  ESCNEW
    FBBE  MNNDX1
    FBC1  BASCALC
    FBD0  BASCLC2
    FBD0  INSTDSP
    FBD9  BELLI
    FBE4  BELL2
    FBEF  RTS2B
    FBF0  STORADV
    FBF4  ADVANCE
    FBFC  RTS3
    FBFD  VIDOUT
    FBS2  INSDS1
    FC10  BS
    FC1A  UP
    FC22  VTAB
    FC24  VTABZ
    FC2B  RTS4
    FC2C  ESC1
    FC42  CLREOP
    FC46  CLE0P1
    FC58  HOME
    FC62  CR
    FC66  LF
    FC70  SCROLL
    FC76  SCRL1
    FC95  SCRL3
    FC9C  CLREOL
    FC9E  CLEOLZ
    FCA1  CLE0L2
    FCA8  WAIT
    FCA9  WAIT2
    FCAA  WAIT3
    FCB4  NXTA4
    FCBA  NXTA1
    FCBC  SCRL2
    FCC8  RTS4B
    FCC9  HEADR
    FCD6  WRBIT
    FCDB  ZERDLY
    FCE2  ONEDLY
    FCE5  WRTAPE
    FCEC  RDBYTE
    FCEE  RDBYT2
    FCFA  RD2BIT
    FCFD  RDBIT
    FD0C  RDKEY
    FD1B  KEYIN
    FD21  KEYIN2
    FD2F  ESC
    FD35  RDCHAR
    FD3D  NOTCR
    FD5F  N0TCR1
    FD62  CANCEL
    FD67  GETLNZ
    FD6A  GETLN
    FD71  BCKSPC
    FD75  NXTCHAR
    FD7E  CAPTST
    FD84  ADDINP
    FD92  PRA1
    FD96  PRYX2
    FDA3  XAMB
    FDAD  MODBCHK
    FDB3  XAM
    FDB6  DATA0U7
    FDBE  CROUT
    FDC5  RTS4C
    FDC6  XAMPM
    FDD1  ADD
    FDDA  PRBVTE
    FDE3  PRHEX
    FDE5  PRHEX2
    FDED  COUT
    FDF1  C0UT1
    FDF6  COUTZ
    FE01  BL1
    FE04  BLANK
    FE0B  STOR
    FE17  RTS5
    FE18  SETMODE
    FE1D  SETMDZ
    FE20  LT
    FE22  LT2
    FE2C  MOVE
    FE36  VFY
    FE58  VFYOK
    FE5E  LIST
    FE63  LIST2
    FE75  A1PC
    FE78  A1PCLP
    FE7F  A1PCPTS
    FE80  SETINV
    FE84  SETNORM
    FE89  SETKBD
    FE8B  INPORT
    FE93  SETVID
    FE95  OUTPORT
    FE97  OUTPRT
    FE9B  IOPRT
    FEA7  IOPRT1
    FEA9  I0PRT2
    FEB0  XBASIC
    FEB3  BASCONT
    FEB6  SETIFLG
    FEBD  INPRT
    FEBF  REGZ
    FEC2  TRACE
    FEC4  STEPZ
    FECA  USR
    FECD  WRITE
    FED4  WR1
    FED6  GO
    FEED  WRBYTE
    FEEF  WRBYT2
    FEF6  CRMON
    FEFD  READ
    FF0A  RD2
    FF16  RD3
    FF2D  PRERR
    FF3A  BELL
    FF3F  RESTORE
    FF44  RESTR1
    FF4A  SAVE
    FF4C  SAV1
    FF59  OLDRST
    FF65  MON
    FF69  MONZ
    FF73  NXTITM
    FF7A  CHRSRCH
    FF90  NXT3IT
    FF98  NXTBAS
    FFA2  NXTBS2
    FFA7  GETNUM
    FFAD  NXTCHR
    FFBA  DIG
    FFBE  TOSUB
    FFC7  ZMODE
    FFCC  CHRTBL
    FFE3  SUBTBL
    FS3C  CLRSC3
