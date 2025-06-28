transformer:

(attention+multilayer perception) -> repeat



step 1

token, or word embedding

good embedding model: similar words have similar directions (**semantic meaning**)

**dot product** of embedding: how two words are aligned



embedding not only encodes the meaning of the word, it might also encode the **context** of a word in the sentence

through... **attention**



soft-max function

turn an arbitrary list of numbers into a valid distribution (0, 1)

e^x to normalize

temperature: a configurable constant T in the denominator of e^(x/T)

**the larger the T, the more weight the lower values have** => distribution is more uniform



input of softmax: **logits**

output: probabilities