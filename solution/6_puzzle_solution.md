# Puzzle 6

| Step| OPCODE| NAME|
|-----|-----|-----|
|00  | 6000   | PUSH1 00
|02  | 35   | CALLDATALOAD
|03  | 56   | JUMP
|04  | FD   | REVERT
|05  | FD   | REVERT
|06  | FD   | REVERT
|07  | FD   | REVERT
|08  | FD   | REVERT
|09  | FD   | REVERT
|0A  | 5B   | JUMPDEST
|0B  | 00   | STOP



Run Bytecode: [60003556FDFDFDFDFDFD5B00](https://www.evm.codes/playground?fork=merge&callValue=16&unit=Wei&codeType=Bytecode&code='60003556~~~5B00'~FDFD%01~_)


### Opcodes: explained in a simple way
- New ones:

**CALLDATALOAD** : Get input data of current environment, but do you have be careful with the offset



## Solution 

Let's break this puzzle:

1. Well, only the new term is CALLDATALOAD, you can pass a value for CALLDATA , this value is **JUMPDEST** that is equal `A`.
2. But,like I said, if you push in the calldata only the value `0x0A` the output is `a00000000000000000000000000000000000000000000000000000000000000 `
3. So, for this reason the answer is `0x000000000000000000000000000000000000000000000000000000000000000a`
 

easy right?  Good job, go to the next level !!!.



- Remember:

**CALLVALUE** : the value we are sent **CALLVALUE** push in the stack.

**CALLDATASIZE** : Get size of input data in current environment. For example: you send some value for calldata and this opcode take the size.

**JUMP**:  It is take in the stack its input, it use for jump looking **JUMPDEST** for continue.

**JUMPI**: this is a little different from **JUMP**, this take 2 argument one is for **JUMPDEST** step and the second one is a conditional if the value is different to 0 is **JUMP** to **JUMPDEST** if the value is 0 it will continue to the next instruction.

**JUMPDEST**:Its just a valid destination for **JUMP**(our case) or **JUMPI**.

**REVERT** (GAME OVER): Stop the current context execution. So, you need avoid.

**STOP** ( WE DIT IT): Exit the current context successfully. 

**CODESIZE** : Each instruction occupies one byte, **CODESIZE** return the size value. for example:
        When we run our code  **CODESIZE** return `a`. But, why? 
        Well, the bytecode is 34380356FDFD5B00FDFD if each instruction ocupies one byte, 34 38 03 56 FD FD 5B 00 FD FD = 10 bytes = `a` in hexadecimal.
**SUB**:  Its take 2 values from the stack and do a subtraction operation

**MUL** :Take two input from stack and do multiplication operation.

**EQ**: Equality comparison. If the result is true their result is 1 otherwise 0.

**XOR** : Bitwise XOR operation. e.g. 

**DUP1**: This is easy, just duplicate 1st stack item. 

**PUSH1**: 	Place 1 byte item on stack, has its argmument.

**PUSH2**: Place 2 byte item on stack, has its argmument.