
00      6000      PUSH1 00
02      35        CALLDATALOAD
03      56        JUMP
04      FD        REVERT
05      FD        REVERT
06      FD        REVERT
07      FD        REVERT
08      FD        REVERT
09      FD        REVERT
0A      5B        JUMPDEST
0B      00        STOP

CALLDATALOAD

Stack input

i: byte offset in the calldata.

Stack output

data[i]: 32-byte value starting from the given offset of the calldata. 
All bytes after the end of the calldata are set to 0.

Since the first line is pushing 00, we will be loading the full 32 byte value (64 digits)
onto the stack.

000000000000000000000000000000000000000000000000000000000000000a

Instruction's and stack after instruction is ran

00      6000      PUSH1 00 -> [0]
02      35        CALLDATALOAD -> [000000000000000000000000000000000000000000000000000000000000000a]
03      56        JUMP -> []
0A      5B        JUMPDEST -> []
0B      00        STOP