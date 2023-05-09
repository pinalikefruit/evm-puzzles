# Puzzle 5

| Step| OPCODE| NAME|
|-----|-----|-----|
|00  | 34   | CALLVALUE
|01  | 80   | DUP1
|02  | 02   | MUL
|03  | 610100   | PUSH2 0100
|06  | 14   | EQ
|07  | 600C   | PUSH1 0C
|09  | 57   | JUMPI
|0A  | FD   | REVERT
|0B  | FD   | REVERT
|0C  | 5B   | JUMPDEST
|0D  | 00   | STOP
|0E  | FD   | REVERT
|0A  | FD   | REVERT


Run Bytecode: [34800261010014600C57FDFD5B00FDFD](hhttps://www.evm.codes/playground?fork=merge&unit=Wei&codeType=Bytecode&code='34800261010014600C57FDFD5B00FDFD'_)


### Opcodes: explained in a simple way
- New ones:

**DUP1**: This is easy, just duplicate 1st stack item. 

**MUL** :Take two input from stack and do multiplication operation.

**PUSH1**: 	Place 1 byte item on stack, has its argmument.

**PUSH2**: Place 2 byte item on stack, has its argmument.

**EQ**: Equality comparison. If the result is true their result is 1 otherwise 0.

**JUMPI**: this is a little different from **JUMP**, this take 2 argument one is for **JUMPDEST** step and the second one is a conditional if the value is different to 0 is **JUMP** to **JUMPDEST** if the value is 0 it will continue to the next instruction.



## Solution 

Let's break this puzzle:

1. We'll figure this out from the end to the beginning. For win we have to go **JUMPDEST** is `C` place and **JUMPI** is a conditional, we need their take 2 arguments [C, !=0] , The value C we haved for the step 07 **PUSH1 0C**. 
2. Remember **EQ** have to be equal to result is 1, well this is the trick, We  need some value that multiplicate for themself is es equal to 0100. 
3. Here is the thing, `0x100` in decimal is `256`. So, what number multiplication for themselve is `256`, we can solve with raiz of `256^(1/2) = 16` if you convert to hexadecimal is `0x10`.
4. Resumen, if you pass `0x10` and the multiplication is equal to `0x100`, so, EQ is true = 1, and **JUMPI** have the conditional for use their counter to **JUMPDEST**.

 

You get it ? Yes ? Good job, go to the next level !!!.



- Remember:

**CALLVALUE** : the value we are sent **CALLVALUE** push in the stack.

**CALLDATASIZE** : Get size of input data in current environment. For example: you send some value for calldata and this opcode take the size.

**JUMP**:  It is take in the stack its input, it use for jump looking **JUMPDEST** for continue.

**JUMPDEST**:Its just a valid destination for **JUMP**(our case) or **JUMPI**.

**REVERT** (GAME OVER): Stop the current context execution. So, you need avoid.

**STOP** ( WE DIT IT): Exit the current context successfully. 

**CODESIZE** : Each instruction occupies one byte, **CODESIZE** return the size value. for example:
        When we run our code  **CODESIZE** return `a`. But, why? 
        Well, the bytecode is 34380356FDFD5B00FDFD if each instruction ocupies one byte, 34 38 03 56 FD FD 5B 00 FD FD = 10 bytes = `a` in hexadecimal.
**SUB**:  Its take 2 values from the stack and do a subtraction operation

**XOR** : Bitwise XOR operation. e.g. 
