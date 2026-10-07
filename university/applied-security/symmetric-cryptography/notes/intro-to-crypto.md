---
handwriting-page-id: 8c996772-2a23-42ed-9441-6ce08704277c
---
#kerckchoff #notation 

# Fundamentals of Private-Key Cryptography

![[Pasted image 20261006113301.png]]

> ***Set-up:** All Messages are sent through an external, potentially insecure channel*

We are interested in protecting the contents of the message from an **eavesdropper**, **known as Eve**. The **sender and encoder** is called **Alice**, and the **receiver and decoder** is called **Bob**.

**For private-key encryption, Alice and Bob pre-agree on a private key, which Alice uses to encrypt and Bob uses to decrypt.**
## Notation

1.  $M \in m$ is known as the *plaintext*. 
2. Alice and Bob have some secret information $K \in k$, known as the **key**. 
3. Alice encrypts using an **encryption function** $e: m \times k \rightarrow c$

The **transmitted sequence, cyphertext** looks like:
$$
C = e(M,K) \in c
$$
Bob receives $C$ and decrypts using the **decryption function** $d : c \times k \rightarrow c$. We require that:
$$
d(e(M,K),K) = M
$$
for **all plaintexts $M$**. This implies that, **for a given key $K$, the encryption function must be injective:**
$$
e(M_{1},K) = e(M_{2},K) \implies M_{1} = M_{2}
$$
> *Injective = one-to-one. 
> If the cyphertext of two messages matches when we use the same key, the original messages must match.*

However, **encryption functions do not needs to be injective in the key domain:**
$$
e(M, K_{1}) = e(M, K_{2}) \centernot\implies K_{1} = K_{2}
$$
> *Cyphertext of the same message matching doesn't necessarily mean the same key was used.*

**In short, one key will never produce the same cyphertext from two different plaintexts, but two different keys can create the same cyphertext from the same message.**


# Kerchoffs' Principles

*The cypher method must not be required to be secret, and it must be able to fall into the hands of the enemy with inconvenience.*

**Security should rely only on the secrecy of the key - relying on security by obscurity is bad!**

This has **four key advantages**:

1. **Key management:** It is much easier to keep *short* keys secret that complex algorithms.
2. **Recovery from compromise**: Should a key be compromised, you can very easily change key. It isn't so simple to change an algorithm.
3. **Standardisation:** It is much for every user to rely on a personal key rather than a personal algorithms.
4. **Collaboration**: Public scrutiny finds and fixes weaknesses

**Assume that Eve knows $e$ and $d$, but not $K$**.

## Attacks
## Cyphertext-Only Attacks

![[Pasted image 20261007101433.png]]

An attacker is able to recover the original message and/or the encryption key with just the resulting ciphertext. If a cipher can be broken this way it is very weak.
## Known-Plaintext

![[Pasted image 20261007101536.png]]

An attacker is able to determine the key used if they have a plaintext-cyphertext pair(s). A more powerful attack as the attacker has more information, but also means that the cipher is more secure (because an attacker needs more information in order to break it).
## Chosen-Plaintext

![[Pasted image 20261007095543.png]]

An attacker is able to encrypt messages of their choice and generate the corresponding cyphertext, meaning they can generate as many plaintext-ciphertext pairs as they wish.

Of course, they wont have the key to do this, but they have some sort of *encryption oracle*, who they give a message to and spits out the corresponding cyphertext without revealing how it works, an API call for example.

Most powerful attack, modern systems must be secure up to and including this.


# Public-Key Cryptogrphy

How can Alice send a message to Bob without meeting prior?
This is the kind of scenario we see on the internet, for example, every day. You might not necessarily be able to have a secure channel with someone you want to talk to before 
