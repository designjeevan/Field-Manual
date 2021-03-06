# Calling Conventions

## For x86:

**Arguments are passed in as data on stack**

For `func(arg1,arg2...,argn)`, assembly code is:
```
push argn
...
push arg2
push arg1
call func 
```
Upon calling a function, the arguments are pushed on to the stack, followed by pushing the current eip to stack, followed by a jump to the function address. 

### Hence, payload must look like:
**Junk + Address of func + Return address + Pointer to arg1 + Pointer to arg2 + ...**

## For x64:

**Arguments are passed in as data on registers in the order:**
```
rdi -> rsi -> rdx -> r10 -> r8 -> r9
```
*First argument in rdi, then next arg in rsi, and so on...*

For `func(arg1,arg2...,argn)`, assembly code may look something like:
```
mov rdi, arg1
...
call func
```
### Hence, payload must look like:
**Junk + Address of pop-pop...-ret gadget that puts appropriate data on appropriate register(s) + Pointer to args + Address of func that the gadget will return to**
