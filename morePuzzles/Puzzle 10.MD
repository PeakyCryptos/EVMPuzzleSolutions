```
[00]	PUSH1	20
[02]	PUSH1	00
[04]	PUSH1	00
[06]	CALLDATACOPY	
[07]	PUSH1	00
[09]	MLOAD	
[0a]	PUSH32	f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0
[2b]	AND	
[2c]	PUSH1	20
[2e]	PUSH1	20
[30]	PUSH1	00
[32]	CALLDATACOPY	
[33]	PUSH1	00
[35]	MLOAD	
[36]	OR	
[37]	PUSH32	abababababababababababababababababababababababababababababababab
[58]	EQ	
[59]	PUSH1	5d
[5b]	JUMPI	
[5c]	REVERT	
[5d]	JUMPDEST	
[5e]	STOP
```

for the JUMPI to have a valid value portion the outcome of the EQ operation
must be 1. For this to happen abababababababababababababababababababababababababababababababab
must be whats on the stack previously as we do an equality operation with it.

There 2 main operations here that rely on a 64 byte value you pass in as CALLDATA.
The first 32 bytes are AND'd with 
f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0

The next 32 bytes are OR'd with the outcome of the AND operation from previous. 

Thus if our outcome is to produce abababababababababababababababababababababababababababababababab
In the first step we can pass in this value itself, the outcome will be 
a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0a0
As anything AND'd with 0, produces a 0.

Next when we OR this value with 
abababababababababababababababababababababababababababababababab
we get the output we desire of 
abababababababababababababababababababababababababababababababab
Why this is the case is because anything OR'd with a 1 is a 1.