```
00      36        CALLDATASIZE
01      6000      PUSH1 00
03      80        DUP1
04      37        CALLDATACOPY
05      36        CALLDATASIZE
06      6000      PUSH1 00
08      6000      PUSH1 00
0A      F0        CREATE
0B      6000      PUSH1 00
0D      80        DUP1
0E      80        DUP1
0F      80        DUP1
10      80        DUP1
11      94        SWAP5
12      5A        GAS
13      F1        CALL
14      6000      PUSH1 00
16      14        EQ
17      601B      PUSH1 1B
19      57        JUMPI
1A      FD        REVERT
1B      5B        JUMPDEST
1C      00        STOP
```

We can see here that JUMPI has a 1B that is pushed to the stack before it which corresponds 
to the location of JUMPDEST. The value porition that signifies whether the jump should occur
or not, has an equality comparison before it which will check if the previous 2 items pushed
to the stack are equivalent.

In this case those 2 items are a 0 (which was pushed via a PUSH1) and the outcome
of the CALL opcode.  

CALL
Stack input
gas: amount of gas to send to the sub context to execute. The gas that is not used by the sub context is returned to this one.
address: the account which context to execute.
value: value in wei to send to the account.
argsOffset: byte offset in the memory in bytes, the calldata of the sub context.
argsSize: byte size to copy (size of the calldata).
retOffset: byte offset in the memory in bytes, where to store the return data of the sub context.
retSize: byte size to copy (size of the return data).

The input into the call opcode will look as follows: 
[GAS, ADDRESS (returned from CREATE step), 0, 0, 0, 0, 0]

Before GAS was pushed onto the stack there was a SWAP5 operation. Which exchanged the most
recent item on the stack with the 6th item. There are 5 zero's present in the stack because
of the 0 pushed onto the stack that was duplicated 4 times.

The CALL operation "Creates a new sub context and execute the code of the 
given account, then resumes the current one. Note that an account with no code will return 
success as true." Our goal is to create a contract that when called reverts.

Why this is the case is because as we mentioned earlier an equivalency check must be passed.
The number pushed to the stack to compare equivalency against is 0.

Deployment code for a contract that returns a revert:
```
PUSH1 FD
PUSH1 00
MSTORE8
PUSH1 01
PUSH1 00
RETURN
 
60FD60005360016000F3
```

To pass this puzzle we send this byte code in as call data. As the CALLDATACOPY operation
copies this code into memory and also uses the size of whatever bytecode you define to
prime the parameters for the CREATE instruction. 

Instruction's and stack after instruction is ran:
```
00      36        CALLDATASIZE -> [a]
01      6000      PUSH1 00 -> [0, a]
03      80        DUP1 -> [0, 0, a]
04      37        CALLDATACOPY -> []
05      36        CALLDATASIZE -> [a]
06      6000      PUSH1 00 -> [0, a]
08      6000      PUSH1 00 -> [0, 0, a]
0A      F0        CREATE -> [ADDRESS]
0B      6000      PUSH1 00 -> [0, ADDRESS]
0D      80        DUP1 -> [0, 0, ADDRESS]
0E      80        DUP1 -> [0, 0, 0, ADDRESS]
0F      80        DUP1 -> [0, 0, 0, ADDRESS]
10      80        DUP1 -> [0, 0, 0, 0, ADDRESS]
11      94        SWAP5 -> [ADDRESS, 0, 0, 0, 0]
12      5A        GAS -> [GAS, ADDRESS, 0, 0, 0, 0]
13      F1        CALL -> [0]
14      6000      PUSH1 00 -> [0, 0]
16      14        EQ -> [1]
17      601B      PUSH1 1B -> [1B, 1]
19      57        JUMPI -> []
1B      5B        JUMPDEST -> []
1C      00        STOP
```