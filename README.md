# Excel 16-Bit CPU
O repositório Excel 16-Bit CPU contém os seguintes arquivos principais:
```
CPU.xlsx - Planilha principal que contém a CPU
ROM.xlsx - Planilha ROM é lida pela CPU quando a chave de leitura da ROM está ligada
InstructionSet.xlsx - Explica o conjunto de instruções (ISA) da CPU
compileExcelASM16.py - O compilador Excel-ASM16
Excel-ASM16.xml - Markdown para a linguagem Excel-ASM16 compatível com o Notepad++
Sample Programs - Pasta de programas de exemplo para a CPU Excel
```

O arquivo CPU.xlsx apresenta uma CPU de 16 bits, 16 registradores de propósito geral, 128 KB de RAM e um display de 128x128.

O Cálculo Iterativo deve estar ativado. Isso pode ser feito a partir de: "Arquivo" -> "Opções" -> "Fórmulas" -> selecionar "Habilitar cálculo interativo" e alterar **"Número Máximo de interações"** em **"1"** 

A CPU é alimentada por um clock definido em "B2". Este sinal do clock será atualizado nas condições normais de recálculo de uma planilha do Excel. Pressionar a tecla "F9" irá recalcular a planilha. 

O botão "Reset" na célula "F2", se configurado como verdadeiro, irá reiniciar o registrador PC de volta para 0. 

O computador CPU.xlsx pode ser controlado tanto no modo automático quanto em modo manual. Isso pode ser definido pelo botão em "J2". Se definido como verdadeiro, quando o sinal do clock em "B2" estiver alto, então a CPU realizará a operação especificada no slot de substituição na "Unidade de Busca" (Fetch Unit) na célula "D8". Se falso, então a CPU executará a operação obtida da tabela de memória, conforme especificado pelo registrador "PC". 

O botão "Reset RAM", se configurado como verdadeiro, irá reiniciar todas as unidades de memória para 0. 

O botão "Read ROM", se configurado como verdadeiro, copiará os valores da tabela de memória na planilha ROM.xlsx para a tabela de RAM na planilha CPU.xlsx. 

A operação normal da CPU consiste em definir o botão "Reset" como alto, alternar os botões "Reset RAM" ou "Read ROM" ligando-od e desligando-os novamente (causa o reset da RAM ou a leitura da ROM para a tabela de RAM) e, em seguida, desligar o botão "Reset". A CPU é então configurada para executar um programa no modo manual ou realizará o programa especificado na RAM. 

A CPU é projetada para operar de acordo com a arquitetura do conjunto de instruções especificada na planilha InstructionSet.xlsx. 

Aviso (atençõa): Não é possível simplesmente pressionar a tecla "F9" continuamente, pois leva tempo para o Excel atualizar a grande quantidade de células, é recomendável aguardar até que o indicativo "Pronto" possa ser visto na linha de status no canto inferior esquerdo do Excel antes de continuar a pressionar a tecla "F9". 

Alternativamente, programas podem ser escritos na linguagem Excel-ASM16 e compilados para a planilha ROM.xlsx.

O Excel-ASM16 possui 24 instruções diferentes, sem diferenciar maiúsculas de minúsculas. 
Existem três operandos diferentes que são usados em cada instrução
```
	REG	; refere-se a qualquer um dos 16 registradores de propósito geral
	Exemplo: R0, R1, R15 &c.
	
	MEM	; refere-se a qualquer unidade de memória endereçável de 16 bits (formatada em hexadecimal)
	Exemplo: @0000, @F000, @FFFF, &c.

	IMD	; refere-se a um número imediato geralmente com 16 bits de comprimento, exceto no caso de ROL e ROR,
		; que podem ser expressos tanto em decimal quanto em hexadecimal
	Exemplo. #0000, $0CCC, #60340, $FF10, &c.
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











