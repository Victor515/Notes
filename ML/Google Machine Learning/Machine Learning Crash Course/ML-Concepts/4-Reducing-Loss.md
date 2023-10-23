# An Iterative Approach

![The cycle of moving from features and labels to models and predictions.](https://developers.google.com/static/machine-learning/crash-course/images/GradientDescentDiagram.svg)

A Machine Learning model is trained by starting with an **initial guess** for the weights and bias and **iteratively adjusting** those guesses until learning the weights and bias with the **lowest possible loss**.

Iterative strategies are prevalent in machine learning, primarily because **they scale so well to large data sets**.





 "Compute parameter updates" part:

For now, just assume that this mysterious box devises new values and then the machine learning system re-evaluates all those features against all those labels, yielding a new value for the loss function, which yields new parameter values.

Iterate until overall loss stops changing or at least changes extremely slowly. When that happens, we say that the model has **converged**.



# Gradient Descent

For the kind of regression problems (linear regression) we've been examining, the resulting plot of loss vs. w1 will always be **convex**.

![A plot of a U-shaped curve, with the vertical axis labeled as 'loss' and the horizontal axis labeled as value of weight w i.](https://developers.google.com/static/machine-learning/crash-course/images/convex.svg)

**Convex problems** have only one minimum; that is, only one place where the slope is exactly 0. That minimum is where the loss function converges.



Gradient descent:

1. Pick a starting value for w1
2. calculate the gradient of the loss curve at the starting point (in the example, the gradient is equal to the derivative, or the slope of the curve)
3. **The gradient always points in the direction of steepest increase in the loss function**. The gradient descent algorithm takes **a step** in the **direction of the negative gradient** in order to reduce loss as quickly as possible (单变量的情况下，因为方向只有正负两个看起来没什么，但是在多元函数的情况下就更有意义了）.



Math note:

The **gradient** of a function, denoted as follows, is the **vector of partial derivatives** with respect to all of the independent variables:
$$
\nabla f(x,y) = \left(\frac{\partial f}{\partial x}(x,y), \frac{\partial f}{\partial y}(x,y)\right)
$$
The gradient of f(x,y)  is a two-dimensional vector that tells you in which (x,y) direction to move for the maximum increase in height. Thus, the negative of the gradient moves you in the direction of maximum decrease in height. **In other words, the negative of the gradient vector points into the valley.**

Note that a gradient is a vector, so it has both of the following characteristics:

- a **direction**, by def, The gradient **always points in the direction of steepest increase** in the loss function
- a **magnitude**



# Learning Rate

Gradient descent algorithms multiply the gradient by a **scalar** known as the **learning rate** (also sometimes called **step size**) to determine the next point.

For example, gradient magnitude = 2.5, learning rate = 0.1, next point will be 0.025 away



**Hyperparameters**: knobs that programmers tweak in machine learning algorithms. Learning rate is one of the hyperparameters.

Math about the ideal learning rate: https://developers.google.com/machine-learning/crash-course/reducing-loss/learning-rate#expandable-1



# Stochastic Gradient Descent

Term **Batch**: the total number of examples you use to calculate the gradient in a single iteration

We cannot use the entire data set (all examples) in large scale, so randomly sampling examples might be redundant

**Stochastic Gradient descent (SGD)**: uses only a single example (a batch size of 1) per iteration. Given enough iterations, SGD works but is very noisy

**Mini-batch stochastic gradient descent (mini-batch SGD)**: typically 10 ~ 1000 examples