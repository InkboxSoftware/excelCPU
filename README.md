# Excel 16-Bit CPU
The Excel 16-Bit CPU repository contains the following main files:
```
CPU.xlsx - The main spreadsheet which contains the CPU
ROM.xlsx - The ROM spreadsheet read by the CPU when the read ROM switch is turned on
InstructionSet.xlsx - Explains the ISA of the CPU
compileExcelASM16.py - The Excel-ASM16 compiler
Excel-ASM16.xml - Markdown for the Excel-ASM16 language compatible with Notepad++
Sample Programs - Folder of sample programs for the Excel CPU
```

The CPU.xlsx file features a 16-bit CPU, 16 general purpose registers, 128KB of RAM, and a 128x128 display.

Iterative Calcuation must be turned on. This can be done by going to File -> Options -> Formulas -> then Enable Iterative Calculation and **set Maximum Iterations to 1**

The CPU runs off a clock signal set in B2. This clock signal will update under the normal conditions of recalculation within an Excel spreadsheet. Pressing the F9 key will recalculate the spreadsheet. 

The Reset Button in the F2 cell, if set to true, will reset the PC register back to 0. 

The computer in the CPU.xlsx file can be controlled either in automatic or manual mode. This is controlled by the button in J2. If set to true, when the clock signal from B2 is high, then the CPU will carry out the operation specified in the override slot in the Fetch Unit in cell D8. If false, then the CPU will execute the operation retrieved from the memory table as specified by the PC register. 

The Reset RAM button, if set to true, will reset every memory unit to 0. 

The Read ROM button, if set to true, will copy the values of the memory table in the ROM.xlsx spreadsheet onto the RAM table of the CPU.xlsx spreadsheet. 

Normal operation of the CPU consists of setting the Reset Button to high, either flipping the Reset RAM or Read ROM buttons on and off again (causing the RAM to be reset or the ROM to be read into the RAM table), and then turning off the Reset Button. The CPU is then set up to either run a program in Manual mode, or will carry out the program specified in RAM. 


The CPU is designed to run according to the instruction set architecture specified in the InstructionSet.xlsx spreadsheet. 

Warning: It is not possible to simply mash the F9 key as fast as possible, it takes time for Excel to update so many cells, it is recommended to wait until the text "Ready" can be seen in the bottom left corner of Excel can be seen before continuing to press the F9 key. 


Alternatively, programs can be written in the Excel-ASM16 language and compiled to the ROM.xlsx spreadsheet.

Excel-ASM16 features 24 different case-insensitive instructions. 
There are three different operands that are used in each instruction
```
	REG	; refers to any of the 16 general purpose registers
	E.G. R0, R1, R15 &c.
	
	MEM	; refers to any 16-bit addressable memory unit (formatted in hexadecimal)
	E.G. @0000, @F000, @FFFF, &c.

	IMD	; refers to an immediate number usually 16-bits long, except in the case of ROL and ROR
		; can be defined either in decimal or hexadecimal
	E.G. #0000, $0CCC, #60340, $FF10, &c.
```
### LOAD
```
	LOAD REG MEM	; loads the specified memory unit into REG
	LOAD REG IMD	; load specified 16-bit immediate value into REG
	LOAD REG REG	; loads memory unit at the address stored in REGB into REGA
```
### STORE
```
	STORE REG MEM	; stores the value of REG to the address specified
	STORE REG REG 	; stores the value of REGA into the memory unit at the address in REGB
```
### JUMP
```
	JMP IMD		; sets PC to the immediate 16-bit value
	JEQ IMD		; if ZF = 0, sets PC to the immediate 16-bit value
	JLT IMD		; if CF = 0, sets PC to the immediate 16-bit value 
	JGE IMD		; if CF = 1 or ZF = 1, sets PC to the immediate 16-bit value 
```
### TRAN
```
	TRAN REG REG	; transfers value from REGA to REGB
```
### ALGEBRAIC INSTRUCTIONS
### ADD
```
	ADD REG REG	; REGA + REGB + CF, result stored in REGA
```
### SUB
```
	SUB REG REG	; (REGA - REGB) - CF, result stored in REGA
```
### MULT
```
	MULT REG REG	; REGA * REGB, low 16-bit result stored in REGA, high 16-bit result stored in REGB
```
### DIV
```
	DIV REG REG	; REGA / REGB result stored in REGA, REGA MOD REGB stored in REGB
```
### INC
```
	INC REG	; REGA++, CF not affected
```
### DEC
```
	DEC REG	; REGA--, CF not affected
```
### BITWISE INSTRUCTIONS
### AND
```
	AND REG REG	; REGA AND REGB, result stored in REGA
```
### OR
```
	OR REG REG		; REGA OR REGB, result stored in REGA
```
### XOR
```
	XOR REG REG	; REGA XOR REGB, result stored in REGA
```
### NOT
```
	NOT REG 		; NOT REGA, result stored in REGA
```
### ROLL INSTRUCTIONS
### ROL
```
	ROL REG IMD	; leftwise roll of bits of REGA carried out IMD times
				; IMD is a 4-bit value
```
### ROR
```
	ROR REG IMD	; rightwise roll of bits of REGA carried out IMD times
				; IMD is a 4-bit value
```
### Flag instructions
```
	CLC			; sets CF to 0
	STC			; sets CF to 1 
```
### NOP
```
	NOP			; does not effect any registers or memory
```
### ORG
```
	ORG IMD		; sets the location of the next instruction
				; must be further than the current length of program
```
### INC
```
	INC "file.bin"	; copies the binary file into the program
```

### Compiling
After having written a program, it is compiled with the commandline instruction
```
	py compileExcelASM16.py program.s ROM.xlsx
```
Where **program.s** is the user's program file, and ROM.xlsx is the ROM spreadsheet

After compiling successfully, the program can be transferred into the CPU.xlsx program by flipping the Read ROM button at the top of the spreadsheet. Note, the ROM.xlsx file must be open for the data to update correctly. 











