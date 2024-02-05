# Excel 16-Bit CPU

ESTE REPOSITÓRIO APRESENTA O PROJETO "Excel 16-Bit CPU" DE AUTORIA DE "[Inkbox](https://github.com/InkboxSoftware)" TRADUZIDO PARA A LÍNGUA PORTUGUESA SEM A REALIZAÇÃO DE NENHUMA ALTERAÇÃO LÓGICA NO MATERIAL ORIGINAL, EXCETO EM SEU TEXTO EM INGLÊS.

O repositório Excel 16-Bit CPU contém os seguintes arquivos principais:
```
CPU.xlsx             - Planilha principal que contém a CPU
ROM.xlsx             - Planilha ROM é lida pela CPU quando a chave de leitura da ROM está ligada
InstructionSet.xlsx  - Explica o conjunto de instruções (ISA) da CPU
compileExcelASM16.py - O compilador Excel-ASM16
Excel-ASM16.xml      - Markdown para a linguagem Excel-ASM16 compatível com o Notepad++
Sample Programs      - Pasta de programas de exemplo para a CPU Excel
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
	
	MEM	; refere-se a qualquer unidade de memória endereçável de 16 bits (formatada em
                ; hexadecimal)
	Exemplo: @0000, @F000, @FFFF, &c.

	IMD	; refere-se a um número imediato geralmente com 16 bits de comprimento, exceto no
                ; caso de ROL e ROR, que podem ser expressos tanto em decimal quanto em hexadecimal
	Exemplo. #0000, $0CCC, #60340, $FF10, &c.
```
### LOAD (CARREGAMENTO)
```
	LOAD REG MEM	; carrega a unidade de memória especificada em REG
	LOAD REG IMD	; carrega o valor imediato de 16 bits especificado em REG
	LOAD REG REG	; carrega a unidade de memória no endereço armazenado em REGB em REGA
```
### STORE (ARMAZENAMENTO)
```
	STORE REG MEM	; armazena o valor de REG no endereço especificado
	STORE REG REG 	; armazena o valor de REGA na unidade de memória no endereço em REGB
```
### JUMP (SALTO)
```
	JMP IMD		; define registrador PC para o valor imediato de 16 bits
	JEQ IMD		; se ZF = 0, define PC para o valor imediato de 16 bits
	JLT IMD		; se CF = 0, define PC para o valor imediato de 16 bits 
	JGE IMD		; se CF = 1 ou ZF = 1, define PC para o valor imediato de 16 bits 
```
### TRAN (TRANSFERÊNCIA)
```
	TRAN REG REG	; transfere o valor de REGA para REGB
```
### INSTRUÇÕES ALGÉBRICAS
### ADD
```
	ADD REG REG	; REGA + REGB + CF, resultado armazenado em REGA
```
### SUB
```
	SUB REG REG	; (REGA - REGB) - CF, resultado armazenado em REGA
```
### MULT
```
	MULT REG REG	; REGA * REGB, resultado armazenado de 16 bits mais baixo em REGA,
                        ; resultado armazenado de 16 bits mais alts em REGB
```
### DIV
```
	DIV REG REG	; REGA / REGB resultado armazenado em REGA, REGA MOD REGB armazenado em REGB
```
### INC
```
	INC REG		; REGA++, CF não é afetado
```
### DEC
```
	DEC REG		; REGA--, CF não é afetado
```
### INSTRUÇÕES BIT A BIT (BITWISE)
### AND
```
	AND REG REG	; REGA AND REGB, resultado armazenado em REGA
```
### OR
```
	OR REG REG	; REGA OR REGB, resultado armazenado em REGA
```
### XOR
```
	XOR REG REG	; REGA XOR REGB, resultado armazenado em REGA
```
### NOT
```
	NOT REG 	; NOT REGA, resultado armazenado em REGA
```
### INSTRUÇÕES DE ROTAÇÃO
### ROL
```
	ROL REG IMD	; rotação para a esquerda dos bits de REGA realizada IMD vezes
			; MD é um valor de 4 bit
```
### ROR
```
	ROR REG IMD	; rotação para a direita dos bits de REGA realizada IMD vezes
			; MD é um valor de 4 bit
```
### INSTRUÇÕES DE FLAG (BANDEIRA)
```
	CLC		; põe CF com 0
	STC		; põe CF com 1 
```
### NOP
```
	NOP		; não afeta nenhum registrador ou memória
```
### ORG
```
	ORG IMD		; define a localização da próxima instrução
			; deve estar mais distante do que o comprimento atual do programa
```
### INC
```
	INC "file.bin"	; copia o arquivo binário para o programa
```

### Compilação
Após escrever um programa, compile-o com a instrução de linha de comando:
```
	py compileExcelASM16.py program.s ROM.xlsx
```
Onde **program.s** é o arquivo do programa do usuário, e ROM.xlsx é a planilha da ROM

Após a compilação bem-sucedida, o programa pode ser transferido para a planilha CPU.xlsx ao acionar o botão "Read ROM" na parte superior da planilha. Observe que o arquivo ROM.xlsx deve estar aberto para que os dados sejam atualizados corretamente





