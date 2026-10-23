# Differential Cryptoanalysis
Comparing XOR of two inputs with XOR of two other inputs.
(Plaintext 1 XOR key) XOR (Plaintext 2 XOR key) = Plaintext 1 XOR Plaintext 2
Permutations don't stop this, we just get a permutated XOR result.
If an s-box often produces an output-differential when given an input-differential, we can do something orother. linear will always do this

linear, if input differential results in an output differential, an pair of inputs that produce that differential produce that sme output differential

Consider examples in notes, non-linear 4-bit to 4-bit. 8 possible input differentials
