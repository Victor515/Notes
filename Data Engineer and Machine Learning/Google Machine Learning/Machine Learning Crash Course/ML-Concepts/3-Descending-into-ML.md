**Linear regression** is a method for finding the straight line or hyperplane that best fits a set of points.



# Linear Regression

Line fitting example to relate slope and offset

model: y = wx + b, w: weight, b: bias, x: feature, y: predicted label

L2 Loss: squared error (y-y')^2



# Training and Loss

**Training** a model simply means *learning (determining) good values for all the weights and the bias* from labeled examples

Supervised learning: attempt to find a model that minimizes loss => **empirical risk minimization**



**Q**: How to aggregate the individual losses?

## Squared Loss

Or l2 loss

```
  = the square of the difference between the label and the prediction
  = (observation - prediction(x))2
  = (y - y')2
```

**Mean square error** (**MSE**) 
$$
MSE = \frac{1}{N} \sum_{(x,y)\in D} (y - prediction(x))^2
$$
Although MSE is commonly-used in machine learning, it is neither the only practical loss function nor the best loss function for all circumstances.