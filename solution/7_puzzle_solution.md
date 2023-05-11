# Puzzle 7

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
|0B  | 3B   | EXTCODESIZE
|0C  | 6001   | PUSH1 01
|0E  | 14   | EQ
|0F  | 6013   | PUSH1 13
|11  | 57   | JUMPI
|12  | FD   | REVERT
|13  | 5B   | JUMPDEST
|14  | 00   | STOP



Run Bytecode: [36600080373660006000F03B600114601357FD5B00](https://www.evm.codes/playground?fork=merge&unit=Wei&codeType=Bytecode&code='36~0803736~0~0F03B~114601357FD5B00'~600%01~_)


### Opcodes: explained in a simple way
- New ones:

**CALLDATACOPY** : Copy input data in current environment to memory, easy right? but them have 3 inputs :
    - destOffset: byte offset in the memory where the result will be copied.
    - offset: byte offset in the calldata to copy.
    - size: byte size to copy.


**CREATE** : Create a new account with associated code, also have three inputs:
    - value: value in wei to send to the new account.
    - offset: byte offset in the memory in bytes, the initialisation code for the new account.
    - size: byte size to copy (size of the initialisation code).

**EXTCODESIZE** : Get size of an account’s code. *note:retrieves the size of the code of the newly created contract. However, at this point, the contract has not yet been deployed, so the result of this operation is 0.*




## Solution 

Let's break this puzzle:



1. We need in the position 11 for **JUMPI** have two values: 13 and a number different to 0, right? well, we have the 13 but for the previus step 0F with **PUSH1 13** and we have the same conditional **EQ** like the challenge before. We need this operation is equal to one.

2. In the step 0C we have the **PUSH1 01** so we need the result in **EXTCODESIZE** sea equal 1, this is no too easy like challenge before. 

3. What is the new here? 
- We make the contract succesffuly deploy via **CREATE** if not the result is 0. 
- Then, the result is an address that have been return 1 when **EXTCODESIZE** is executed.

 For this reason, you need create a runtime code has only 1 instruction for that return 1 byte. 

 ```
 PUSH1 00 // 00 is the opcode for STOP
PUSH1 00 // this will be used as the offset of MSTORE8 that store 1 byte in memory
MSTORE8 // will store in memory from offset 0 the `00` value (from the first PUSH1)

PUSH1 01 // how many bytes must be returned
PUSH1 00 // from which memory offset return those bytes
RETURN
```



Solution [600060005360016000F3](https://www.evm.codes/playground?fork=merge&unit=Wei&codeType=Mnemonic&code='Q00%20isJopcodzfor%20STOPgQthisLill_zused%20asJkof%20WqatZ1_ytzinjgW~willZinj%20XI0J%6000%60%20valuz%7BXJfirsKV%7Dggx1~howYany_ytesYust_zreturnedgQXLhichjIreturnqoszbytesgRETURN'~%20%2F%2F%20ze%20xV%200q%20thkoffseKjYemoryg%5Cn_%20bZ%20storzY%20mXfromWMSTORE8VPUSH1Qx0~L%20wKt%20JqzI%20k%01IJKLQVWXYZ_gjkqxz~_) and you convert to bytecode.

Here you can find a better solution by @StErMi [here](https://stermi.xyz/blog/evm-puzzle-7-solution)

I know it, this was a bit  more complex, but don't worry, just keep going and going !!!.

- Remember:

**CALLVALUE** : the value we are sent **CALLVALUE** push in the stack.

**CALLDATALOAD** : Get input data of current environment, but do you have be careful with the offset

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