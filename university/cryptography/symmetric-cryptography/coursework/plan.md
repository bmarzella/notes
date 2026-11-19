---
handwriting-page-id: 8abe0380-454e-4836-a8de-91c8086fffe5
---
1. takes plaintext-ciphertext and the number of rounds and returns encryption key.
2. what is complexity of feistel, can we just brute force?
	1. $n \times r$ i think, thats reasonable no?
	2. in which case we just try every possible key on the plaintext $O(rn)$ and check whether it matches the ciphertext $O(m)$, so we'd expect to find the key in $O\left( \frac{n}{2} \right)$?
$$

$$