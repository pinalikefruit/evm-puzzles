# Puzzle 9

| Step| OPCODE| NAME|
|-----|-----|-----|
|00  | 36   | CALLDATASIZE
|01  | 6003 | PUSH1 03
|03  | 10   | LT
|04  | 6009 | PUSH1 09
|06  | 57   | JUMPI
|07  | FD   | REVERT
|08  | FD   | REVERT
|09  | 5B   | JUMPDEST
|0A  | 34   | CALLVALUE
|0B  | 36   | CALLDATASIZE
|0C  | 02   | MUL
|0D  | 6008   | PUSH1 08
|0F  | 14   | EQ
|10  | 6014   | PUSH1 14
|12  | 57   | JUMPI
|13  | FD   | REVERT 
|14  | 5B   | JUMPDEST
|15  | 00   | STOP


Run Bytecode: [36600310600957FDFD5B343602600814601457FD5B00](https://www.evm.codes/playground?fork=merge&unit=Wei&codeType=Bytecode&code='36600310600957FDFD5B343602600814601457FD5B00'_)


### Opcodes: explained in a simple way
- New ones:

**LT**: Less-than comparison. Input [a.b]. output = a < b.


## Solution 

Let's break this puzzle:

Intro: in this case we just have a new opcodes and I'm sure you kwon how works. The different here is we need pass 2 argument this time, one for value and one calldata, let's do it.

1. [from:00 ; to: 09]: We need pass a calldata that its size is higher to 03. Easy ,right?  for now whatever calldata size higher 3bytes works.
2. [from:09; to:15]: This is the operation :
    **CALLVALUE** * **CALLDATASIZE**  == 8  ==> Remember that **CALLDATASIZE**  has 3 majors, so a minimum of 4 and if it is 4 to fulfill the sentence, the call value is 2.

    Answer: **CALLVALUE** = 2  **CALLDATA** = 0x00000000



You're an expert , just keep going and going !!!.

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