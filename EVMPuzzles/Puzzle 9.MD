```
00      36        CALLDATASIZE
01      6003      PUSH1 03
03      10        LT
04      6009      PUSH1 09
06      57        JUMPI
07      FD        REVERT
08      FD        REVERT
09      5B        JUMPDEST
0A      34        CALLVALUE
0B      36        CALLDATASIZE
0C      02        MUL
0D      6008      PUSH1 08
0F      14        EQ
10      6014      PUSH1 14
12      57        JUMPI
13      FD        REVERT
14      5B        JUMPDEST
15      00        STOP
```

In this puzzle we have 2 JUMPI instruction we must traverse to halt execution cleanly.

Starting with the second JUMPI who's value porition is 
determined by the outcome of an equality operation. In this equality operation we are 
checking to see if there is a number equivalent to the value 8.

The number that is compared against that 8 is produced via the outcome of a CALLVALUE
and CALLDATASIZE operation. CALLVALUE pushes to the stack the decimal amount of the wei
pushed in as input. CALLDATASIZE is the byte size of the call data passed in.

To get to this portion of the instruction set we must have arrived here from the first
JUMPI instruction. For this instruction if we wanted the value porition to be 1, we must
push in a CALLDATASIZE which is greater than 3.

As the LT operator (less than) takes two items off the stack [a, b] and evaluates them in
this manner: a < b. Returns 0 if false, else 1.

To solve this puzzle we know now that we need to pass in a call data that is both
greater than 3, and a callvalue which when multiplied against the call data size returns an 8.
Meaning we could pass in a CALLDATASIZE of 4, and a CALLVALUE of 2.

Since call data is represented as hex and every 2 digits represents 1 byte we can pass in
a call data of: 0x00000000

CALLVALUE: 2     

Instruction's and stack after instruction is ran:
```
00      36        CALLDATASIZE -> [4]
01      6003      PUSH1 03 -> [3, 4]
03      10        LT -> [1]
04      6009      PUSH1 09 -> [9, 1]
06      57        JUMPI -> []
09      5B        JUMPDEST -> []
0A      34        CALLVALUE -> [2]
0B      36        CALLDATASIZE -> [4, 2]
0C      02        MUL -> [8]
0D      6008      PUSH1 08 -> [8, 8]
0F      14        EQ -> [1]
10      6014      PUSH1 14 -> [1, 14]
12      57        JUMPI -> []
14      5B        JUMPDEST -> []
15      00        STOP
```