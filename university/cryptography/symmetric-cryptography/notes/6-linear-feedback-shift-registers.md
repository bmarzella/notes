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

*Note the initial state, the left-most bit is the highest index, we are XORing the rightmost bits to get our fill value.*

All of this is linear, so multiplying a state by the inverse matrix lets us step a state backwards. An attacker isn't likely to see the entire keystream in a transmission, but this lets them reverse back to the seed from observing any $2n$ bits.
# Non-Linearity
To solve the linearity issue, we introduce non-linear elements. We can do this in three ways:
## Nonlinear Feedback Function
*Feedback function being how we select our input bit per cycle.*
Replace XOR with some non linear function with $\approx \frac{1}{2}$ probability of `0` or `1`.
Result is a **Feedback Shift Register (FSR)**
## Nonlinear Output Function
Apply non linear function to entire state to decide our output
## Nonlinear Combine
Slide is self explanatory.
# Practical Stream Cipher Constructions
## Trivium
Runs three different non-linear FSRs in parallel. 
To generate output:
1. Each `XOR`s an internal bit with the rightmost bit to get an individual output.
2. Individual outputs are `XOR`d together to get a final output.
3. This is completely linear.

To generate feedback:
1. Each FSR pulls from self and other 


need to choose function wisely, and would probably end up mostly 0s which is particularly bad as we established, or would be all ones etc
non linear functions need to take a random input and return a roughly random output, not the case for and or or
doing this gives us an FSR, droppping the linear

trivium for example does something probably
snow does non linear of entire state

