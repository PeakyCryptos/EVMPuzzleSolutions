Instruction set and stack after instruction is ran:
```
00      5A        GAS 
01      34        CALLVALUE 
02      5B        JUMPDEST 
03      6001      PUSH1 01 
05      90        SWAP1 
06      03        SUB 
07      80        DUP1
08      6000      PUSH1 00 
0A      14        EQ
0B      6011      PUSH1 11
0D      57        JUMPI
0E      6002      PUSH1 02
10      56        JUMP
11      5B        JUMPDEST
12      5A        GAS
13      90        SWAP1
14      91        SWAP2
15      03        SUB
16      60A6      PUSH1 A6
18      14        EQ
19      601D      PUSH1 1D
1B      57        JUMPI
1C      FD        REVERT
1D      5B        JUMPDEST
1E      00        STOP
```

In this puzzle we can see that for the first JUMPI to have a valid value porition(1)
CALLVALUE - 1 must be equal to 0. 

However, if the JUMPI is not valid  then the instruction continues onto push a 2 to the stack
and jump to that byte offset in instruction set. If we follow the execution flow we can see
that when the program loops back to the second instruction it carries over the previous
result of the subtraction operation

i.e if we push a 3 as CALLVALUE it would look like this:
```
00      5A        GAS -> [GAS]
01      34        CALLVALUE -> [3, GAS]
02      5B        JUMPDEST -> [3, GAS]
03      6001      PUSH1 01 -> [1, 3, GAS]
05      90        SWAP1  -> [3, 1, GAS]
06      03        SUB  -> [2, GAS]
07      80        DUP1 -> [2, 2, GAS]
08      6000      PUSH1 00 -> [00, 2, 2, GAS]
0A      14        EQ -> [0, 2, GAS]
0B      6011      PUSH1 11 -> [11, 0, 2, GAS]
0D      57        JUMPI -> [2, GAS]
0E      6002      PUSH1 02 -> [2, 2, GAS]
10      56        JUMP -> [2, GAS] (Jumps back to 02 in instruction with this stack)
```

As you can see it will continue looping in this manner until either the remaining gas runs out,
or the JUMPI has a satisfied value porition. I.e if we pass in a CALLVALUE of 3 then 
we will decrement 3 by 1, 3 times to get 0 to cause a valid JUMPI that breaks out of the loop.

If the first JUMPI is traversed successfully it carries over the remaining gas pushed to the
stack at the start of the execution. Remaining gas is then calculated again and the operation
remaining gas beginning - remaining gas after the loop is executed. If this equals A6 (166)
then we will satisfy the value portion of the second JUMPI allowing us to come to a clean
halt of this program.  

Thus, our mission is to pass in a sufficient CALLVALUE such that it invokes a loop enough times
that remaining gas beginning - remaining gas after = 166.
281474976689653 

CALLVALUE: 2 , JUMPDEST: 1, PUSH1: 3, SWAP1: 3, SUB: 3, DUP1: 3, EQ: 3 	      
JUMPI: 10, GAS: 2

Initital run Gas cost (before loop is entered): 4 (GAS + CALLVALUE) 

Regular Loop Gas cost: 43 

Final Loop Gas cost: 33 (1 loop)

Amount of regular loops = (166 - 33 - 4) / 43 = 3

CALLVALUE = # of regular loops + Final Loop
CALLVALUE = 3 + 1 = 4