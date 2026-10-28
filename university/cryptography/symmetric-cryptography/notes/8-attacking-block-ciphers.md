![[block_cipher_attacks_lecture_5(1).pdf]]
# DES
A 128 bit key means $2^{128}$ possible keys, but we have to check $2^{127}$ as we expect to find the key halfway (as it is chosen randomly).

One way people tried to keep DES alive was triple DES, encrypting with key 1, decrypting with key 2, and encrypting with key 3. This allows for backwards compatibility, makes adoption very easy, we can just set all keys to be the same if you refuse to upgrade to 3DES.

2DES was not a thing because of:
# Meet-in-the-Middle Attacks

What happens if we encrypt once using k1 and then again with k2?

Need any plaintext ciphertext pair. 
We know that M is encrypted with some key K1, which was then encrypted wit some K2, which gives us C.
We can take M and encrypt with every possible K1 store in a hash table or dictionary or sorted list (we'll look at this) etc. 
We know have a sorted list of middle stages and decrypt C with every possible K2. For every decryption, we match to our sorted list, which takes o(logn).
We do $2^{56}$ encryptions and $2^{55}$ decryptions, meaning $2^{55} + 2^{56} $