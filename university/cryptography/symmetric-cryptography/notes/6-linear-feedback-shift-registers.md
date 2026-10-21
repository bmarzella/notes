![[Lecture 3.pdf]]
# LFSRs
1. Read rightmost bit of the seed string, add this to our pseudorandom string.
2. Shift everything to the right, deleting the rightmost bit and leaving an empty space in at the start.
3. There are certain positions known as taps. The value of every active tap position before the shift is `XOR`'d together and the result is used to fill the gap.
4. This repeats until we get a pseudorandom string of the length that we need.

This has some **implications:**
1. First n bits of pseudorandom string are just the original seed backwards.
2. If we ever see any state again, then we'll begin looping. At best, we can get $2^{n}-1$ states before we'll definitely repeat.
	- $-1$ because seeing all zeroes guarantees we get stuck.

![[Pasted image 20261021093217.png]]

*Note *

All of this is linear, so multiplying a state by the inverse matrix lets us step a state backwards. An attacker isn't likely to see the entire keystream in a transmission, but this lets them reverse back to the seed from observing any $2n$ bits.
# Non-Linearity
replace xor with some non linear function
apply non lienar function to entire state
combine multiple non linearly

need to choose function wisely, and would probably end up mostly 0s which is particularly bad as we established, or would be all ones etc
non linear functions need to take a random input and return a roughly random output, not the case for and or or
doing this gives us an FSR, droppping the linear

trivium for example does something probably
snow does non linear of entire state

