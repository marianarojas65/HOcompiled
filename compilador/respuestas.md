Trabajamos en el programa calculator.c

1. Pre-procesador: `make preprocessing`
Ejecutando este paso 
gcc -E calculator.c -o calculator.pp_c 

Me genera una nueva fila calculator.pp_c donde se incluyeron las directivas tales como include:
#include <stdio.h> 
#include <features.h>
y `librerías dinámicas` de linux
#include <cdefs.h>
#include <wordsize.h>
#include <stubs.h>
#include <stddef.h>
#include <types.h>

y aprovecha funciones ya definidas
extern struct _IO_FILE_plus _IO_2_1_stdin_;
extern struct _IO_FILE_plus _IO_2_1_stdout_;
extern struct _IO_FILE_plus _IO_2_1_stderr_;

2. Compilacion I: `make assembler`
gcc -masm=intel -S calculator.c -o calculator.asm 

Aquí está la diferencia si compilamos con intel o gnu. En nuestro caso estamos compilando con gnu.

a) Front end
Genera una representación intermedia (IR) 

b) Middle end
Optimiza la representación intermedia 

c) Back end
Genera el código assembler

	.file	"calculator.pp.c"
main:
	push	rbp
Esto me lleva a la info a la memoria RAM para realizar los cálculos
Estos son los números que voy a sumar
	mov	esi, 11
	mov	edi, 31
y llamo a la rutina que los suma
	call	add_numbers
	call	printf
La rutina add_numbers
add_numbers:
.LFB1:
	.cfi_startproc
	push	rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	mov	rbp, rsp
	.cfi_def_cfa_register 6
	mov	DWORD PTR [rbp-4], edi
	mov	DWORD PTR [rbp-8], esi
	mov	edx, DWORD PTR [rbp-4]
	mov	eax, DWORD PTR [rbp-8]
	add	eax, edx
	pop	rbp
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc


3. Compilacion II: `make object`
gcc -c calculator.c -o calculator.o
El archivo es muy criptico y no se puede entender.
nm calculator.o

000000000000003c T add_numbers --  descriptores
0000000000000000 T main        --  descriptores  
                 U printf      --  entrada

4. Linkeo: `make executable`
gcc -v calculator.o -o calculator
En el linkeo me dice que compilador usa
COLLECT_GCC=gcc
Que está usando un entorno en linux
COLLECT_LTO_WRAPPER=/usr/lib/gcc/x86_64-linux-gnu/5/lto-wrapper

Finalmente el ejecutable es operativo
./calculator 
I know how to add! 31 + 11 is 42







