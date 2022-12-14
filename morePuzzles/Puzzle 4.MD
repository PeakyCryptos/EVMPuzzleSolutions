```
00      30        ADDRESS
01      31        BALANCE
02      36        CALLDATASIZE
03      6000      PUSH1 00
05      6000      PUSH1 00
07      37        CALLDATACOPY
08      36        CALLDATASIZE
09      6000      PUSH1 00
0B      30        ADDRESS
0C      31        BALANCE
0D      F0        CREATE
0E      31        BALANCE
0F      90        SWAP1
10      04        DIV
11      6002      PUSH1 02
13      14        EQ
14      6018      PUSH1 18
16      57        JUMPI
17      FD        REVERT
18      5B        JUMPDEST
19      00        STOP
```

This contract balance can be 2 and the create contract should be 1
As when the DIV operation is done it divides Balance of this / balance of create

construct contract that takes 2 wei and keeps 1.
A valid way I see to do this is to create contract bytecode to pass in that 
will create another contract that passes 1 wei as value (leaving initial created contract
with 1 wei).

CREATE:
Stack input
value: value in wei to send to the new account.
offset: byte offset in the memory in bytes, the initialisation code for the new account.
size: byte size to copy (size of the initialisation code).

contract that creates a contract
passes 2 wei to first contract, then 1 wei to the second contract

second contract code: 60016000F3 -> When called just halts execution
```
[00]	PUSH1	01
[02]	PUSH1	00
[04]	RETURN
```
--------------------------------------
When called halts execution.
Deploys another contract in initalization phase, passing in 1 wei value.

first contract code: 6460016000F36000526005601B6001F0303160016000F3 
```
[00]	PUSH5	60016000F3
[06]	PUSH1	00
[08]	MSTORE	
[09]	PUSH1	05
[0b]	PUSH1	1B
[0d]	PUSH1	01
[0f]	CREATE	
[10]	ADDRESS	
[11]	BALANCE	
[12]	PUSH1	01
[14]	PUSH1	00
[16]	RETURN	
```