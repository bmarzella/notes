# Introduction
1. Advanced Encryption Standard introduced to replace DES.
2. Larger key sizes, can choose from 128, 192 and 256 bits.
3. Algebraically derived S-boxes (understanding which is beyond scope)
# Architecture
Uses a substitution-permutation network (not Feistel)
Consist of three components
1. XOR with something idk
2. S boxes
3. Permute for diffusion etc

S-boxes must be invertible, injective. Feistel s-boxes have more inputs that outputs so cannot be used here.
AES s-boxes 8-bits -> 8-bits.
