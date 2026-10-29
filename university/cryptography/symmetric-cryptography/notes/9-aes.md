# Introduction
1. Advanced Encryption Standard introduced to replace DES.
2. Larger key sizes, can choose from 128, 192 and 256 bits.
3. Algebraically derived S-boxes (understanding which is beyond scope)
# Architecture
Uses a substitution-permutation network (not Feistel)
Consist of three components:
1. S-boxes for nonlinearity
2. (Linear) Permutations for diffusion
3. Subkey XORing for confusion.

Difference from Feistel is that S-boxes must be invertible and injective. Feistel s-boxes have more inputs than outputs and thus cannot be used here.

**Blocks**: 128 bits, represented as 4x4 arrays of bytes, each cell containing one byte (8 bits)
**Key**: 128, 192 or 258 bits.
**Rounds**: Depends on ke
AES s-boxes 8-bits -> 8-bits.
