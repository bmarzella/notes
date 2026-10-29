![[block_cipher_attacks_lecture_5(1).pdf]]
# DES
If we have a 128 bit key, that means $2^{128}$ possible keys. However, we expect to only have to check $2^{127}$ if brute forcing as the key is chosen randomly, so on average, we'll find the key after searching half the key space.

One way people tried to keep DES alive was triple DES, encrypting with key 1, decrypting with key 2, and encrypting with key 3. This allows for backwards compatibility, makes adoption very easy, we can just set all keys to be the same if you refuse to upgrade to 3DES.

*DES-X also exists, but it doesn't appear in slides so I assume it isn't important.*
# Meet-in-the-Middle Attacks
2DES (encrypt -> encrypt) because of MITM attacks. If we have any plaintext-ciphertext pair, then:

1. We know that M is encrypted with some key K1, which was then encrypted with some K2,  giving us C.
2. We can take M and encrypt with every possible K1 and store the in a hash table or sorted list. This takes $2^{56}$ operations.
3. We can then decrypt C with every possible K2 and check our data structure to see whether we find a match.
4. In a hash table, this is $O(1)$, and in a sorted array, this is $O(Log(n)) = O(Log(2^{56})) = O(56)$, either of which is negligible.
5. Thus, our total operations is $2(2^{56}) = 2^{57}$.

We do $2^{56}$ encryptions and $2^{55}$ decryptions, meaning $2^{55} + 2^{56} \approx 2^{56}$. We gain very little security.
This isn't just for 2DES. Anytime an encryption system does something with one function then something with a second, it is vulnerable to a meet-in-the-middle attack and thus not much better than just a single function.
This however requires $2^{k}$ space, as we're storing all of our middle stage entries.

For 3DES, we roll two of our functions into one, so $f_1$ might be encrypt; and $f_2$ decrypt, encrypt (this is the preferable orientation as we have to store less.)
For $f_2$, we expect to do $2^{111}$ operations and $2^{56}$ for f. Total cost is $\approx 2^{111}$. roughly the same as brute forcing two operations instead of three, which, while not ideal, is still considered secure.