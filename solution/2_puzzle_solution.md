# Puzzle 1 

| Step| OPCODE| NAME|
|-----|-----|-----|
| 00 | 34   | CALLVALUE
|01  | 38   | CODESIZE    
|02  | 03   | SUB
|03  | 56   | JUMP
|04  | FD   | REVERT
|05  | FD   | REVERT
|06  | 5B   | JUMPDEST
|07  | 00   | STOP
|08  | FD   | REVERT
|09  | FD   | REVERT

Run Bytecode: [34380356FDFD5B00FDFD](https://www.evm.codes/playground?fork=merge&callValue=8&unit=Wei&codeType=Bytecode&code='34380356FDFD5B00FDFD%5Cn'_)

### Opcodes: explained in a simple way
- New ones:

**CODESIZE** : Each instruction occupies one byte, **CODESIZE** return the size value. for example:
        When we run our code  **CODESIZE** return `a`. But, why? 
        Well, the bytecode is 34380356FDFD5B00FDFD if each instruction ocupies one byte, 34 38 03 56 FD FD 5B 00 FD FD = 10 bytes = `a` in hexadecimal.
**SUB**:  Its take 2 values from the stack and do a subtraction operation

- Remember:

**CALLVALUE** : the value we are sent **CALLVALUE** push in the stack.

**JUMP**:  It is take in the stack its input, it use for jump looking **JUMPDEST** for continue.

**REVERT** (GAME OVER): Stop the current context execution. So, you need avoid.

**JUMPDEST**:Its just a valid destination for **JUMP**(our case) or **JUMPI**.

**STOP** ( WE DIT IT): Exit the current context successfully. 


## Solution 

Let's break this puzzle:

1. Its a little similar like we did it before, we need found a value that with subtraction operation with *CODESIZE* is equal to the JUMPDEST, right ?
2. The result is 6 slot for the JUMPDEST, we have the CODESIZE is `a` well 6 = a - b => b = a - 6 = 4
3. Our value is 4. That is the answer.


You get it ? Yes ? Good job, go to the next level !!!.