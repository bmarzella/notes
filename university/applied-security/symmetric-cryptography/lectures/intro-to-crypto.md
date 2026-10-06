---
handwriting-page-id: 8c996772-2a23-42ed-9441-6ce08704277c
---
#kerckchoff #notation 

# Fundamentals of Private-Key Cryptography

![[Pasted image 20261006113301.png]]

> ***Set-up:** All Messages are sent through an external, potentially insecure channel*

We are interested in protecting the contents of the message from an **eavesdropper**, **known as Eve**. The **sender and encoder** is called **Alice**, and the **receiver and decoder** is called **Bob**.

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
e(m_{1})
$$
