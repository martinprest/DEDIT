
                    Psion 2 Organiser Emulator (Beta 5)
                    ===================================

Last Updated: 20th September 1998
Author:       Paul Robson (autismuk@aol.com)
Homepage:     http://users.aol.com/psionorg/index.htm

This emulator is freeware.

The default has now been changed to run the LA ROM Image. If you were
using the CM ROM image, change Psion.Ini, both the ROM Name and the
RAM Size. Because the Memory is now (usually) saved, to perform a cold
start you have to delete the RAM image specified in PSION.INI

Runs under Win95 & DOS but not Windows NT. Support for Datapacks
(albeit quite limited) is now implemented.

The auto-power-off is always disabled. Battery consumption on the emulator
is pretty minimal. It does need to be "turned off" for the RAM to be saved.

Keys - Full screen Organiser display
====

F1        is On/Clr
F2        is Mode
F10       is Enter the Debugger
Backspace is Del
Enter     is Exe

Use OFF (O) to exit full scrren mode and save a snapshot of Organiser main 
memory to be restored at next run.

The other keys are the obvious ones (A-Z Space and Enter)

Other "numeric" chars such as $," etc. by shifting the correct alpha key
as if on organiser, having a real organiser or picture helps here.

Keys 0-9 and . are also 'translated' into their appropriate key sequences
(e.g. 0 'presses' Shift+Y). This does not work for CALC because the
nice Psion presses the Shift keys for you.

The plus key on the numeric keypad will toggle speed between fixed (default)
and as fast as possible. Speed can be illusory because the SLP instruction is
executed in most wait loops, and goes to the next event almost instantaneously.
On my Cyrix P120 it is about 4x the speed of my Psion POS350.

In real speed mode it isn't exact, largely because the LCD Controller does
not have the delays it has in practice. Timing via the interrupts will be
near-as-dammit accurate.

Debugger Commands
-----------------
F1      Set Break
F2      Resynchronise System
F3      Run to LCD Write
F5      Run normally (with the big screen)
F7      Step through
F8      Single Step
F9      Run to Break
HOME    Back to PC
0-9A-F  Change Program Address
+Ctrl   Change Data Address
ESC     Exit emulator (does *NOT* save RAM)

Note - can use TRAP $00 - to stop code and find its location, but emulator
hangs at trap, so can't return to Org main menu and use OFF to save snapshot!
only exit is with ESC

If using Dosbox - CTRL-F10 to release mouse

In the Data area, 100-1BF is the HD44780 Controller RAM.

Datapack Usage
==============

The emulator supports standard OPK format files. These are (simply) the
characters 'O' 'P' and 'K', followed by the byte zero, followed by the
overall size of the file in high-low order, followed by the image of
the pack. This can be constructed either by the 'BLDPACK' program
in the Development kit, or by the MAKEPACK program provided.

Datapacks are controlled via the DRVx and PAKx variables in PSION.INI
these state for each slot (x = B C or D) the type of Datapack connected
and the filename of the Datapack image.

There are two (currently) types of DataPack

        0       Nothing plugged in (default)
        1       A linear EPROM pack
		
MAKEOPL.BAT
-----------
Usage: MAKEOPL <oplfile>  -Note <oplfile> is without .opl extension
Creates pack definition file (PACK.BLD) as below, runs MAKEPACK, runs Org2 emulator
very useful when editing opl file and for quick loading to the emulator

MAKEPACK.EXE
------------
The makepack program expects a text file containing the contents of the
file as follows (everything in [] is explanatory) :-

can have any name, but PACK.BLD is used in example file.

MYPACK  8                       [The output pack is MYPACK.OPK, size 8k]
PROG1   OB3                     [Add Prog1.Ob3 to the pack]
PROG2   OPL                     [Add Prog2.Opl to the pack]
PROG3   TXT                     [Add Prog3.Txt to the pack]

Put these lines in a file (say MYPACK.DEF) and type MAKEPACK MYPACK.DEF ;
the program will construct MYPACK.OPK as defined.

Unlike the BLDPACK program the MAKEPACK program will convert source OPL
files and include them in a datapack. At present it does not allow
ODB files to be included in a datapack.

The datapack handlers only support 8k or 16k EPROM packs, linearly
addressed, write only at present. Any pack image which is not write
protected will be internally flagged write protected. This is because
I haven't written the 'writing' code yet.

==============================================================================
Beta 1 (12 September 1998)

First release of something that works.

==============================================================================
Beta 2 (13 September 1998)

1) In CALC the replacement numbers are confusing cos' its in shift lock mode.
2) Full stop didn't work. (fixed)
3) Accelerate didn't work. (fixed)
4) Right Shift didn't work (fixed)
5) Added Paging support for up to 128k RAM and 128k ROM (max known is 96k
   RAM and 64k ROM ?)
6) Reduced display size by 1/3 as I thought the main display too big.
7) Frame added, size increased slightly to accommodate 20 character display
8) Added RAM/ROM Paging Code, but 64k ROMs and 64/96k RAM can't be used yet.
9) Added Psion.Ini configuration file.

==============================================================================
Beta 3 (13 September 1998)

1) RAM Saved on switch-off, and reloaded on switch-on.
2) Emulated the ACOUT bit (bit 1, port 5), needed for 1)
3) 8th line of UDGs was always blank, now can be anything.

==============================================================================
Beta 4 (13 September 1998)

1) Changed 100-3FF hardware decoding for additional h/w stuff
2) Added ROM Paging support
3) Added support for 20 x 4 LCD Display (LZ Series)
4) Added RAM Paging support

==============================================================================
Beta 5 (20 September 1998)

1) Built with v0.95 Beta 5 of WDOSX, (now runs under NT Win Networks)
2) Writing to Port $15 doesn't cause a debugger trap. This was causing
   password setting on the LZ series to halt due to an ROR $15 instruction.
3) Added very basic support for Datapacks (read only,linear)
4) Added MAKEPACK.EXE to the distribution.

==============================================================================

Thanks to :-

Jaap Scherphuis         for my POS350 and lotsa assistance, also the Site
Dave Woolnough          for the Org2 site at http://www.org2.com
Zac Schroff             who wrote the LCD Document
Sam                     whose idea it was in the first place.
Michael Tippich         for WDOSX - my favourite DOS Extender.
