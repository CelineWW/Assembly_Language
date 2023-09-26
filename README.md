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