# Puzzle 10

| Step| OPCODE| NAME|
|-----|-----|-----|
|00  | 38  | CODESIZE
|01  | 34 | CALLVALUE
|02  | 90   | SWAP1
|03  | 11 | GT
|04  | 6008   | PUSH1 08
|06  | 57   | JUMPI
|07  | FD   | REVERT
|08  | 5B   | JUMPDEST
|09  | 36   | CALLDATASIZE
|0A  | 610003   | PUSH2 0003
|0D  | 90   | SWAP1
|0E  | 06   | MOD
|0F  | 15   | ISZERO
|10  | 34   | CALLVALUE
|11  | 600A   | PUSH1 0A
|13  | 01   | ADD 
|14  | 57   | JUMPI
|15  | FD   | REVERT
|16  | FD   | REVERT
|17  | FD   | REVERT
|18  | FD   | REVERT
|19  | 5B   | JUMPDEST
|1A  | 00   | STOP


Run Bytecode: [38349011600857FD5B3661000390061534600A0157FDFDFDFD5B00](https://www.evm.codes/playground?fork=merge&unit=Wei&codeType=Bytecode&code='38349011600857FD5B3661000390061534600A0157FDFDFDFD5B00'_)


### Opcodes: explained in a simple way
- New ones:

**GT**: Greater-than comparison .Input [a.b]. output = a > b

**MOD**: Modulo remainder operation. a % b

**ISZERO**: a == 0

**ADD** : a + b


## Solution 

1. The value of STEP[00,08] **CODESIZE** is equal to 1b, which is 27 bytes. The first requirement is that **CODESIZE** must be greater than **CALLVALUE**. Since **CODESIZE** is 27, we know that **CALLVALUE** must be less than 27.
2. In the next step (STEP[09,0F]), we need to pass a **CALLDATA** whose size is such that **CALLDATASIZE** % 3 == 0. If no **CALLDATA** is passed, **CALLDATASIZE** will be 0, and this requirement is automatically satisfied.
3. In the final step (STEP[10,1A]), we need to pass a **JUMPI** instruction to the location of **JUMPDEST**. The location of **JUMPDEST** is 19, which we can represent as the hexadecimal value "13" in the bytecode. We also need a **CALLVALUE** + A = 19, where "A" represents the value 10 in hexadecimal. Converting 19 and 10 from hexadecimal to decimal, we get 25 and 10 respectively. Therefore, the value of **CALLVALUE** must be 15, which is less than the **CODESIZE** of 27.



Well done!!  All puzzle are SOLVED!!

### Remember:

**CALLVALUE** : the value we are sent **CALLVALUE** push in the stack.

**CALLDATALOAD** : Get input data of current environment, but do you have be careful with the offset

**CALLDATASIZE** : Get size of input data in current environment. For example: you send some value for calldata and this opcode take the size.

**CALLDATACOPY**: Copy input data in current environment to memory, easy right? but them have 3 inputs :
    - destOffset: byte offset in the memory where the result will be copied.
    - offset: byte offset in the calldata to copy.
    - size: byte size to copy.

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

**LT**: Less-than comparison. Input [a.b]. output = a < b.

**XOR** : Bitwise XOR operation. e.g. 

**DUP1**: This is easy, just duplicate 1st stack item. 

**SWAP5** : Exchange 1st and 6th stack items.


**PUSH1**: 	Place 1 byte item on stack, has its argmument.

**PUSH2**: Place 2 byte item on stack, has its argmument.

**CREATE**: Create a new account with associated code, also have three inputs:
    - value: value in wei to send to the new account.
    - offset: byte offset in the memory in bytes, the initialisation code for the new account.
    - size: byte size to copy (size of the initialisation code).

**EXTCODESIZE**: Get size of an account’s code. *note:retrieves the size of the code of the newly created contract. However, at this point, the contract has not yet been deployed, so the result of this operation is 0.*

**GAS** : remaining gas (after this instruction).

**CALL** : Message-call into an account
    - Stack input
gas: amount of gas to send to the sub context to execute. The gas that is not used by the sub context is returned to this one.
address: the account which context to execute.
value: value in wei to send to the account.
argsOffset: byte offset in the memory in bytes, the calldata of the sub context.
argsSize: byte size to copy (size of the calldata).
retOffset: byte offset in the memory in bytes, where to store the return data of the sub context.
retSize: byte size to copy (size of the return data).
   - Stack output
success: return 0 if the sub context reverted, 1 otherwise.