# SC_MP-INS8060-NIBL-FloatingPoint-TinyBASIC-Interpreter

## SC/MP (pronunciation 'skæmp') – one of the first microprocessors on the market

### NIBLFP - TinyBasic with floating point extension

### About the INS8060 or SC/MP Processor [1]
National Semiconductor introduced the ISP-8A/500D microprocessor chip (also known as SC/MP) In April 1976 and it was intended for small and cheap industrial controllers. The acronym SC/MP stands for **S**mall **C**ost-effective **M**icro **P**rocessor. The development at that time required few external components. Although not a powerful microprocessor but at the time it was cheap and simple to use. The SC/MP had an instruction set of only 46 basic instructions and was rather easy to program at binary level. This chip was developed in P-MOS technology and needed a positive power supply of 5 Volts and a negative of 7 Volts.

The SC/MP has a rather simple architecture and can address a total of 65536 bytes memory. Because only 12 of the required 16 address lines are connected to pins the addressing space is 4KiB but the high four bits of the address were multiplexed on the data bus during the NADS signal. So the 64KiB memory is divided in 16 pages of 4096 bytes each. Only a limited number of memory reference instructions can cross a page boundary.

Later National Semiconductor introduced the SC/MP-II (with official names being first ISP-8A/600 and, later, INS8060) that was made in N-MOS technology and only needed a single 5 Volt power supply. Three of the signals on the SC/MP-II were logically inverted (i.e. BREQ became NBREQ) so the SC/MP-II is not 100% pin-compatible with the first SC/MP chip. 

In 1977 Elektor Magazine devoted a few issues to the SC/MP and the Elektor design came in two versions, a simple basic one and a version with a hexadecimal keyboard, 7-segment displays and a monitor program in ROM, cassette interface and the option to connect a visual display unit (the Elekterminal).

Beside the Elektor design [2] there was a very similar system from Firma Homecomputer Vertriebs GmbH at Düsseldorf/Germany (not existent anymore) discussed extensively in a book [3], further a one board computer implementation called the MK14, developed by Science of Cambridge Ltd [4]. These are still sold on eBay sometimes. For use inside the NDR small computer Michael Haardt developed a CPU board for the SC/MP [5]. Additionally there are also great emulators in software and hardware (e.g. with PIC microcontroller).

### SC/MP articles in Wikipedia
<https://de.wikipedia.org/wiki/National_Semiconductor_SC/MP></br>
<https://en.wikipedia.org/wiki/National_Semiconductor_SC/MP>

### Tiny BASIC NIBL

NIBL is an abbreviation for **N**ational **I**ndustrial **B**asic **L**anguage.

The following three NIBL variants are reassembled using the macro assembler from Alfred Arnold [6]. The generated binary files are identical to the original ones.

#### NIBL
The first NIBL was published in Dr. Dobb's Journal [7]. An excerpt is found in the belonging subdirectory.

#### NIBLE
NIBLE is practically identical to NIBL except that the program runs in page 1 of the SC/MP memory space. Due to this several things had to be changed. I adapted the source for the mentioned macro assembler.

#### NIBLFP
This program was written by me in the years 1976 - 1986 as floating point extension for the above mentioned NIBL and NIBLE, it covers the address range from hex D000 to FFFF. The resurrected version starte with a program entry point at hex DFC1. Due to existence of the mentioned macro assembler the code was rewritten, so that the program can start now at hex D000, but beside that, was intended from historical reasons to be left largely unchanged.

The floating-point routines were originally an adapted 1-to-1 translation from source code for the 6502, released in 1976 [8] and written by Roy Rankin and Steve Wozniak. I leave some information extracted from the original article in the belonging subdirectory, especially an assembled listing and the binary code.

On his website [9] Ronald Dekker preserved my NIBLFP over the time, his listings, however, are for the 7.8 version. Kindly he translated my original German description into English. The version published in this repository is 7.9 and is my latest attempt in SC/MP programming dated from the eighties of the last century. The instruction set is found in the belonging subdirectory. 

#### NIBLFP_2024
Here is presented a completely rewritten, modernized and commented version. The code is published forty years too late but in the hope that it may be helfül in any way in the future. The presence of a powerful macroassembler opened up completely new possibilities for efficient programming. The publication will take place at this point until the intended location in waltje's repository (see acknowledgments) is ready.

### MONITOR
This program originates from around 1982 and was written for the Elektor-System, it resides on page C and is started at address $C000. More details are found in the associated directory.

