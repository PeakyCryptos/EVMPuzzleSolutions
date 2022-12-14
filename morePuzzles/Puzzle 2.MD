```
00      36        CALLDATASIZE
01      6000      PUSH1 00
03      6000      PUSH1 00
05      37        CALLDATACOPY
06      36        CALLDATASIZE
07      6000      PUSH1 00
09      6000      PUSH1 00
0B      F0        CREATE
0C      6000      PUSH1 00
0E      80        DUP1
0F      80        DUP1
10      80        DUP1
11      80        DUP1
12      94        SWAP5
13      5A        GAS
14      F1        CALL
15      3D        RETURNDATASIZE
16      600A      PUSH1 0A
18      14        EQ
19      601F      PUSH1 1F
1B      57        JUMPI
1C      FE        INVALID
1D      FE        INVALID
1E      FE        INVALID
1F      5B        JUMPDEST
20      00        STOP
```

New instruction encountered is RETURNDATASIZE which pushes to stack, the size of return data
of last execution context from a CALL, CALLCODE, DELEGATECALL or STATICCALL.

In this case we create a contract which returns data of 10 byte size when called.

```
[00]	PUSH1	10
[02]	PUSH1	0C
[04]	PUSH1	00
[06]	CODECOPY	
[07]	PUSH1	10
[09]	PUSH1	00
[0b]	RETURN	
[0c]	PUSH10	00000000000000000000
[17]	PUSH1	0A
[19]	PUSH1	00
[1b]	RETURN	

6010600C60003960106000F36900000000000000000000600A6000F3
```

This contract above will return the data of 00000000000000000000600, which is 10 bytes.