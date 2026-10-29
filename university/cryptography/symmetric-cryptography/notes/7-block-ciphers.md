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
### Properties of a Feistel Function
1. **Confusion** - Each ciphertext bit should depend on multiple plaintexts bits.
2. **Diffusion** - Changing a single plaintext bit should change many ciphertext bits.
	- Similar to confusion in the opposite direction
	- Otherwise we can look at small sections in isolation, which is much easier than breaking the full ciphertext.
3. **Nonlinearity** - Linearity lets us do a lot to find the keys.
## Data Encryption Standard
- Feistel cipher that works with 64-bit blocks.
- 64-bit keys, divided into 8-bit bytes. Last bit of each bit is a parity bit
	- Last bit is set so the total number of 1s is always odd
- This means keys are effectively 56-bits
	- No idea why this is the case, and primary reason it's now considered insecure.
	- At this point, security by obscurity was still a thing, so we don't really know why a lot of things about DES are the way they are
### Key Schedule
We want to produce the 16 subkeys for Feistel's 16 rounds. We want a 48-bit key for every round (which is weird, because surely we'd do $r$, again, no clear reason for this.)

1. Take main 64-bit key $K$ and run $PC_{1}(K)$:
	1. Drops 8 parity bits, bringing us down to 56-bits.
	2. Generates a permutation.
	3. Splits permutation into two halves $C$ and $D$, each 28-bits.
2. Perform circular left shift on both halves by one or two bits, depending on round count.
	1. If round = 1,2, 9 or 16, then shift by one bit. 
	2. Shift by two otherwise.
3. Run $PC_{2}(C, D)$, which recombines $C$ and $D$ and takes a permutation of 48-bits to be our subkey.
4. Repeat steps 2 and 3 to generate remaining 15 subkeys.
### Expansion Function
Now, we have to turn our right half from 32 to 48 bits so it matches the key length. We do this with expansion function $E$:

1. Split 32-bit block into eight 4-bit blocks.
2. Take first and last bit of each block and attach to neighbouring blocks.
	1. $\text{Block } 1: [b_{32} \ \mathbf{b_1 \ b_2 \ b_3 \ b_4} \ b_5] \quad \vert{} \quad \text{Block } 2: [b_4 \ \mathbf{b_5 \ b_6 \ b_7 \ b_8} \ b_9] \quad \vert \quad \dots$
3. This gives us eight 6-bit blocks.

These blocks are combined back together and XORed with the subkey. This is known as the **S-box input block,**
### S-Box
A substitution box (s-box) is essentially made up of 8 lookup tables $(S_{1}, S_{2}, \dots, S_{8})$. 

1. Split the input box back into eight 6-bit strings ($B_1, B_2, \dots, B_8$) 
2. Take 6-bit string, read bits 1 and 6 as two bits, denary is our row number
	- `011011` -> `01` -> row 1
2. Take remaining middle 4-bits, read as denary for column number
	- `011011` -> `1101` -> column 13
3. Output is the corresponding value in corresponding table.
	1. $B_1 \rightarrow S_1, \space B_2 \rightarrow S_2 \dots$
4. Concatenate results into a 32-bit string.

This 32-bit string is permuted once last time, then XOR'd with the left half to become our new right half.
## DES Security
56-bits is just too small for a key.

## Variants and Legacy
### Triple DES
- Uses DES three times
- Effective key length of 112 or 168 bits
- 