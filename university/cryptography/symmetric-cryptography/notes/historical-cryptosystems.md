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

**Takeaway is that a large key space **