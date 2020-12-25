# [Day 17] Reverse Engineering - ReverseELFneering

## Topics

- x86-64 Assembly

### What is the value of local_ch when its corresponding movl instruction is called (first if multiple)? 
```
1
```
1. Load the program into r2 with `r2 -d ./challenge1`
2. Analyze the program with `aa`
3. Check if there's a __main__ function with `afl | grep main`
4. Print the main function `pdf @main`

__output:__
```
            ;-- main:
/ (fcn) sym.main 35
|   sym.main ();
|           ; var int local_ch @ rbp-0xc
|           ; var int local_8h @ rbp-0x8
|           ; var int local_4h @ rbp-0x4
|              ; DATA XREF from 0x00400a4d (entry0)
|           0x00400b4d      55             push rbp
|           0x00400b4e      4889e5         mov rbp, rsp
|           0x00400b51      c745f4010000.  mov dword [local_ch], 1
|           0x00400b58      c745f8060000.  mov dword [local_8h], 6
|           0x00400b5f      8b45f4         mov eax, dword [local_ch]
|           0x00400b62      0faf45f8       imul eax, dword [local_8h]
|           0x00400b66      8945fc         mov dword [local_4h], eax
|           0x00400b69      b800000000     mov eax, 0
|           0x00400b6e      5d             pop rbp
\           0x00400b6f      c3             ret

```
We can see the following instruction:
```
0x00400b51      c745f4010000.  mov dword [local_ch], 1
```
This instruction is moving the number __1__ into the __local_ch__ variable.

### What is the value of eax when the imull instruction is called?
```
6
```
1. Set a breakpoint in the instruction after the imul `db 0x00400b66`
2. Run the program until it hits the breakpoint `dc`
3. When it hits the bp, check the contents of the eax with `dr`. It should say `rax = 0x00000006`

### What is the value of local_4h before eax is set to 0?
```
6
```
1. Advance into the next instruction with `ds`
2. Check the contents of the __local_4h__ variable with `px @rbp-0x4`. It should have the value __6__.
