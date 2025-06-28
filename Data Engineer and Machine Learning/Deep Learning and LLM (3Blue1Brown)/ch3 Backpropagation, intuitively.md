algorithm: backpropagation

magnitude of weights: how sensitive the output responds to the features



what it does:

for each training example, start from the last layer, with the label (correct answer), try to find how to tune each weights and biases to best predict the label

last layer -> second last layer -> ..... -> first layer

propagate backwards



real-world: **random mini-batch**, average among them and compute the gradient to speed up

stochastic gradient descent