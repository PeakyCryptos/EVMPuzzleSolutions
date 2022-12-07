[00]	PUSH32	fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff0
[21]	CALLVALUE	
[22]	ADD	
[23]	PUSH1	01
[25]	EQ	
[26]	PUSH1	2a
[28]	JUMPI	
[29]	REVERT	
[2a]	JUMPDEST	
[2b]	STOP	

fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff0
represents the value 
115792089237316195423570985008687907853269984665640564039457584007913129639920
In decimal (Lets call this PUSH32 VALUE).

To have the value portion of the JUMPI evaluate to 1 (so we have have a valid JUMP)
we must have the operation CALLVALUE + (the PUSH32 value) = 01

The MAX_UINT in the evm is:
115792089237316195423570985008687907853269984665640564039457584007913129639935

To have a value that when added to the PUSH32 VALUE will equal 1, we must cause an overflow.
In the EVM MAX_UINT + 1 = 0.

To find the value that equals 1 we do (MAX_UINT + 2) - PUSH32 VALUE
Which is: 17 (11 in hex which it automatically converts to the base of when passing input value)