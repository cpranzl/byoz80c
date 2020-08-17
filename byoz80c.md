# Build Your Own Z80 Computer
## Design Guidelines and Application Notes
## by Steve Ciarcia

## Introduction

Text

## Table Of Content

- [Build Your Own Z80 Computer](#build-your-own-z80-computer)
  - [Design Guidelines and Application Notes](#design-guidelines-and-application-notes)
  - [by Steve Ciarcia](#by-steve-ciarcia)
  - [Introduction](#introduction)
  - [Table Of Content](#table-of-content)
  - [Chapter 1 Power Supply](#chapter-1-power-supply)
  - [Chapter 2 Central Processor Basics](#chapter-2-central-processor-basics)
  - [Chapter 3 The Z80 Microprocessor](#chapter-3-the-z80-microprocessor)
  - [Chapter 4 Build Your Own Computer - Start With the Basics](#chapter-4-build-your-own-computer---start-with-the-basics)
  - [Chapter 5 The Basic Peripherals](#chapter-5-the-basic-peripherals)
  - [Chapter 6 The ZAP Operating System](#chapter-6-the-zap-operating-system)
    - [Chapter 6.1 Operating System Functions](#chapter-61-operating-system-functions)
      - [Chapter 6.1.1 Cold Start Operation](#chapter-611-cold-start-operation)
      - [Chapter 6.1.2 Warm Start Operation](#chapter-612-warm-start-operation)
    - [Chapter 6.2 Operating System Module Description](#chapter-62-operating-system-module-description)
      - [Chapter 6.2.1 Warm Start Module](#chapter-621-warm-start-module)
      - [Chapter 6.2.2 Command Recognition Module](#chapter-622-command-recognition-module)
      - [Chapter 6.2.3 Restart Module](#chapter-623-restart-module)
  - [Chapter 7 Programmin an EPROM](#chapter-7-programmin-an-eprom)
  - [Chapter 8 Connecting ZAP to the Real World](#chapter-8-connecting-zap-to-the-real-world)
  - [Chapter 9 Build a CRT Terminal](#chapter-9-build-a-crt-terminal)
  - [Appendix A Construction Techniques](#appendix-a-construction-techniques)
  - [Appendix B ASCII Codes](#appendix-b-ascii-codes)
  - [Appendix C Manufacturers Specification Sheets](#appendix-c-manufacturers-specification-sheets)
  - [Appendix D ZAP Operating System](#appendix-d-zap-operating-system)
  - [Appendix E Z80 CPU Technical Specifications](#appendix-e-z80-cpu-technical-specifications)
  - [Glossary](#glossary)
  - [Index](#index)

## Chapter 1 Power Supply

It's not enough to build a central processor card with a little input/output (I/O) and menmory, and call it a computer. From the time you walk over to the computer and flip the switch, the system is completely dependent upon the proper operation of its power supply. 

## Chapter 2 Central Processor Basics

Text

## Chapter 3 The Z80 Microprocessor

Text

## Chapter 4 Build Your Own Computer - Start With the Basics

Text

## Chapter 5 The Basic Peripherals

Text

## Chapter 6 The ZAP Operating System

The function of an operating system is to provide the programmer with a set of tools to help him in developing, debugging and executing a program. In general, the operating system assists the programmer by managing the resources of the computer, and by eliminating his involvement woith repetitive machine-code manipulations. Operating systems span a broad sectrum of complexity. Small systems, for example, provide only a rudimentary means for a programmer to enter and read 8-bit data from memory; large systems, on the other hand, can dynamically manage the allocation of all memory and peripherals.
Large systems allocate computer resources to more than one user in a multiprograming, multitasking, or a time sharing environment. A system of thsi mangnitude far exceeds the capabilities of the computer described in this book. This being the case, what would be a suitable operating system for the ZAP computer? As previously stated, the objective of an operating system is to manage the resources of the computer. The ZAP computer described here and enhanced with the minimum peripherals contains the following resources:

* Z80 microprocessor
* 1024 bytes of EPROM memory
* 1024 bytes of programmable memory
* Nineteen-key keyboard
* Two-charcter data display
* Four-character address display
* UART for serial I/O

The operating system must provide access to these resources ans give the user a way to manage them during execution of programms. The operating systems designed for the ZAP computer will include the following facilities and functions:

* Cold start
* Warm start
* Memory displey and replace
* Register display and replace
* Execute
* Serial input and output

Each will be explanined in detail concerning its functions and program implementation.

### Chapter 6.1 Operating System Functions

#### Chapter 6.1.1 Cold Start Operation

Text

#### Chapter 6.1.2 Warm Start Operation

### Chapter 6.2 Operating System Module Description

#### Chapter 6.2.1 Warm Start Module

Text

#### Chapter 6.2.2 Command Recognition Module

Text

#### Chapter 6.2.3 Restart Module

Text



## Chapter 7 Programmin an EPROM

Text

## Chapter 8 Connecting ZAP to the Real World

Text

## Chapter 9 Build a CRT Terminal

Text

## Appendix A Construction Techniques

Text

## Appendix B ASCII Codes

| Dec | Oct | Hex | Parity Space | Character | Control Keyboard Equivalent | Alternate Code Names
| ---:| ---:| ---:|:------------ |:--------- |:---------------------------:|:-------------------------------
| 000 | 000 | 00  | Even         | NUL       | @                           | NULL, CTRL SHIFT P, TAPE LEADER
| 001 | 001 | 01  | Odd          | SOH       | A                           | START OF HEADER, SOM
| 002 | 002 | 02  | Odd          | STX       | B                           | START OF TEXT, EOA


## Appendix C Manufacturers Specification Sheets

Text

## Appendix D ZAP Operating System

```
ZERO   EQU  0
ONE    EQU  1
TWO    EQU  2
THREE  EQU  3
FOUR   EQU  4
FIVE   EQU  5
EIGHT  EQU  8D
ADDIS1 EQU  5   * MSDS ADDRESS DISPLAY PORT
ADDIS2 EQU  6   * LSDS ADDRESS DISPLAY PORT
DATDIS EQU  7   * DATA DISPLAY PORT
EXECK  EQU 16D  * EXEC KEY
NEXTK  EQU 32D  * NEXT KEY
MEM    EQU 64D
UARTIO EQU  2
UARTST EQU  3
KEYPRT EQU  0  * KEYBOARD INPUT PORT
```
COLD sets the operating system stack pointer and enters the command recognition module.
```
0000 31 C4 07         COLD    LD   SP,SPSTRT          * INITIALIZE STACK POINTER
0003 C3 40 00                 JP   WARM01
```
Reset vector definition
```
0006 00 00                    DS   2
0008 C3 47 00         WARM    JP   WARM1              * RST 1 OR WARM START  
000B 00 00 00 00 00           DS   5
0010 C3 C5 07         RST2E   JP   RST2V              * RST 2 TRANSFER
0013 00 00 00 00 00           DS   5
0018 C3 C8 07         RST3E   JP   RST3V              * RST 3 TRANSFER
001B 00 00 00 00 00           DS   5
0020 C3 CB 07         RST4E   JP   RST4V              * RST 4 TRANSFER
0023 00 00 00 00 00           DS   5
0028 C3 CE 07         RST5E   JP   RST5V              * RST 5 TRANSFER
002B 00 00 00 00 00           DS   5
0030 C3 D1 07         RST6E   JP   RST6V              * RST 6 TRANSFER
0033 00 00 00 00 00           DS   5
0038 C3 D4 07         RST7E   JP   RST7V              * RST 7 TRANSFER
003B 00 00 00 00 00           DS   5
```

```
0040 ED 73 DB 07      WARM01  LD   (SPLSAV),SP
0044 C3 89 00                 JP   CRM                * GOTO COMMAND RECOGNITION MODULE
```
CRM is the command recognition module.
```
0089 CD F1 00         CRM     CALL CLDIS              * CLEAR DISPLAY, BUFFER AND FLAGS
008C 3E FF                    LD   A,255D
008E D3 05                    OUT  ADDIS1,A
0090 D3 06                    OUT  ADDIS2,A
0092 D3 07                    OUT  DATDIS,A
0094 CD 03 01                 CALL KEYIN              * GET INPUT CHARACTER
0097 06 40                    LD   B,MEM
0099 B8                       CP   B
009A CA F1 01                 JP   Z,MDAR             * JUMP IF MEMORY REQUEST
009D 04                       INC  B
009E B8                       CP   B
009F CA 4B 02                 JP   Z,RDAR             * JUMP IF REGISTER REQUEST
00A2 04                       INC  B
00A3 B8                       CP   B
00A4 CA 10 03                 JP   Z,EXEC             * JUMP IF EXEC REQUEST
00A7 C3 89 00                 JP   CRM
```
CLDIS clears the data and adress displays, sets the keyboard buffer to 0 and clears the keyboard flags.
```
00F1 3E 00            CLDIS   LD   A,ZERO
00F3 32 F1 07                 LD   (KFLAGS),A         * CLEAR FLAGS
00F6 32 F2 07                 LD   (KDATA1),A         * CLEAR BUFFER
00F9 32 F3 07                 LD   (KDATA2),A
00FC D3 07                    OUT  DATDIS,A           * CLEAR DATA FIELD DISPLAY
00FE D3 05                    OUT  ADDIS1,A           * CLEAR ADDRESS FIELD DISPLAY
0100 D3 06                    OUT  ADDIS2,A
0102 C9                       RET
```
KEYIN waits for input from the keyboard. Upon detecting data at the input port (0). Via the strobe bit (7) being set the data is input, the strobe bit is cleared and the character is returned to the user in A.
```
0103 DB 00            KEYIN   IN   A,KEYPRT           * INPUT DATA
0105 CB 03 01                 BIT  7,A
0107 CA 03 01                 JP   Z,KEYIN            * LOOP IF NO DATA
010A 32 F4 07                 LD   (TEMP1),A          * SAVE CHARACTER
010D DB 00            KEYIN1  IN   A,KEYPRT
010F CB 7F                    BIT  7,A
0111 C2 0D 01                 JP   NZ,KEYIN1          * JUMP IF STROBE PRESENT
0114 3A F4 07                 LD   A,(TEMP1)
0117 CB BF                    RES  7,A                * CLEAR STROBE
0119 C9                       RET
```
ONEC inputs one character followed by a NEXT or EXEC from the keyboard, validates it, and returns it to the user in KDATA2.
```
013A CD F1 00         ONEC    CALL CLDIS              * CLEAR DISPLAY, BUFFER AND FLAGS
013D CD 03 01                 CALL KEYIN              * GET CHARACTER
0140 D3 07                    OUT  DATDIS             * DISPLAY CHARACTER
0142 CD 5D 01                 CALL CHKC1              * CHECK CHARACTER
0145 CB 77                    BIT  6,A
0147 C2 51 01                 JP   NZ,ONEC1           * JUMP IF SHIFT
014A D6 10                    SUB  16D                * CHARACTER IS 0 - F
014C F2 3A 01                 JP   P,ONEC             * JUMP IF CHARACTER IS NOT 0 - F
014F C6 10                    ADD  16D
0151 32 F3 07         ONEC1   LD   (KDATA2),A         * SAVE CHARACTER
0154 CD 03 01                 CALL KEYIN              * GET NEXT CHARACTER
0157 CD 6A 01                 CALL CHKC2             
015A C3 51 01                 JP   ONEC1              * REPEAT IF NOT NEXT OR EXEC KEY
```
CHKC1 checks for a NEXT or EXEC on an initial character, sets the proper flags via KFLG02 or KFLG12 and returns to the user. 
If not NEXT or EXEC the routine returns to the originator of the request.
```
015D 06 20            CHKC1   LD   B,NEXTK            * CHECK FOR NEXT KEY
015F B8                       CP   B
0160 CA 1A 01                 JP   Z,KFLG02           * IF NEXT KEY JUMP
0163 06 10                    LD   B,EXECK            * CHECK FOR EXEC KEY
0165 B8                       CP   B
0166 CA 2A 01                 JP   Z,KFLG12           * IF EXEC KEY JUMP
0169 C9                       RET
```
CHKC2 checks for a NEXT or EXEC on a character, sets the proper flags via KFLG0 or KFLG1 and returns to the user. 
If not NEXT or EXEC the routine returns to the originator of the request.
```
016A 06 20            CHKC2   LD   B,NEXTK            * CHECK FOR NEXT KEY
016C B8                       CP   B
016D CA 1A 01                 JP   Z,KFLG0            * IF NEXT KEY JUMP
0170 06 10                    LD   B,EXECK            * CHECK FOR EXEC KEY
0172 B8                       CP   B
0173 CA 2A 01                 JP   Z,KFLG1            * IF EXEC KEY JUMP
0176 C9                       RET 
```
TWOC inputs two characters followed by a NEXT or EXEC from the keyboard, validates it, and returns them to the user in KDATA1.
```
0177 CD A0 A1         TWOC    CALL CLDAT
```
CLDAT clears the input buffer, flags and data display.
```
01A0 3E 00            CLDAT   LD   A,ZERO
01A2 32 F1 07                 LD   (KFLAGS),A         * CLEAR FLAGS
01A5 32 F2 07                 LD   (KDATA1),A         * CLEAR BUFFER
01A8 32 F3 07                 LD   (KDATA2),A
01AB C9                       RET
```
CLADD clears the address display.
```
01AC 3E 00            CLADD   LD   A,ZERO             * CLEAR ADDRESS FIELD DISPLAY
01AE 32 F1 07                 OUT  ADDIS1
01B0 32 F2 07                 LD   ADDIS2
01B2 32 F3 07                 RET
```
FOURC inputs four characters followed by a NEXT or EXEC from the keyboard, validates it, and returns them to the user in KDATA1 and KDATA2.
```
01B3 CD A0 01         FOURC   CALL CLDAT              * CLEAR FLAGS AND BUFFER
01B6 CD 03 01                 CALL KEYIN              * GET INPUT CHARACTER
01B9 CD 5D 01                 CALL CHKC1              * CHECK FOR NEXT OR EXEC KEY

```
MDAR is the memory display and replace module
```
01F1 3E 00            MDAR    LD   A,ZERO

```
RDAR is the register display and replace module
```
024B CD 3A 01         RDAR    CALL ONEC

```
EXEC
```
0310 CD AC 01         EXEC    CALL CLADD
0313 CD B3 01                 CALL FOURC              * GET RESTART ADDRESS
```
 
```
07C4 00               SPSTRT  DB   0
```
User restart area
```
07C5 00 00 00         RST2V   DS   3                  * USER BRANCH AREA FOR RST 2
07C8 00 00 00         RST3V   DS   3                  * USER BRANCH AREA FOR RST 3
07CB 00 00 00         RST4V   DS   3                  * USER BRANCH AREA FOR RST 4
07CE 00 00 00         RST5V   DS   3                  * USER BRANCH AREA FOR RST 5
07D1 00 00 00         RST6V   DS   3                  * USER BRANCH AREA FOR RST 6
07D4 00 00 00         RST7V   DS   3                  * USER BRANCH AREA FOR RST 7
```
Register save area
```
07DB 00               SPLSAV  DB   0
07DE 00               SPHSAV  DB   0
```
Data storage area
```
07F1 00               KFLAGS  DB   0
07F2 00               KDATA1  DB   0
07F3 00               KDATA2  DB   0
07F4 00               TEMP1   DB   0
07F5 00               TEMP2   DB   0
07F6 00               MBASE1  DB   0
07F7 00               MBASE2  DB   0
```

## Appendix E Z80 CPU Technical Specifications

Text

## Glossary

**Accumulator** A temorary register where results of calculations may be stored by the central processor. One or more accumulators may be part of the arithmentic-logical unit.

**Acoustal coupler** A device that permits a terminal to be connected to the computer via a telephone line. It connects to the telephone handset.

**Address** An identifying number or label for locations in the memory

**Algorithm** A step-by-step solution to a problem in a finite number of steps. A specific procedure for accomplishing a desired result.

**ASCII** American Standard Code for Information Interchange. Widely used 7-bit standard code. Also known as USASCII; IBM uses EBCDIC, which has 8 bits.

**Assembler** A program that converts symbolic instructions into machine macro-instructions

**Backplane** A board equipped with plugs interconnected by buses into which the modules which make up a computer may be inserted. Also known as a motherboard.

## Index

