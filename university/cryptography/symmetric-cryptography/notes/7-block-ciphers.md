# Definition
Lecture notes complicate this a lot, all it means is that a block cipher accepts $n$-bits at a time. Otherwise, the definition is the same as what we've seen so far.
# Modes of Operations
A block cipher isn't a cryptosystem, rather it is used to encrypt particular block(s) of data which are much shorter than the messages we want to encrypt (key length $\geq$ block length). To form a full cryptosystem, we define a **mode of operation.**
## Electronic Codebook
**Message divided into blocks, each block is encrypted/decrypted separately.** 

Lack of **diffusion**
- Because each block is handled individually, the plaintext isn't "spread out" throughout the ciphertext
- This means patterns are hidden poorly and are able to be found in the ciphertext.
- Thus, **ECB** is not recommended.
## Feistel
- Iterative, consisting of total rounds $r$.
	- This is typically even and $\geq 3$
- Split input in half, giving us a left (L) and right (R) half, each t bits long.
- In each round, right half goes through a scrambling (**not encryption!**) function $f$.
	- The subkey for each round is derived from main encryption key
- Scrambled right half is XORd with left half, producing a new right half
- Halves swap sides, right becomes left and vice versa
- Once we reach r rounds, we swap our halves one last time and combine to get our ciphertext.

Decryption just does this but reversing the order of subkeys.

The $f$ function doesn't need to be invertible (hence why I called it a scrambling function) to allow for decryption. It can be any function as long as that function doesn't compromise the cipher.

Only the right side ever acts as an input on $f$, the left side is never touched (and vice-versa for decryption). 
- Sides swap each round, so both halves are used as input, but it's always one side.

This all results in a simple to use process. Decryption is the same as encryption, all we change is the order of the keys, and we don't have to worry about making $f$ reversible, making designing the function easier!
### Properties of a Feistal Function
1. **Confusion** - Each ciphertext bit should depend on multiple plaintexts bits.
2. **Diffusion** - Changing a single plaintext bit should change many ciphertext bits.
	- Similar to confusion in the opposite direction
	- Otherwise we can look at small sections in isolation, which is much easier than breaking the full ciphertext.
3. **Nonlinearity** - Linearity lets us do a lot to find the keys.
## Data Encryption Standard
Feistal cipher that works with 64-bit blocks.
64-bit keys, divided into 8-bit bytes. Last bit of ea