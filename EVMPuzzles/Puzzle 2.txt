00      34      CALLVALUE
01      38      CODESIZE
02      03      SUB
03      56      JUMP
04      FD      REVERT
05      FD      REVERT
06      5B      JUMPDEST
07      00      STOP
08      FD      REVERT
09      FD      REVERT

We can see there is one JUMP instruction here that will lead to a clean halt (STOP).
This JUMP instruction would need to take the value of 06 to reach the JUMPDEST that will allow 
this.

What precedes the JUMP instruction is SUB which is a subtraction of the two top most items on the
stack. CODESIZE - CALLVALUE = 06.CALLVALUE is the value in wei that we are passing in. 
CODESIZE is the bytesize of the code itself

CODESIZE = 10 (10 instructions) | 10 - 4  = 6
thus, CALLVALUE should be 4 if we expect to halt execution cleanly.


Instructions and stack after instruction has been executed

00      34      CALLVALUE -> [4]
01      38      CODESIZE -> [a]
02      03      SUB -> [6]
03      56      JUMP -> []
06      5B      JUMPDEST -> []
07      00      STOP