### EMULATOR
The emulator in Python is based on a program written in C++ around 2000 by Sipke de Wal and handed down by Henry Mason on [10]. The ingenious idea of using two illegal SC/MP opcodes as signals for output and input, respectively, allows the emulator to be used in a normal console window. Adapted versions for MONITOR and NIBLFP are provided in the related directory.</br>
To start an emulator session with the provided files, use either
```
$ ./scmp_curses.py MONITOR C000
$ ./scmp_curses.py NIBL 0000
$ ./scmp_curses.py NIBLFP D000 '["E4C0","E42F"]'
$ ./scmp_curses.py NIBLFP D000
```
Values in brackets are optional breakpoints as hexadecimal numbers.</br>
To load an existing BASIC program from ancient times open the file in an editor, choose 'Edit' 'Select All' and 'Copy' to get the whole program into the clipboard. Start the desired BASIC and paste the content of the clipboard with Ctrl-Shift-V (console under Linux). The code will be accepted.</br>
Since the emulator can generate snapshot files (s option), read that into page 1 (r option). At start NIBLFP will detect that there is still a program from earlier sessions. This will be a method to exchange BASIC programs from one computer to another.</br>
For NIBL respective NIBLE a calendar program in BASIC is included. NIBLFP enthusiasts use KALENDA.BAS (german) or CALENDA.BAS (english) for the same purpose.

### Tools
The hardware built-in a 19" rack was lost but the program survived several moves as a hexdump listing in paper form. So own tools had to be written for the resurrection of the original, they are gathered in this folder.

Particularly worth mentioning here is the rewritten disassembler `2_disass4scmp.py` with a syntax that takes some getting used to. There are two logical variables within the code that control the behavior of the program:</br>
  `tabbed`: set to False [default: True] to get a normal listing with addresses and opcodes</br>
  `xppc3_call`: set to False [default: True] to get normal behavior of XPPC P3</br>
With the default preferences the program generates source code that can be put in directly into the mentioned macro assembler.</br>
The program `15_dcm_6502.py` converts a floating point decimal number into a 4 byte floating point representation to be used in NIBLFP. The encoding is totally equivalent to the method used by Apple for the 6502 (see above).

### Acknowledgements
Special thanks go as usual to the people in the developer community at StackOverflow. Without their help and answered questions at <https://stackoverflow.com/> and affiliate sites this work would not be possible.

A very special thank you also goes to Fred N. van Kempen <https://github.com/waltje>, who expertly analyzed the NIBLFP assembly and gave crucial advice to reduce inconsistencies in the disassembled code. A completely rewritten, modernized and commented version will be available soon at his github repository for virtual archaeological computing <https://github.com/VARCem/VARCem8>.

### Literature

[1] Hein Pragt <https://www.heinpragt.com/english/software_development/ins8060_or_scmp_processor.html></br>
[2] Elektor Magazin 5(1979), p. 50 "SC/MP-Mikrocomputer mit BASIC-Interpreter" in German</br>
[3] C. Lorenz, SC/MP Microcomputer Handbuch, W. Hofacker Verlag, 1980 ISBN-13:978-3921682425</br>
<https://oldcomputers.dyndns.org/public/pub/manuals/sc-mp_microcomputer-handbuch_(ger_gray).pdf></br>
[4] <https://en.wikipedia.org/wiki/MK14></br>
[5] Michael Haardt, SC/MP: CPU card for the NDR Klein computer</br>
<http://www.moria.de/tech/scmp/cpuscmp/></br>
[6] Alfred Arnold, Macroassembler AS</br>
<http://john.ccac.rwth-aachen.de:8000/as/></br>
[7] Mark Alexander, Dr. Dobb's Journal of Calisthenics & Orthodontia, vol. 1(1976), p. 331-347</br>
[8] Roy Rankin, Steve Wozniak, Dr. Dobb's Journal of Calisthenics & Orthodontia, vol. 1(1976), p. 207-209</br>
download Dr. Dobb's at <http://archive.6502.org/publications/dr_dobbs_journal/dr_dobbs_journal_vol_01.pdf></br>
[9] Ronald Dekker <https://www.dos4ever.com/SCMP/SCMP.html#fpoint></br>
[10] <http://xgistor-echo.scorchingbay.nz/>, download site <http://xgistor-echo.scorchingbay.nz/scmp.htm>

### Disclaimer:
This software was created for educational and demonstration purposes and is not intended for production use. The author is therefore not liable for damage arising from the use of the software and does not guarantee its completeness, freedom from errors and suitability for a specific purpose.

