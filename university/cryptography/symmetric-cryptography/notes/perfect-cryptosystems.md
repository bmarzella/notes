# Perfect Security

> ***A cryptosystem is perfect if knowledge of the ciphertext $C$ gives absolutely no information about the plaintext $M$.***

Every possible plaintext is equally likely given any ciphertext. Observing ciphertext $C$ provides no information to reduce uncertainty about plaintext $M$

$P(M|C) = P(M)$ for all $M \in \mathcal{M} \, \space C \in \mathcal{C}$
## Fundamental Limitation

![[Pasted image 20261009163532.png]]


### Proof

![[Pasted image 20261009163623.png]]

1. Suppose the number of possible keys is less than the number of plaintexts, and let $C$ be a ciphertext. 
2. Also let $d(C)$ be the set of plaintexts that can be decrypted from $C$.
3. $d(C)$ is a subset of all possible plaintexts, and since encryption is injective for every key, the number of plaintexts that can be decrypted from $C$ is less than or equal to the number of total possible keys, which is less than the number of total possible messages.
4. This means that the set of plaintexts that can be decrypted from $C$ is a subset of the set of all possible messages. 
5. This must mean that there is some other message that belongs to the set of all possible messages but not to the set of messages that can be decrypted from $C$, therefore we've learned something about the plaintext, it isn't $M*$.

>**