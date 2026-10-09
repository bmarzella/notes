# Perfect Security

> ***A cryptosystem is perfect if knowledge of the ciphertext $C$ gives absolutely no information about the plaintext $M$.***

Every possible plaintext is equally likely given any ciphertext. Observing ciphertext $C$ provides no information to reduce uncertainty about plaintext $M$

$P(M|C) = P(M)$ for all $M \in \mathcal{M} \, \space C \in \mathcal{C}$
## Fundamental Limitation

![[Pasted image 20261009163532.png]]


### Proof

![[Pasted image 20261009163623.png]]

Suppose the number of keys is less than the number of plaintexts, and let C be a ciphertext. 
d(C) is the set of plaintexts that can be decrypted from C.
d(C) is a subset of all possible plaintexts, and since encryption is injective for every k, the number of plaintexts that can be decypted from c is less that or equal to the number ofkeys which is les than the bumber of total possible messages
This would make d(C) a subset of $\mathcal{M}$ and there exists some other message M* that belongs to 