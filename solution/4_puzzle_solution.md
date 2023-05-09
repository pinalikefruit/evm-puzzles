# Puzzle 4 

| Step| OPCODE| NAME|
|-----|-----|-----|
|00  | 34   | CALLVALUE
|01  | 38   | CODESIZE
|02  | 18   | XOR
|03  | 56   | JUMP
|04  | FD   | REVERT
|05  | FD   | REVERT
|06  | FD   | REVERT
|07  | FD   | REVERT
|08  | FD   | REVERT
|09  | FD   | REVERT
|0A  | 5B   | JUMPDEST
|0B  | 00   | STOP


Run Bytecode: [34381856FDFDFDFDFDFD5B00](https://www.evm.codes/playground?fork=merge&unit=Wei&callData=0x00000000&codeType=Bytecode&code='34381856~~~5B00'~FDFD%01~_)


### Opcodes: explained in a simple way
- New ones:

**XOR** : Bitwise XOR operation. e.g. 

| Input A| Input B| XOR Output|
|-----|-----|-----|
|0  | 0  | 0
|0  | 1   | 1
|1  | 0   | 1
|1  | 1   | 0


## Solution 

Let's break this puzzle:

1. Well, the metodologies is the same, we need pass some value to **JUMP** and arrive in **JUMPDEST** but first we must move on the  **XOR** operation.
2. **CALLVALUE** = X this is the value we need to found, **CODESIZE** already have = 34381856FDFDFDFDFDFD5B00 = 34 38 18 56 FD FD FD FD FD FD 5B 00 = 12 bytes. Finally, we know the result have to be 0xA  for the **JUMPDEST** 
3. We take this operation to binaries 


<table>
  <tr>
    <th>X</th>
    <td>-</td>
    <td>-</td>
    <td>-</td>
    <td>-</td>
  </tr>
  <tr>
    <th>CODESIZE</th>
    <td>1</td>
    <td>1</td>
    <td>0</td>
    <td>0</td>
  </tr>
  <tr>
    <th>RESULT</th>
    <td>1</td>
    <td>0</td>
    <td>1</td>
    <td>0</td>
  </tr>
</table>

For this to be fulfilled X have to be:
<table>
  <tr>
    <th>X</th>
    <td>0</td>
    <td>1</td>
    <td>1</td>
    <td>0</td>
  </tr>
  <tr>
    <th>CODESIZE</th>
    <td>1</td>
    <td>1</td>
    <td>0</td>
    <td>0</td>
  </tr>
  <tr>
    <th>RESULT</th>
    <td>1</td>
    <td>0</td>
    <td>1</td>
    <td>0</td>
  </tr>
</table>

4. The X result is 0110 that is equal in decimal 6. In thats is our key!!

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


