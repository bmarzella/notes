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
**Eve knows $e$, $d$ and $C$**, and is able to recover $M$ or $K$ from this.
Weakest posible atack model, if a cipher is broken by a ciphertext only attack, it is very insecure.

## Known-Plaintext

Model:
- **Eve has a plaintext-ciphertext pair ($M_{i}, C_{i})$**
- Goal: Determine $K$ to decrypt other messages
- Can analyse the relationship betwen known pairs

More power full that ciphertext-only, access to pairs reveals entire encryption structure.

## Chose-Plaintext

Eve is able to encrypt messages of their choice, for example, they have access to an *encryption oracle*. They can generate as many pairs as they wish.

Most powerful attack, modern systems must be secure up to and including this.


# Public-Key Cryptogrphy

How can Alice send a message to Bob without meeting prior?
This is the kind of scenario we see on the internet, for example, every day. You might not necessarily be able to have a secure channel with someone you want to talk to before 
