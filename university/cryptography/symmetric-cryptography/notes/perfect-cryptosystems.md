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