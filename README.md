# Assembly_Language
AL_programming

## Messages
```
_start:	            ;tells linker entry point
   mov	eax,4       ;system call number (sys_write)
   mov	ebx,1       ;file descriptor (stdout)
   mov	ecx,msg     ;message to write, A pointer to the variable 'msg'
   mov	edx,len     ;message length
   int	0x80        ;call kernel
	
   mov	eax,1       ;system call number (sys_exit)
   int	0x80        ;call kernel

section	.data
    msg db 'I came,', 0xa, 'I saw,', 0xa, 'I conquered.',0xa 
    len equ $ - msg
```
![I_came](https://github.com/CelineWW/Assembly_Language/blob/main/Message/I_came.png)

## Varibles
initialized and uninitialized variables
```
section .text
        global _start

_start:
   	mov eax,[var1]  ;load var1 to eax
   	mov ebx,[var2]  ;load var2 to ebx
   	add eax,ebx     ;update eax result by adding var1 and var2  
   	mov [result], eax  ;store eax value to result variable
  
        mov eax,1	;set eax register to 1 
        int 0x80	;interrupt 0x80 

section .bss
result resb 1    ;define uninitialized variable result.

section .data
    var1 dd 10     ;initialize var1 to be 10.
    var2 dd 15     ;initialize var2 to be 15.
```
![w5b_gdb3](https://github.com/CelineWW/Assembly_Language/blob/main/Variables/w5b_gdb3.png)

## Arithmetic
Arithmetic operation
- w6_1.asm
    result = -var1 * 10 
    result = -5 * 10 = -50
- w6_2.asm
    result = var1 + var2 + var3 + var4 
    result = 1 + 2 + 3 + 4 = 10
- w6_3.asm
    result = (-var1 * var2) + var3
    result = (-2 * 3) + 17 = 11
- w6_4.asm
    result = (var1 * 2)/(var2 - 3)
    result = (10 * 2)/(8 - 3) = 4
```
section .text
    global _start

_start:
    mov eax,[var1]        ;store var1=10 to eax; eax is 10 
    mov dl, 2             ;store 2 into dl; dl is 2
    mul dl                ;multiply eax=10 by 2; eax is 20
    mov ebx,[var2]        ;store var2=8 to ebx; ebx is 8
    sub ebx,3             ;substitue ebx=8 by 3; ebx is 5
    mov [var2], ebx       ;return ebx=5 back to var2
    mov bl,[var2]         ;store ebx=5 as a divisor into bl
    div bl                ;divide eax=20 by bl=5; eax is 4
    mov [result],eax      ;store eax=4 into result variable
    
    mov eax,1
    int 0x80

section .data
    var1 DD 10            ;var1 is assigned 10 
    var2 DD 8             ;var2 is assigned 8
    
segment .bss
    result resb 1         ;uninitialized variable

```
![w6_4](https://github.com/CelineWW/Assembly_Language/blob/main/Arithmetic/w6_4.png)

## Logical Instructions
Logical Instructions: AND, OR, XOR, TEST, NOT.
Use GDB to debugging and checking register process.

- XOR to clear register.
    ![W7_after xor.png](https://github.com/CelineWW/Assembly_Language_Programming/blob/main/Logical%20Instructions/W7_after%20xor.png)

- TEST to distinguish odd and even number.
    If odd(var=5), then multiple 2, save to result (10), and print "Odd number" on console.
    ![W7_odd*2_2.png](https://github.com/CelineWW/Assembly_Language_Programming/blob/main/Logical%20Instructions/W7_odd*2_2.png)
    If even(var=8), then divide 2, save to result (4), and print "Even number" on console.
    ![W7_even:2_2.png](https://github.com/CelineWW/Assembly_Language_Programming/blob/main/Logical%20Instructions/W7_even%3A2_2.png)
