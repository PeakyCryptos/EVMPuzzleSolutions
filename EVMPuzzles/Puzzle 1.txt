00      34      CALLVALUE -> value in wei passed in
01      56      JUMP
02      FD      REVERT
03      FD      REVERT
04      FD      REVERT
05      FD      REVERT
06      FD      REVERT
07      FD      REVERT
08      5B      JUMPDEST
09      00      STOP

We can see the objective here is to reach the STOP instruction, to ensure
that the execution comes to a halt without reverting. 

To do this we must reach JUMPDEST (which is a code put in to ensure JUMP is moving to a 
valid JUMP instruction (jump destination).

JUMP consumes the byte location of where JUMPDEST is, which in this case that is 08. Meaning
we must pass in a CALLVALUE of 08 to execute this program cleanly.  


Instruction's and stack after instruction is ran

00      34      CALLVALUE -> [8]
01      56      JUMP -> []
08      5B      JUMPDEST -> []
09      00      STOP 


