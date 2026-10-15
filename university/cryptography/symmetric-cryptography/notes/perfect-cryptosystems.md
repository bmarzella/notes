# Perfect Security

> ***A cryptosystem is perfect if knowledge of the ciphertext $C$ gives absolutely no information about the plaintext $M$.***

Every possible plaintext is equally likely given any ciphertext. Observing ciphertext $C$ provides no information to reduce uncertainty about plaintext $M$

$P(M|C) = P(M)$ for all $M \in \mathcal{M} \, \space C \in \mathcal{C}$
## Fundamental Limitation

![[Pasted image 20261009163532.png]]

### Proof

![[Pasted image 20261009163623.png]]

1. Suppose the number of possible keys is *strictly* less than the number of plaintexts, and let $C$ be a ciphertext. 
2. Also let $d(C)$ be the set of plaintexts that can be decrypted from $C$.
3. $d(C)$ is a subset of all possible plaintexts, and since encryption is injective for every key, the number of plaintexts that can be decrypted from $C$ is less than or equal to the number of total possible keys, which is less than the number of total possible messages.
4. This means that the set of plaintexts that can be decrypted from $C$ is a subset of the set of all possible messages. 
5. This must mean that there is some other message that belongs to the set of all possible messages but not to the set of messages that can be decrypted from $C$, therefore we've learned something about the plaintext, it isn't $M*$.
6. **Contradiction. This cannot be a perfect cryptosystem so our initial assumption that keys is strictly less than plaintexts is wrong.**

>*If there are fewer possible keys than possible plaintexts, and an attacker intercepts a ciphertext, they can test every possible key and rule out at least one possible plaintext - giving them information about the message and proving the system isn't perfectly secret.* 

### Achieving Perfect Security - One-Time Pad

A one-time pad is an encryption technique where we generate a completely random key of the exact same length as the plaintext. A purely random key means a purely random ciphertext.

For example, if we choose every letter of a Vigenère key independently and uniformly at random, we get a one-time pad:

![[Pasted image 20261009165030.png]]
![[Pasted image 20261009165132.png]]

#### Practical Issues with One-Time Pad

![[Pasted image 20261009165207.png]]
![[Pasted image 20261009165325.png]]

*Need to securely transmit and store a long, complicated key. Not much easier than just having a secure channel to send the message.*

Reusing a key, even once, completely breaks the one-time pad. To generate our ciphertext, we (usually, not above though!) `XOR` the key with the plaintext:$$C_1 = M_1 \oplus K$$$$C_2 = M_2 \oplus K$$
If we use the same key, and the attacker were to intercept both plaintexts, `XOR`ing them together would produce the key:$$C_1 \oplus C_2 = (M_1 \oplus K) \oplus (M_2 \oplus K) = M_1 \oplus M_2$$
Similarly, if we use modular addition, as we do with Vigenère, the ciphertext can still be compromised:$$(C_1 - C_2) \pmod{26} = (M_1 - M_2) \pmod{26}$$
If $C_1 - C_2 = 0$, then $M_1$ and $M_2$ have the exact same character at that position. Spaces are the most common character in English, which we can use to reconstruct both messages.
#### Historical Use of One-Time Pad
![[Pasted image 20261009165347.png]]

#### Implementation and Modern Use of One-Time Pad
![[Pasted image 20261009165443.png]]