# Differential Cryptoanalysis
**Input Differential** - XOR between pair of inputs, **output differential** is self explanatory.

Two 6-bit string inputs, split into inner outer bits to get row and column, look up value on table for output. repeat for second input, getting our second output, then we can XOOR our inputs and our outputs to get our differentials.

Different pairs of inputs will give the same input differential but might give a different output differential. However, if a certain output differential shows up particularly frequently, them we know that the corresponding input differentials are showing up frequently, which gives us information about the key.

Comparing XOR of two inputs with XOR of two other inputs.
(Plaintext 1 XOR key) XOR (Plaintext 2 XOR key) = Plaintext 1 XOR Plaintext 2
Permutations don't stop this, we just get a permutated XOR result.
If an s-box often produces an output-differential when given an input-differential, we can do something orother. linear will always do this

linear, if input differential results in an output differential, an pair of inputs that produce that differential produce that sme output differential

Consider examples in notes, non-linear 4-bit to 4-bit. 8 possible input differentials pairs or something idk
