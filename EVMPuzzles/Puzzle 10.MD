```
00      38          CODESIZE
01      34          CALLVALUE
02      90          SWAP1
03      11          GT
04      6008        PUSH1 08
06      57          JUMPI
07      FD          REVERT
08      5B          JUMPDEST
09      36          CALLDATASIZE
0A      610003      PUSH2 0003
0D      90          SWAP1
0E      06          MOD
0F      15          ISZERO
10      34          CALLVALUE
11      600A        PUSH1 0A
13      01          ADD
14      57          JUMPI
15      FD          REVERT
16      FD          REVERT
17      FD          REVERT
18      FD          REVERT
19      5B          JUMPDEST
1A      00          STOP
```

As seen in the previous puzzle, this one also contains two JUMPI instructions. 

Taking a look at the second one we can see the jump location is not a hardcoded instruction.
Instead this is the outcome of an ADD operation that performs addition on the two top most
items of the stack. 0A(10) is added to CALLVALUE. If this value results in 19, we reach
the JUMPDEST instruction that will allow this program to execute cleanly. Because the result
returns a hex value, if we want to reach location 19 this is actually 25 in hex. So we must have
10 + CALLVALUE = 25 (which is 15).

Going to the beginning of the program so that we can clear the first JUMPI we can see that.
There is a instruction CODESIZE which loads the bytecode size of the code onto the stack.
In this case that would be 1B (27). The next instruction loads the CALLVALUE you specified
on the stack.

This is followed by a SWAP1 instruction which swaps the position of CODESIZE and CALLVALUE.
What follows this is a GT(Greater than opcode) if the stack looks like so:
[a, b] the operation will evaluate a > b. It will return 1 if the outcome is true, 0 otherwise.
Unlike the second JUMPI we can see that this one has the first JUMPDEST location hardcoded.
Meaning our mission is to just make the value porition of the JUMPI 1, which will be the case
if CODESIZE is greater than CALLVALUE.

What follows the first JUMPDEST is a CALLDATASIZE operation which just reads in
the byte size of the call data you pass in. Then the PUSH2 operator pushes a 2 byte size
operation onto the stack. Then we see another SWAP1 operation which exchanges the push data
to the stack, with the CALLDATASIZE. These values are consumed by the MOD operator which
returns the remainer of an integer division onto the stack. Again the format being
[a, b] -> a % b.

The ISZERO operator checks if the top most element on the stack(outcome of mod operation)
is a zero or not. Pushing a 1 if so, 0 if not. Next, a CALLVALUE operation just pushes
the call value input onto the stack. Which as was explained previously, is just added together
and then later used as the JUMP location for the JUMPI. Now we also know that the value portion
of the second JUMPI comes from what is pushed via the ISZERO instruction.
  
Now we know we have 2 rules to follow for CALLVALUE. CALLVALUE less than 27, and when added to 10 
produces a 25. For CALLADATASIZE, it must be a value that when divided by 3, divides into
as whole, resulting in the MOD operator pushing a 0. Then the ISZERO operator will push a 1
allowing the JUMPI instruction to be valid, instead of continuing onto a REVERT.   

CALLVALUE: 15
CALLADATASIZE: any multiple of 3 (I choose just 3) 

Instruction's and stack after instruction is ran:
```
00      38          CODESIZE -> [1b]
01      34          CALLVALUE -> [f, 1b]
02      90          SWAP1 -> [1b, f]
03      11          GT -> [1]
04      6008        PUSH1 08 -> [8, 1]
06      57          JUMPI -> []
08      5B          JUMPDEST -> []
09      36          CALLDATASIZE -> [3]
0A      610003      PUSH2 0003 -> [0003, 3]
0D      90          SWAP1 -> [3, 0003]
0E      06          MOD -> [0]
0F      15          ISZERO -> [1]
10      34          CALLVALUE -> [f, 1]
11      600A        PUSH1 0A -> [a, f, 1]
13      01          ADD -> [19, 1]
14      57          JUMPI -> []
19      5B          JUMPDEST -> []
1A      00          STOP
```