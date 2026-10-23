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
- Iterative, consisting of total rounds r
- Split input in half, giving us a left (L) and right (R) half, each t bits long.
- In each round, one half goes through an encryption function.
- Encrypted half is XORd with other half
- 
