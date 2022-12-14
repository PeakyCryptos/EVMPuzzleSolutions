```
00      34          CALLVALUE
01      80          DUP1
02      02          MUL
03      610100      PUSH2 0100
06      14          EQ
07      600C        PUSH1 0C
09      57          JUMPI
0A      FD          REVERT
0B      FD          REVERT
0C      5B          JUMPDEST
0D      00          STOP
0E      FD          REVERT
0F      FD          REVERT
```

In this puzzle we have a JUMPI which takes 2 parameters off the stack. The first being 
the byte offset in code where JUMPDEST is located, and a value. If this value is 0
then the program will continue to the next instruction, if not then it will JUMP to the 
specified byte offset.

We can see the top most item pushed to the stack before the JUMPI is 0C which is 11.
The location of the JUMPDEST is alread hardcoded for us. The next item pushed to the stack
would be the value portion of JUMPI. Which is the outcome of an EQ (equality comparison) 
between whatever CALLVALUE is passed in multiplied against itself compared against 0100 (256).

The aim is to produce a CALLVALUE that when multiplied against itself will be equivalent to the
value 256. sqrt(256) = 16 (16 * 16 = 256). 

Instruction's and stack after instruction is ran

```
00      34          CALLVALUE -> [10]
01      80          DUP1 -> [10, 10]
02      02          MUL -> [100]
03      610100      PUSH2 0100 -> [100, 100]
06      14          EQ -> [1]
07      600C        PUSH1 0C -> [C, 1]
09      57          JUMPI -> []
0C      5B          JUMPDEST -> []
0D      00          STOP
```