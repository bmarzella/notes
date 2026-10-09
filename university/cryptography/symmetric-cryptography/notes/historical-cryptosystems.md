# Shift (Caesar) Cipher

![[Pasted image 20261009161234.png]]

- Only 25 possible keys
- Vulnerable to brute force attacks
- Can be easily broken by just trying every possible shift

# Substitution Cipher
## Method

![[Pasted image 20261009161444.png]]

## Analysis

- Initially seemingly very secure
- $26! \approx 4 \times 10^{26}$ possible permutations.

However, weak to **frequency analysis.** 

![[Pasted image 20261009161732.png]]

With a reasonably long ciphertext, we can see the most common letter and substitute in the most common English letters.

**A large key space $\ne$ secure**.

![[Pasted image 20261009161909.png]]

# Vigenère Cipher: Polyalphabetic Substitution

***Different substitutions for different positions.***

- Each letter in the key represents a number to shift by. For example, plaintext T and key B means shift T forwards by 2, yielding V.
- Key can be anywhere from the length of the plaintext to a single repeating character (though that would just be a Caesar cipher).

![[Pasted image 20261009162039.png]]

## Analysis

![[Pasted image 20261009162112.png]]

- If the way we apply the key is regular, then it's possible to separate chunks and break them individually.

![[Pasted image 20261009162639.png]]

### Kasiski Attack

![[Pasted image 20261009162705.png]]

