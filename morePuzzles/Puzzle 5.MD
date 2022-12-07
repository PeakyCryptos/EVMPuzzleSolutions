```
00      6020      PUSH1 20
02      36        CALLDATASIZE
03      11        GT
04      6008      PUSH1 08
06      57        JUMPI
07      FD        REVERT
08      5B        JUMPDEST
09      36        CALLDATASIZE
0A      6000      PUSH1 00
0C      6000      PUSH1 00
0E      37        CALLDATACOPY
0F      36        CALLDATASIZE
10      59        MSIZE
11      03        SUB
12      6003      PUSH1 03
14      14        EQ
15      6019      PUSH1 19
17      57        JUMPI
18      FD        REVERT
19      5B        JUMPDEST
1A      00        STOP
```


CALLDATASIZE must be greater than 20(32 in decimal) to pass first JUMPI.


CALLDATACOPY loads into memory the call data you pass in.
To pass second JUMPI -> MSIZE - CALLDATASIZE must equal 3

MSIZE returns the value in hex of the amount of byte slots written to
i.e a 32 byte value has an msize of (32 -> 20 in hex),
i.e a 1 byte value has an msize of 20 as well as it writes to one whole 32 byte slot

Thus we need a CALLDATASIZE that is 3 less than a multiple of 32.
As whenever you write to memory you write in 32 byte multiples even if you
don't write to the entire slot.

thus, we could pass in a CALLDATASIZE of 64 - 3 = 61.
MSIZE - CALLDATASIZE would then be (64 - 61 = 3)

Why we can't use a CALLDATASIZE of 32 - 3 is because 32 in hex is 20 and we must
have a CALLDATASIZE greater than 20 to pass the first JUMPI check. 