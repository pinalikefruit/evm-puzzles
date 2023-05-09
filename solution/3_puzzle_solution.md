# Puzzle 3 

| Step| OPCODE| NAME|
|-----|-----|-----|
| 00 | 36   | CALLDATASIZE
|01  | 56   | JUMP
|02  | FD   | REVERT
|03  | FD   | REVERT
|04  | 5B   | JUMPDEST
|05  | 00   | STOP


Run Bytecode: [3656FDFD5B00](https://www.evm.codes/playground?fork=merge&unit=Wei&codeType=Bytecode&code='3656FDFD5B00'_)

### Opcodes: explained in a simple way
- New ones:

**CALLDATASIZE** : Get size of input data in current environment. For example: you send some value for calldata and this opcode take the size.




## Solution 

Let's break this puzzle:

1. If we continue as before solve this challenge, we need pass some value to **JUMP**  for arrive in **JUMPDEST** this values is 4. 
2. So, we need pass for calldata some value that **CALDATASIZE** result equal to 4. Ex: 0x00000000 = 00 00 00 00 = 4 bytes


You get it ? Yes ? Good job, go to the next level !!!.



- Remember:

**CALLVALUE** : the value we are sent **CALLVALUE** push in the stack.

**JUMP**:  It is take in the stack its input, it use for jump looking **JUMPDEST** for continue.

**REVERT** (GAME OVER): Stop the current context execution. So, you need avoid.

**JUMPDEST**:Its just a valid destination for **JUMP**(our case) or **JUMPI**.

**STOP** ( WE DIT IT): Exit the current context successfully. 

**CODESIZE** : Each instruction occupies one byte, **CODESIZE** return the size value. for example:
        When we run our code  **CODESIZE** return `a`. But, why? 
        Well, the bytecode is 34380356FDFD5B00FDFD if each instruction ocupies one byte, 34 38 03 56 FD FD 5B 00 FD FD = 10 bytes = `a` in hexadecimal.
**SUB**:  Its take 2 values from the stack and do a subtraction operation