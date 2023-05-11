# Puzzle 8

| Step| OPCODE| NAME|
|-----|-----|-----|
|00  | 36   | CALLDATASIZE
|01  | 6000   | PUSH1 00
|03  | 80   | DUP1
|04  | 37   | CALLDATACOPY
|05  | 36   | CALLDATASIZE
|06  | 6000   | PUSH1 00
|08  | 6000   | PUSH1 00
|0A  | F0   | CREATE
|0B  | 6000   | PUSH1 00
|0D  | 80   | DUP1
|0E  | 80   | DUP1
|0F  | 80   | DUP1
|10  | 80   | DUP1
|11  | 94   | SWAP5
|12  | 5A   | GAS
|13  | F1   | CALL
|14  | 6000   | PUSH1 00
|16  | 14   | EQ
|17  | 601B   | PUSH1 1B
|19  | 57   | JUMPI
|1A  | FD   | REVERT
|1B  | 5B   | JUMPDEST
|1C  | 00   | STOP




Run Bytecode: [36600080373660006000F03B600114601357FD5B00](https://www.evm.codes/playground?fork=merge&unit=Wei&codeType=Bytecode&code='36~803736~~F0~80808080945AF1~14601B57FD5B00'~6000%01~_)


### Opcodes: explained in a simple way
- New ones:

**SWAP5** : Exchange 1st and 6th stack items.

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



## Solution 

Let's break this puzzle:


1. We need to pass a **JUMPI** two values `1B` for destination and 1 for conditional, en the step `17` we have `1B`. So, it is like the challenge before, we need pass the **EQ** sucessfully. But the different is the comparation is with `0`. 
2.  We need call return `0`.How? We make if the sub contet reverted. 
3. How we can revert the call ?

```
// SPDX-License-Identifier: GPL-3.0

pragma solidity 0.8.0;

contract noName {

   fallback() external  payable {
       revert();
   }
}

```
When any transaction doesn't mach with any function, call the `fallback` it will be rolled back automatically.
If you translate to bytecode `6080604052348015600f57600080fd5b50603f80601d6000396000f3fe6080604052600080fdfea264697066735822122001afea1f193f9d37652ce64a
85c799012297d5c94c0d41810d84d47378c431c064736f6c63430008000033`
This value can be passed to calldata. 




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

**PUSH1**: 	Place 1 byte item on stack, has its argmument.

**PUSH2**: Place 2 byte item on stack, has its argmument.

**CREATE**: Create a new account with associated code, also have three inputs:
    - value: value in wei to send to the new account.
    - offset: byte offset in the memory in bytes, the initialisation code for the new account.
    - size: byte size to copy (size of the initialisation code).

**EXTCODESIZE**: Get size of an account’s code. *note:retrieves the size of the code of the newly created contract. However, at this point, the contract has not yet been deployed, so the result of this operation is 0.*

