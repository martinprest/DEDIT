# DEDIT
Diary tool for Psion Organiser II XP

 
DEDIT for Psion Organiser II XP (or CM) - A Diary editing tool
--------------------------------------------------------------
Martin Prest, Dec-2024

Introduction
------------
An editor for diary entries, which lists them in ascending chronological order.
Provides functions such as: Edit, Delete and Find, plus Anniv for copying anniversaries to the following year.
Compatible with the main Organiser DIARY and Jaap's year 2000 fix.
DEDIT can be used as an alternative to the main DIARY, although not all functions are reproduced (Tidy from DIARY is quite useful).
As a precaution, use the Save function of the main DIARY before installing DEDIT, and to periodically backup your diary!

Files
-----

DEDIT:--------main program <br/>
DYsize:-------OS call of diary allocator cell size <br/>
DYnrecs:-----gets total number of records (similar to DYsize, but uses PEEKB instead of OS call) <br/>
DYread$:(dp%)--get diary record at dp%, end of diary returns null string (dp% is diary pointer, relative to diary start address) <br/>
DYdel:(dp%,sz%)			delete a diary record at dp%, length sz% <br/>
DYadd:(dp%,rec$)		add a diary record, rec$ at dp% <br/>
DYiadd:(rec2$)			info add - add in correct place depending on info <br/>
DYinfo$:(rec$)			get info bytes from diary record <br/> 
DYedrec$:(rec$,pt$)		edit rec$ for new or replacement record, with prompt pt$ <br/>
DOW:(d%,m%,y%)			get day of week as no.0-6, with Sunday as 0 <br/>
MOD:(n%,d%)				get modulus (remainder) of division n%/d% <br/>
KBflsh:					flush keyboard buffer using os call (used to solve key bounce on my XP) <br/>
POKCNV%:(P%,A$)			Jaap's machine code builder, pokes hex A$ to address starting at P% <br/>
TIMEINST:				Jaap's Y2K fix, uses POKCNV% to install terminate and stay resident (TSR) machine code <br/>

Total size of OB3 object files: about 7.8 KB

Install
-------
FOR XP or CM only - The LZ Diary works differently

Modification of the program may result in a system crash, and although I've been testing it, use with caution!

Copy all files to a datapak or to A: using a CommsLink
To use with Jaap's year 2000 bug fix, TIMEINST is included. From the Organiser main menu go to PROG then RUN 
and enter TIMEINST in A:, B: or C: as appropriate to install the Y2K fix.
(TIMEINST uses POKCNV% to convert and poke the machine code)
(to remove get TSRREMV from Jaap's website - y2000fix.zip, or reset the Psion)
Set time using TIME on the Organiser main menu.
Use MODE on the main menu to install DEDIT there, or use PROG, RUN, and enter DEDIT

Using DEDIT
-----------
DEDIT works a bit like the LIST option in the XP (or CM) DIARY, except that it allows to edit diary entries.
Diary entries are stored in a memory allocator cell in a similar format to database records in the A:MAIN file, 
but different enough that OS calls are required to add or delete them.
The OS calls used are: <br/>
al$grow - grows an allocator cell (from a given point) <br/>
al$shrink - shrinks an allocator cell (from a given point) <br/>
ut$cpyb - copies memory contents from one place to another (used to read/write diary entries to/from a string variable) <br/>

Diary entries are referred to as records in this document.
On launch, the first record is dispalyed, which will be the oldest entry in the diary. <br/>
Records are displayed in the following format: <br/>
ddDD-MM-YY HH:MM, where: dd=day, DD=date, MM=Month, YY=year, HH=hour, MM=minute <br/>
record text is displayed on the line below and will scroll if longer than 16 characters. Max length for diary entries is 64 chars. <br/>
Find mode is indicated by a capital F between the date and time on the top line of the display. <br/>

Keys
----
EXE moves to the next record, or during Find it will find the next match. <br/>
UP & DOWN arrows move to first or last record, respectively. <br/>

During EDIT of date & time, arrow keys move left & right and up & down to increase or decrease numbers on date & time, 
or numbers can be entered on the keypad.
There's some basic error checking of date & time, invalid date or time generates a "Bad Date or Time" error until date & time are ok.

During EDIT of record text (uses OPL command EDIT): UP goes to start, DOWN goes to end, On/Clr clears the text.

To choose an editing function, press MODE to get the options in a menu, or just press the 1st letter of the option (without the menu): <br/>
r*/*,NEW,COPY,ANNIV,DEL,EDIT,FIND,MODE,BACK,ZAP,QUIT

The 1st menu item is a record indicator showing the current record number and the total number of records, e.g.:
r2/7 would be record 2 of a total of 7 records. Selecting this item will show 
further info: the total size of the diary in bytes and the total no. of diary records.

NEW		creates a new record with current date and time, then allows to edit it<br/>
COPY	copy record, if new record has same date&time as old, confirm replace with Y/N<br/>
ANNIV	as copy, but with year+1<br/>
DEL		delete current record, confirms with Y/N before deleting record<br/>
EDIT	edit current record (as COPY but old record deleted)<br/>
FIND	search for text in records, like FIND from the Organiser top menu, a second selection turns find mode off<br/>
MODE	changes record displayed after edit: either new edited record (new rec) or record prior to edit (old rec)<br/>
BACK	move back to previous record<br/>
ZAP		delete whole diary, all records! Use with caution! Asks to confirm Y/N, twice<br/>
QUIT	exit DEDIT<br/>

note1:	moving back works by scanning forwards from the start through the whole diary to the previous record, this is incase record size has changed due to edit<br/>
note2:	mode set to old record also scans the diary, so can be slow if the diary has a lot of records<br/>

The diary is held in what is called an "allocator cell" which is a reserved area of RAM.
There are a number of allocator cells and they can each be made bigger or smaller.
The diary is the 3rd cell in the list, 1st is drivers such as COMMS and 2nd is a list of top menu entries.

Diary record structure
----------------------

system variable:
dirycell at memory location: $2004/05 is a pointer to the memory location of the Diary allocator cell. 

Diary entries are listed sequentially in ascending date&time order.
Each diary entry consists of 7 information bytes and the entry text:

LYMDHMA Length,Year,Month,Day,Hour,Minute,Alarm<br/>
0123456

0: Length byte (0-64) Text length of the diary entry. The maximum length is 64 characters.<br/>
1: Year (0-99)<br/>
2: Month (0-11)<br/>
3: Day (0-30)<br/>
4: Hour (0-23)<br/>
5: Minutes (0-59)<br/>
6: Alarm, 0=no alarm, else the number of minutes before the entry +1 when the alarm will sound.<br/>
This is followed by the text of the diary entry, its length given by byte 0 above.<br/>

The last diary entry has only an initial zero byte to indicate the end of the diary.
