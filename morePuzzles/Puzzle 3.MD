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
11      93        SWAP4
12      5A        GAS
13      F4        DELEGATECALL
14      6005      PUSH1 05
16      54        SLOAD
17      60AA      PUSH1 AA
19      14        EQ
1A      601E      PUSH1 1E
1C      57        JUMPI
1D      FE        INVALID
1E      5B        JUMPDEST
1F      00        STOP
```

New instructions seen are DELEGATECALL and SLOAD.

DELEGATECALL will call an address you pass in and run that code as if it was
ran from the base contract itself (in the calling contract).

SLOAD will load a 32 byte value from the specified storage slot.

To solve this we must craft a contract that when called pushes the value AA to storage slot
5. As the JUMPI value portion is the outcome of an equality comparison between the value AA
and the value in storage slot 5.

We must make use of the operation SSTORE which allows us to specifically write 
a value to a specified storage slot [key, value].

```
[00]	PUSH1	AA
[02]	PUSH1	00
[04]	SSTORE

60AA600555
```

Define constructor that will deploy this contract:
6005600C60003960056000F360AA600555

```
[00]	PUSH1	05
[02]	PUSH1	0C
[04]	PUSH1	00
[06]	CODECOPY	
[07]	PUSH1	05
[09]	PUSH1	00
[0b]	RETURN	
[0c]	PUSH1	AA
[0e]	PUSH1	05
[10]	SSTORE
```