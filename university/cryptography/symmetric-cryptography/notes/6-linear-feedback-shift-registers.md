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
Runs three different non-linear FSRs in parallel, A feeds into B feeds into C feeds into A etc.., but they all update at once.
To generate output:
1. Each `XOR`s an internal bit with the rightmost bit to get an individual output.
2. Individual outputs are `XOR`d together to get a final output.
3. This part is completely linear.

To generate feedback:
1. Each FSR pulls from self and other FSRs.
2. Feedback is a function taking a bit from this FSR and a subset of bits from other FSRs.
*Notes don't go into any more detail than this, so I assume this is all I need to know.*

To set up, we have a seed and initialisation vector. seed fills first 80 bits of A, IV first 80 of B, all else is 0 except for final 3 bits of C, which are 1. This runs for 1152 clock cycles before it begins to produce output, ensuring initial state is shaken up first.

## Salsa20 and ChaCha20
We take as input 256 bit seed, 64 bit iv. made up of a padding function pad and permutation `pi`.
Pad takes seed, IV and a 64-bit counter (which tracks which block this is), using this to form a 512 bit block.
Permutation then scrambles each block produced by Pad. It has three operations:
- **Addition (modulo $2^{32}$):** Adds two 32-bit numbers and throws away any overflow past 32 bits (like a clock wrapping around). This creates the non-linearity needed to stop linear algebra attacks.
- **Bitwise Rotation:** Shifts bits left, but instead of dropping the ones that fall off the end, it loops them back around to the right side (like a wheel spinning). This spreads changes quickly across the block.
- **XOR:** Standard bitwise XOR.

The $i^{th}$ block is $\pi(h_i) \oplus h_i$, where $h_i$ is the block created by Pad.

ChaCha is just an improved variant of Salsa. Both are considered secure.
# Generating Random Bits in Practice
To produce a secure keystream with a PRG, we need a truly random seed. See [[5. psuedorandom-generators-and-stream-ciphers#Issues with Generating Truly Random Keys|this outline on TRGs]].
# Summary
1. LFSRs provide efficient pseudorandom generation but are cryptographically weak alone  
2. Nonlinearity is essential - can be introduced via feedback, output, or combination  
3. Modern ciphers like Trivium and ChaCha20 use sophisticated nonlinear constructions  
4. Random number generation requires continuous entropy collection from environment  
5. Security depends on implementation - proper entropy management is crucial