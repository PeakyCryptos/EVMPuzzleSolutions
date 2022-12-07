00      36      CALLDATASIZE
01      56      JUMP
02      FD      REVERT
03      FD      REVERT
04      5B      JUMPDEST
05      00      STOP

In this puzzle we can see we want a JUMP which hits a JUMPDEST located at 04.
The only value we are passing in is call data. The value that is read into the stack
is the CALLDATASIZE. CALLDATA is passed in as hex, and each 2 digits represents one byte.
Thus, we can pass in 00000000 (4 bytes) 

Instructions and stack after instruction has been executed

00      36      CALLDATASIZE -> [4]
01      56      JUMP -> []
04      5B      JUMPDEST -> []
05      00      STOP