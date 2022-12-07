```
00      34      CALLVALUE
01      38      CODESIZE
02      18      XOR
03      56      JUMP
04      FD      REVERT
05      FD      REVERT
06      FD      REVERT
07      FD      REVERT
08      FD      REVERT
09      FD      REVERT
0A      5B      JUMPDEST
0B      00      STOP
```

Here we can see we need a JUMP of 0A (10) to reach a JUMPDEST that allows for a clean
execution. Right before the JUMP instruction is a XOR operation on the CALLVALUE
and CODESIZE. The CODESIZE is 12. 

To perform the XOR operation lets convert the CODESIZE and 10 to binary and refer to this XOR table.
https://community.anaplan.com/t5/image/serverpage/image-id/29631i3AA6C01377A8550F/image-size/large?v=v2&px=999



```
CALLVALUE
XOR
1100
-----------
1010
```

The CALLVALUE that produces the bottom result if we refer to the table is
1 XOR 0 = 1
1 XOR 1 = 0
0 XOR 1 = 1
0 XOR 0 = 0

0110 is the required CALLVALUE to pass this. as CALLVALUE is passed in as decimal
we must convert this back to base 10 which is 6.
 

Instructions and stack after instruction has been executed

```
00      34      CALLVALUE -> [6]
01      38      CODESIZE -> [C, 6]
02      18      XOR -> [a]
03      56      JUMP -> []
0A      5B      JUMPDEST -> []
0B      00      STOP
```