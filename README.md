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
