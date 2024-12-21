
DEDIT for Psion Organiser II XP (or CM) - A Diary editing tool
--------------------------------------------------------------
Martin Prest, Dec-2024

Introduction
------------
A diary browser and editor, which lists entries in ascending chronological order.
Provides functions such as: Edit, Delete and Find, plus Anniv for copying anniversaries to the following year.
Compatible with the main Organiser DIARY and Jaap's year 2000 fix.
DEDIT can be used as an alternative to the main DIARY, although not all functions are reproduced.
As a precaution, use the SAVE function of the main DIARY before installing DEDIT, and to periodically backup your diary!

About DEDIT
-----------
DEDIT works a bit like the LIST option in the XP (or CM) DIARY, except that it allows to edit diary entries.
Diary entries are referred to as records for DEDIT.
Diary records are stored in a memory allocator cell in a similar format to database records in the A:MAIN database
but different enough that OS calls are required to add or delete them.
The OS calls used are: 
al$grow - grows an allocator cell (from a given point)
al$shrink - shrinks an allocator cell (from a given point)
ut$cpyb - copies memory contents from one place to another (used to read/write diary entries to/from a string variable)

Files
-----
DEDIT:					main program
files.trn 				list of files to translate from opl to object ob3 files using opltran or atran
dedit.bld 				same list of files, but as ob3 to make a pack using bldpack or makepack
DEDIT_beta.opk   		pack image with all the files
DEDIT_beta.sna   		XP 32K snap file for Jaap's Jape emulator for those who want to try it without having to run TIMEINST first
DYsize:					OS call of diary allocator cell size
DYnrecs:				gets total number of records (similar to DYsize, but uses PEEKB instead of OS call)
DYread$:(dp%)			get diary record at dp%, end of diary returns null string (dp% is diary pointer, relative to diary start address)
DYdel:(dp%,sz%)			delete a diary record at dp%, length sz%
DYadd:(dp%,rec$)		add a diary record, rec$ at dp%
DYiadd:(rec2$)			info add - add in correct place depending on info
DYinfo$:(rec$)			get info bytes from diary record
DYedrec$:(rec$,pt$)		edit rec$ for new or replacement record, with prompt pt$
DOW:(d%,m%,y%)			get day of week as no.0-6, with Sunday as 0
MOD:(n%,d%)				get modulus (remainder) of division n%/d% 
KBflsh:					flush keyboard buffer using os call (used to solve key bounce on my XP)
POKCNV%:(P%,A$)			Jaap's machine code builder, pokes hex A$ to address starting at P%
TIMEINST:				Jaap's Y2K fix, uses POKCNV% to install terminate and stay resident (TSR) machine code

Total size of OB3 object files: about 7.8 KB

Install
-------
FOR XP or CM only - The LZ Diary works differently

Modification of the program may result in a system crash, and although I've been testing it, use with caution!

Copy all files to a datapak or to A: using a CommsLink
translate OPL on the organiser, or translate before transfer using OPLTRAN or ATRAN with the files.trn list
Or build a pack using BLDPACK and the DEDIT.BLD file and make a pack using orglink or similar.
To use with Jaap's year 2000 bug fix, TIMEINST is included. From the Organiser main menu go to PROG then RUN 
and enter TIMEINST in A:, B: or C: as appropriate to install the Y2K fix.
(TIMEINST uses POKCNV% to convert and poke the machine code)
(to remove get TSRREMV from Jaap's website - y2000fix.zip, or reset the Psion)
Set time using TIME on the Organiser main menu.
Use MODE on the main menu to install DEDIT there, or use PROG, RUN, and enter DEDIT


Using DEDIT
-----------
On launch, the record browser displays the first record, which will be the oldest entry in the diary.
Records are displayed in the following format:
ddDD-MM-YY HH:MM, where: dd=day, DD=date, MM=Month, YY=year, HH=hour, MM=minute
Record text is displayed on the line below and will scroll if longer than 16 characters. 
Max length for diary entries is 64 chars. If record has an alarm set, it is inducated as (A:xx) where x is the time in minutes for the alarm prior to the record time.
(uses OPL view) so left and right arrows stop or continue scrolling, as appropriate.
FIND mode is indicated by a capital F between the date and time on the top line of the display. FIND mode ends when the end of the diary is reached.
When an editing mode is selected (NEW, EDIT, COPY or ANNIV), the text prompt (2nd line) indicates the mode (ANNIV is displayed as ANN to save space on the screen).

Keys
----
EXE moves to the next record, or during FIND it will find the next match.
UP & DOWN arrows move to first or last record, respectively.

During edit of date & time, arrow keys move left & right and up & down increase or decrease numbers on date & time, 
or numbers can be entered on the keypad.
Invalid date or time generates a "Bad Date or Time" error until date & time are corrected.
EXE confirms the date & time, then record text can be edited.
On/Clr during date and time edit cancels the edit and returns to the record browser.

During EDIT of record text (uses OPL command EDIT): UP goes to start, DOWN goes to end, On/Clr clears the text. 
On/Clr a second time returns the edit cursor to the time & date.
EXE enters the text, then the alarm time can be edited (no. of minutes before the event time, max 59)

To choose an editing function, press MODE to get the options in a menu, or just press the 1st letter of the option (without the menu):
r*/*,NEW,COPY,ANNIV,DEL,EDIT,FIND,MODE,BACK,ZAP,QUIT
On/Clr can also be used to QUIT and return the the Organiser main menu.
The DEL key selects the delete function.

The 1st menu item is a record indicator showing the current record number and the total number of records, e.g.:
r2/7 would be record 2 of a total of 7 records. Selecting this item will show 
further info: the total size of the diary in bytes and the total no. of diary records.

NEW		creates a new record with current date and time, then allows to edit it
COPY	copy record, if new record has same date&time as old, confirm replace with Y/N
ANNIV	as copy, but with year+1
DEL		delete current record, confirm with Y/N
EDIT	edit current record (as COPY but old record deleted)
FIND	search for text in records, like FIND from the Organiser top menu, a second selection turns find mode off, recalls find text from previous search
MODE	changes record displayed after edit: either new edited record (new rec) or record prior to edit (old rec)
BACK	move back to previous record
ZAP		delete whole diary, all records! Use with caution! Asks to confirm Y/N, twice
QUIT	exit DEDIT

note1:	BACK operates by returning to the 1st record, then scanning forwards to the previous record; this method is simple and reliable but slow.
note2:	MODE set to "old rec" (see above) also scans the diary, so can be slow if the diary has a lot of records.

The diary is held in what is called an "allocator cell" which is a reserved area of RAM.
There are a number of allocator cells and they can each be made bigger or smaller.
The diary is the 3rd cell in the list, 1st is drivers such as COMMS and 2nd is a list of top menu entries.

Diary record structure
----------------------

system variable:
dirycell at memory location: $2004/05 is a pointer to the memory location of the Diary allocator cell. 

Diary entries are listed sequentially in ascending date&time order.
Each diary entry consists of 7 information bytes and the entry text:

LYMDHMA Length,Year,Month,Day,Hour,Minute,Alarm
0123456

0: Length byte (0-64) Text length of the diary entry. The maximum length is 64 characters.
1: Year (0-99)
2: Month (0-11)
3: Day (0-30)
4: Hour (0-23)
5: Minutes (0-59)
6: Alarm, 0=no alarm, else the number of minutes before the entry +1 when the alarm will sound.
This is followed by the text of the diary entry, its length given by byte 0 above.

The last diary entry has only an initial zero byte to indicate the end of the diary.
