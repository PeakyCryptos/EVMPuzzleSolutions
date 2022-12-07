00      36        CALLDATASIZE
01      6000      PUSH1 00
03      80        DUP1
04      37        CALLDATACOPY
05      36        CALLDATASIZE
06      6000      PUSH1 00
08      6000      PUSH1 00
0A      F0        CREATE
0B      3B        EXTCODESIZE
0C      6001      PUSH1 01
0E      14        EQ
0F      6013      PUSH1 13
11      57        JUMPI
12      FD        REVERT
13      5B        JUMPDEST
14      00        STOP

In this puzzle we can see the JUMPI is primed with the location of 13 which points to the 
JUMPDEST that will allow this program to exit cleanly. The value portion of the JUMPI is 
dependent upon the outcome of an EQ (equality comparison) with the value 1. If the EXTCODESIZE
(byte size of the code) returned from the CREATE (creates a contract) is 1, the value porition
of JUMPI will be 1.

CREATE takes 3 parameters: 
value: value in wei to send to the new account.
offset: byte offset in the memory in bytes, the initialisation code for the new account.
size: byte size to copy (size of the initialisation code).

In order of the stack. Value passed in will be 0 wei, and byte offset in memory is 0.
As they are the previous top 2 items pushed onto the stack. Finally, the byte size to copy
from the previously specified position in memory is dependent on the CALLDATASIZE you pass
in. 

This means create will deploy a contract passing in a value of 0 wei, and the byte offset in memory
to start reading from of 0. It will also load in the size of whatever constructor code you define.

CALLDATACOPY takes in 3 arguments 
Stack input
destOffset: byte offset in the memory where the result will be copied.
offset: byte offset in the calldata to copy.
size: byte size to copy.   


Because of the previous elements that have been pushed to the stack. We will get 
0 for the location in memory where the result will begin being copied, and a 0 for the location
in call data where to begin copying. The size again will be the entire bytecode size you passed
in for the call data.

To construct deployment code that returns (appends to the blockchain) a contract 
with bytecode size of 1:

In the deployment from CREATE we must use the RETURN opcode to pull from memory what the
code that would be appended to the blockchain as the executable run time code will be. 

Return takes 2 stack inputs:
offset: byte offset in the memory in bytes, to copy what will be the return data of this context.
size: byte size to copy (size of the return data).
Example

Choose a random opCode that doesn't alter the flow of exection i.e 3A (pushes GASPRICE onto the 
stack). Next we must load this opcode into memory so the return can access it. We can do this
with the CODECOPY opcode. 

CODECOPY
Stack input
destOffset: byte offset in the memory where the result will be copied.
offset: byte offset in the code to copy.
size: byte size to copy.

Since we are putting GASPRICE as the first byte in the new contract we must load it
from byte offset 0. The resulting parameters will look like so:
destOffset: 0, offset: 0, size: 1 (as we have 1 instruction)

Next we must also push to the stack the neccessary inputs for return. 0 and 1 for the
offset in memory to return (execution runtime code) and the byte size to copy 1.

This will give us the instruction set of:
3A
PUSH 01
PUSH 00
PUSH 00
CODECOPY
PUSH 01
PUSH 00
RETURN

3A6001600060003960016000F3      

Instruction's and stack after instruction is ran
00      36        CALLDATASIZE -> [d]
01      6000      PUSH1 00 -> [0, d]
03      80        DUP1 -> [0, 0, d]
04      37        CALLDATACOPY -> []
05      36        CALLDATASIZE -> [d]
06      6000      PUSH1 00 -> [0, d]
08      6000      PUSH1 00 -> [0, 0, d]
0A      F0        CREATE -> [contractAddress]
0B      3B        EXTCODESIZE -> [1]
0C      6001      PUSH1 01 -> [1, 1]
0E      14        EQ -> [1]
0F      6013      PUSH1 13 -> [13, 1]
11      57        JUMPI -> []
13      5B        JUMPDEST -> []
14      00        STOP