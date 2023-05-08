# Puzzle 1 

| Step| OPCODE| NAME|
|-----|-----|-----|
| 00 |    34|      CALLVALUE|
|01  |    56 |     JUMP|
|02  |    FD |     REVERT
|03  |FD   |   REVERT
|04  | FD|      REVERT
|05    |  FD |     REVERT
|06   |   FD  |    REVERT
|07  |    FD   |   REVERT
|08 |     5B    |  JUMPDEST
|09|      00     | STOP

Run Bytecode: [3456FDFDFDFDFDFD5B00](https://www.evm.codes/playground?fork=merge&callValue=8&unit=Wei&codeType=Bytecode&code='3456~~~5B00'~FDFD%01~_)

### Opcodes: explained in a simple way

**CALLVALUE** : the value we are sent **CALLVALUE** push in the stack.

**JUMP**:  It is take in the stack its input, it use for jump looking **JUMPDEST** for continue.

**REVERT** (GAME OVER): Stop the current context execution. So, you need avoid.

**JUMPDEST**:Its just a valid destination for **JUMP**(our case) or **JUMPI**.

**STOP** ( WE DIT IT): Exit the current context successfully. 


## Solution 

If we undestand well, we start in the step 0 pass a value and wait for arrive to **STOP**  for get success. So, let's do it!

1. Like I say before, **CALLVALUE** push the value we will send in stack. But, what value is correct ? Its the value what are looking for.
2. Then, we see the instruction **JUMP** their opcode is looking for **JUMPDEST** for a safe arrive. Its very help for us, because we need avoid pass for **REVERT** instruction. 
3. **JUMP** use the value in their stack for arrive to **JUMPDEST**, so, if we pass a value for **CALLDATA** is a value use for **JUMPDEST** and we can found **JUMPDEST** in the slop 8. 
4. Here the answer, if we pass the value 8 we solven this puzzle, yeey /o/.


Go to the next level !!!.