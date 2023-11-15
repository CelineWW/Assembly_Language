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


## Conditional Instructions
Using conditional jumps to skip code or execute code backward steps.
- Generate a sequence of odd numbers from 1 to 20.
```_start:
        mov eax,0
    
label:
        inc eax
        test eax,1	;test if eax is odd(1) or even(0).
        jnz odd

        cmp eax,20
        jl label
        je exit

odd:
        mov [odds],eax	;save value to odds if it is an odd number
        jmp label

exit:
        mov eax,1
        int 0x80
```

 ![W9_odds_console.png](https://github.com/CelineWW/Assembly_Language_Programming/blob/main/Conditional%20Instructions/W9_odds_console.png)

- Find the largest number among the five integer numbers.
```
_start:
	mov eax,[num1]
    	cmp eax,[num2]	
    	jg label1
    	mov eax,[num2]	;replace eax with num2 if num2 > num1
    
label1: 
    	cmp eax,[num3]
   	jg label2
    	mov eax,[num3]	;replace eax with num3 if num3 > eax
```
![W9_max_console.png](https://github.com/CelineWW/Assembly_Language_Programming/blob/main/Conditional%20Instructions/W9_max_console.png)

## Loops and Arrays
1. Generate a counter using an EBX register to increment eax from 0 to 10.
    ```
    _start: 
        mov ebx,10
        mov eax,0

    compare:
        cmp ebx,0	
        jg label	
        jmp exit

    label:  
        inc eax
    	dec ebx
        jmp compare

    exit:
        mov eax,1
        int 0x80
    ```

2. Calculate the final number of the first 10 Fibonacci numbers starting from 0. The result is confirmed to be 55 via GDB.
    ```
    _start:
        mov ecx,10
        mov eax,1	;second Fibonacci number 
        mov ebx,0	;first Fibonacci number
    label:
        mov edx,eax	;temporarily save second last number to edx
        add eax,ebx	;add last two numbers and save to eax, now eax is the last number
        mov ebx,edx	;move back edx to ebx, now ebx is the second last number 
        loop label	;loop label block for 10 times
	
        mov [result],ebx
        mov eax,1
        int 0x80
    ```
    ![w10_2_console.png](https://github.com/CelineWW/Assembly_Language_Programming/blob/main/Loops%20and%20Arrays/w10_2_console.png)

3. Define the array with length 3 and, type integer, find the largest element in the array. 
    ```
    _start:
        mov eax,4	;initialize eax to be array size 
        mov ebx,0
            mov edx,0
        mov ecx,array	;move array address from memory to register

    replace:
            mov edx,ebx	;if ebx is greater than edx, replace edx with ebx
            jmp loop
        
    loop:
        mov ebx,[ecx]
            cmp ebx,edx     ;compare ebx and edx
        jg replace
        add ecx,4	;move pointer to next element
        dec eax		;go to next element
        jnz loop

            mov [result],edx  ;save largest number of the array into result variable
        
        mov eax,1
        int 0x80

    section .data
        array DD 2,5,3,7
    ```

## Procedures
    Generate English uppercase characters from A to Z. After every character, there is a line feed. Code optimized with procedures and loops.
    ```
    _start:
        mov ecx,26
        mov eax,65  ;initialize eax to ASCII=65 'A'

    label:
        push ecx    ;ecx = 26 is saved in the stack
        mov [temp],eax  
        mov ecx,temp
        push eax    ;eax = 65 is saved in the stack
        call output
        mov eax,10
        mov [temp],eax
        mov ecx,temp
        call output
        pop eax	    ;eax is removed from the stack and saved in the eax
        pop ecx     ;ecx is removed from the stack and saved in the ecx
        inc eax	    ;increment eax to be next ASCII code
        loop label

    output:
        mov eax,4
        mov ebx,1
        mov edx,1
        int 0x80
        ret
    ```
![W11_Procedures_AZ.png](https://github.com/CelineWW/Assembly_Language_Programming/blob/main/Procedures/W11_Procedures_AZ.png)


## Functions
1. Assign a number to a register or a variable, pass a number to the function and display the result, 'odd' or 'even'.
    ```
    _foobar:               ;take 1 argument to check it is odd or even 
	push ebp
	mov ebp,esp    ;move base pointer to the top of stack
	mov eax,DWORD[ebp+8]  
    	test eax,1
    	jz even        ;if eax is even, AND operation return value 0
    	jnz odd	       ;if eax is odd, AND operation return value 1

odd:
    	mov eax,4
    	mov ebx,1
    	mov ecx,msg1   ;print message 'Odd' on the console
    	mov edx,len1
    	int 0x80
    	leave         
    	ret            ;pointer return to the function address

even:
    	mov eax,4
    	mov ebx,1
    	mov ecx,msg2.  ;print message 'Even' on the console
    	mov edx,len2
    	int 0x80
    	leave
    	ret            ;pointer return to the function address
    ```
![W12_Odd_Run.png](https://github.com/CelineWW/Assembly_Language_Programming/blob/main/Functions/W12_Odd_Run.png)

2. Create a function that takes 3 arguments (integers) and returns the largest integer. 

    ```
    _largest:
        push ebp	
        mov ebp,esp
        sub esp,4
        
        mov eax,DWORD[ebp+16]
        cmp eax,DWORD[ebp+12]          ;compare first two numbers, save larger one into eax
        jg label
        mov eax,DWORD[ebp+12]	      ;if DWORD[ebp+8] > eax, replace eax with larger number

    label:				      
        cmp eax,DWORD[ebp+8] 	      ;compare second two numbers, save larger one into eax
        jg exit
        mov eax,DWORD[ebp+8]	      ;if DWORD[ebp+4] > eax, replace eax with larger number
        
    exit:
        mov DWORD[ebp-4],eax
        leave 
        ret

    section .text
        global _start

    _start:
        push 5			       ;add 3 parameters
        push 10
        push 20
        call _largest		       ;call the function

        mov ebx,0
        mov eax,1
        int 0x80		       ;exit the program
    ```
## File Management
1. Create a text-based filecalled"quotes.txt" and add contents.
![W13_1.png](https://github.com/CelineWW/Assembly_Language_Programming/blob/main/File%20Management/W13_1.png)
2. Append quotes in the same file.
![W13_2.png](https://github.com/CelineWW/Assembly_Language_Programming/blob/main/File%20Management/W13_2.png)