---
layout: post
title: Nerual Networks and Deep Learning - Week 2 Logistic Regression
categories: Deep-Learning
tags: Deep-Learning
---

----
## Table of Content
{:.no_toc}

* TOC
{:toc}
----
## Logistic Regression

Logistic regression is a learning algorithm used in a supervised learning problem when the output 𝑦 areall either zero or one. The goal of logistic regression is to minimize the error between its predictions andtraining data.

**Example: Cat vs No - cat**

Given an image represented by a feature vector 𝑥, the algorithm will evaluate the probability of a catbeing in that image.

{% raw %}
$$Given \ x, \hat{y}=P(y=1|x), where \ 0 \leq \hat{y} \leq 1$$
{% endraw %}

<!-- excerpt -->

The parameters used in Logistic regression are:

- The input features vector: {% raw %} $$x \in \mathbb{R}^{n_x}$$ {% endraw %} , where {% raw %} $$n_x$$ {% endraw %} is the number of features 

    for example the 64x64 RGB image, {% raw %} $$n_x = 64 \times 64 \times 3=12,288$$ {% endraw %})

- The trainig lable: {% raw %} $$y \in  0,1$$ {% endraw %}

- {% raw %} $$m$$ {% endraw %} training example {% raw %} $$\{{(x^{(1)},y^{(1)}),(x^{(2)},y^{(2)}),…,(x^{(m)},y^{(m)})}\}$$ {% endraw %}

    {% raw %} $$X={ \left[ \begin{array}{cccc} | & | & ... & |\\ x^{(1)} & x^{(2)} & ... & x^{(m)}\\ | & | & ... & | \end{array}  \right ]}, X \in \mathbb{R}^{n_x \times m}, X.shape=(n_x, m)$$ {% endraw %}

    {% raw %} $$Y = [y^{(1)}, y^{(2)}, ...,y^{(m)}], Y \in \mathbb{R}^{1 \times m, Y.shape=(1,m)}$$ {% endraw %}

- The weights: {% raw %} $$w \in \mathbb{R}^{n_x}$$ {% endraw %} , where {% raw %} $$n_x$$ {% endraw %} is the number of features

- The threshold: {% raw %} $$b \in \mathbb{R}$$ {% endraw %}

- The output (sigmoid function):
{% raw %} $$z = w^T x + b$$ {% endraw %}
{% raw %} $$\hat{y} = a = \sigma(w^Tx+b)=\sigma(z)=\frac{1}{1+e^{-z}}$$ {% endraw %}

<img src="/images/dl-sigma.png" style="width:300px;align:left;" /> 

Some observations from the graph: 

  - If {% raw %} $$z$$ {% endraw %} is a large positive number, then {% raw %} $$\sigma(z)=1$$ {% endraw %}
  - If {% raw %} $$z$$ {% endraw %} is small or large negative number, then {% raw %} $$\sigma(z)=0$$ {% endraw %}
  - If {% raw %} $$z=0$$ {% endraw %},then {% raw %} $$\sigma(z)=0.5$$ {% endraw %}

## Logistic Regression: Cost Function

To train the parameters {% raw %} $$w$$ {% endraw %} and {% raw %} $$b$$ {% endraw %}, we need to define a cost function. 

Recap:

{% raw %} $$\hat{y} = a = \sigma(w^Tx+b)=\sigma(z)=\frac{1}{1+e^{-z}}$$ {% endraw %}

{% raw %} $$Given \ \{{(x^{(1)},y^{(1)}),(x^{(2)},y^{(2)}),…,(x^{(m)},y^{(m)})}\}, want \ {{\hat{y}}^{(i)} \approx y^{(i)}}$$ {% endraw %}

**Loss (error) function**

The loss function measures the discrepancy between the prediction {% raw %} $${\hat{y}}^{(i)}$$ {% endraw %} and the desired output {% raw %} $$y^{(i)}$$ {% endraw %}. In other words, the loss function computes the error for a single training example. 

{% raw %} $$L({\hat{y}}^{(i)},y^{(i)})=\frac{1}{2}{({\hat{y}}^{(i)}-y^{(i)})}^2$$ {% endraw %}

  <img src="/images/dl-loss0.png" style="width:200px;align:left;" />

{% raw %} $$L({\hat{y}}^{(i)},y^{(i)})=-(y^{(i)}log({\hat{y}}^{(i)})+(1-y^{(i)})log(1-{\hat{y}}^{(i)}))$$ {% endraw %}

  <img src="/images/dl-loss1.png" style="width:200px;align:left;" />

- If {% raw %} $$y^{(i)}=1$$ {% endraw %}: {% raw %} $$L({\hat{y}}^{(i)},y^{(i)})=-log({\hat{y}}^{(i)})$$ {% endraw %} where {% raw %} $$log({\hat{y}}^{(i)})$$ {% endraw %} and and {% raw %} $${\hat{y}}^{(i)}$$ {% endraw %} should be close to 1
- If {% raw %} $$y^{(i)}=0$$ {% endraw %}: {% raw %} $$L({\hat{y}}^{(i)},y^{(i)})=-log(1-{\hat{y}}^{(i)})$$ {% endraw %} where {% raw %} $$log(1-{\hat{y}}^{(i)})$$ {% endraw %} and {% raw %} $${\hat{y}}^{(i)}$$ {% endraw %} should be close to 0

**Cost function**

The cost function is the average of the loss function of the entire training set. We are going to find the parameters {% raw %} $$w$$ {% endraw %} and {% raw %} $$b$$ {% endraw %} that minimize the overall cost function. 

{% raw %} $$J(w,b)=\frac{1}{m} \sum\limits_{i=1}^m L({\hat{y}}^{(i)},y^{(i)})=-\frac{1}{m}\sum\limits_{i=1}^m [y^{(i)}log({\hat{y}}^{(i)})+(1-y^{(i)})log(1-{\hat{y}}^{(i)})]$$ {% endraw %}



**Gradient Descent**
Recap:

{% raw %} $$\hat{y} = a = \sigma(w^Tx+b)=\sigma(z)=\frac{1}{1+e^{-z}}$$ {% endraw %}
{% raw %} $$J(w,b)=\frac{1}{m} \sum\limits_{i=1}^m L({\hat{y}}^{(i)},y^{(i)})=-\frac{1}{m}\sum\limits_{i=1}^m [y^{(i)}log({\hat{y}}^{(i)})+(1-y^{(i)})log(1-{\hat{y}}^{(i)})]$$ {% endraw %}

Want to find {% raw %} $$w,b$$ {% endraw %} that minimize {% raw %} $$J(w,b)$$ {% endraw %}
  <img src="/images/dl-gradient-descent.png" style="width:400px;align:left;" />



Every iteration will update {% raw %} $$w$$ {% endraw %} and {% raw %} $$b$$ {% endraw %} as following, the {% raw %} $$\alpha$$ {% endraw %} is the "learing rate"

{% raw %} $$w:=w - \alpha \frac{\mathrm{d}J(w,b)}{\mathrm{d}w}$$ {% endraw %}

{% raw %} $$b:=b - \alpha \frac{\mathrm{d}J(w,b)}{\mathrm{d}b}$$ {% endraw %}

And in code, the derivative {% raw %} $$\frac{\mathrm{d}J(w,b)}{\mathrm{d}w}$$ {% endraw %} is the variable of "dw" and derivative {% raw %} $$\frac{\mathrm{d}J(w,b)}{\mathrm{d}b}$$ {% endraw %} is the variable of "db"

{% raw %} $$w := w - \alpha \mathrm{d}w$$ {% endraw %}

{% raw %} $$b :=  b - \alpha \mathrm{d}b$$ {% endraw %}

  <img src="/images/dl-gradient-descent-2.png" style="width:400px;align:left;" />
  
## Computation Graph 
Computation Graph 

  <img src="/images/dl-Computation-Graph-1.png" style="width:600px;align:left;" />

Derivatives with a Computation Graph 

  <img src="/images/dl-Computation-Graph-2.png" style="width:600px;align:left;" />

## Logistic Regression Gradient descent 

{% raw %} $$z = w^T x + b$$ {% endraw %}

{% raw %} $$\hat{y} = a = \sigma(w^Tx+b)=\sigma(z)=\frac{1}{1+e^{-z}}$$ {% endraw %}

{% raw %} $$L({\hat{y}}^{(i)},y^{(i)})=-(y^{(i)}log({\hat{y}}^{(i)})+(1-y^{(i)})log(1-{\hat{y}}^{(i)}))$$ {% endraw %}

{% raw %} $$J(w,b)=\frac{1}{m} \sum\limits_{i=1}^m L({\hat{y}}^{(i)},y^{(i)})=-\frac{1}{m}\sum\limits_{i=1}^m [y^{(i)}log({\hat{y}}^{(i)})+(1-y^{(i)})log(1-{\hat{y}}^{(i)})]$$ {% endraw %}

### Logistic regression derivatives

  <img src="/images/dl-Logistic-regression-derivatives-1.png" style="width:600px;align:left;" />

{% raw %} $$dz = (a-y)$$ {% endraw %}
{% raw %} $$dw_1 = x_1*dz$$ {% endraw %}
{% raw %} $$dw_2=x_2*dz$$ {% endraw %}
{% raw %} $$db = dz$$ {% endraw %}

### Vectorization

  <img src="/images/dl-Logistic-regression-derivatives-2.png" style="width:600px;align:left;" />

### Vectorizing Logistic Regression 

  <img src="/images/dl-Vectorizing-Logistic-Regression-1.png" style="width:600px;align:left;" />



{% raw %} $$dZ = A-Y$$ {% endraw %}
{% raw %} $$dB = \frac{1}{m} np.sum(dZ)=\frac{1}{m} \sum\limits_{i=1}^m (a^{(i)}-y^{(i)})$$ {% endraw %}
{% raw %} $$dW=\frac{1}{m} XdZ^T=\frac{1}{m}X(A-Y)^T$$ {% endraw %}

### Implementing Logistic Regression

  <img src="/images/dl-Vectorizing-Logistic-Regression-2.png" style="width:600px;align:left;" />

## Code Implementation
{% highlight python linenos %}
import numpy as np
import matplotlib.pyplot as plt
import h5py
import scipy
from PIL import Image
from scipy import ndimage
from lr_utils import load_dataset

%matplotlib inline

# Loading the data (cat/non-cat)
# train_set_x_orig.shape = (209, 64, 64, 3)
# train_set_y.shape = (1, 209)
# test_set_x_orig.shape = (50, 64, 64, 3)
# test_set_y.shape = (1, 50)
train_set_x_orig, train_set_y, test_set_x_orig, test_set_y, classes = load_dataset()

# Number of training examples: m_train = 209
# Number of testing examples: m_test = 50
# Height/Width of each image: num_px = 64
m_train = train_set_y.shape[1]
m_test = test_set_y.shape[1]
num_px = train_set_x_orig.shape[1]

# train_set_x_flatten shape: (12288, 209)
# test_set_x_flatten shape: (12288, 50)
train_set_x_flatten = train_set_x_orig.reshape(train_set_x_orig.shape[0], -1).T
test_set_x_flatten = test_set_x_orig.reshape(test_set_x_orig.shape[0], -1).T

# standardize our dataset.
train_set_x = train_set_x_flatten / 255.
test_set_x = test_set_x_flatten / 255.

# GRADED FUNCTION: sigmoid
def sigmoid(z):
    s = 1 / (1 + np.exp(-z))
    return s

# GRADED FUNCTION: initialize_with_zeros
def initialize_with_zeros(dim):
    w = np.zeros(shape=(dim, 1))
    b = 0
    assert(w.shape == (dim, 1))
    assert(isinstance(b, float) or isinstance(b, int))
    return w, b

# GRADED FUNCTION: propagate
def propagate(w, b, X, Y):
    m = X.shape[1]
    
    # FORWARD PROPAGATION (FROM X TO COST)
    A = sigmoid(np.dot(w.T, X) + b)  # compute activation
    cost = (- 1 / m) * np.sum(Y * np.log(A) + (1 - Y) * (np.log(1 - A)))  # compute cost
    
    # BACKWARD PROPAGATION (TO FIND GRAD)
    dw = (1 / m) * np.dot(X, (A - Y).T)
    db = (1 / m) * np.sum(A - Y)

    assert(dw.shape == w.shape)
    assert(db.dtype == float)
    cost = np.squeeze(cost)
    assert(cost.shape == ())
    
    grads = {"dw": dw,
             "db": db}
    
    return grads, cost

# GRADED FUNCTION: optimize

def optimize(w, b, X, Y, num_iterations, learning_rate, print_cost = False):
    costs = []
    
    for i in range(num_iterations):
        
        # Cost and gradient calculation (≈ 1-4 lines of code)
        grads, cost = propagate(w, b, X, Y)
        
        # Retrieve derivatives from grads
        dw = grads["dw"]
        db = grads["db"]
        
        # update rule (≈ 2 lines of code)
        w = w - learning_rate * dw  # need to broadcast
        b = b - learning_rate * db
        
        # Record the costs
        if i % 100 == 0:
            costs.append(cost)
        
        # Print the cost every 100 training examples
        if print_cost and i % 100 == 0:
            print ("Cost after iteration %i: %f" % (i, cost))
    
    params = {"w": w,
              "b": b}
    
    grads = {"dw": dw,
             "db": db}
    
    return params, grads, costs

# GRADED FUNCTION: predict

def predict(w, b, X):
    
    m = X.shape[1]
    Y_prediction = np.zeros((1, m))
    w = w.reshape(X.shape[0], 1)
    
    # Compute vector "A" predicting the probabilities of a cat being present in the picture
    A = sigmoid(np.dot(w.T, X) + b)
    
    for i in range(A.shape[1]):
        # Convert probabilities a[0,i] to actual predictions p[0,i]
        Y_prediction[0, i] = 1 if A[0, i] > 0.5 else 0
    
    assert(Y_prediction.shape == (1, m))
    
    return Y_prediction

# GRADED FUNCTION: model
def model(X_train, Y_train, X_test, Y_test, num_iterations=2000, learning_rate=0.5, print_cost=False):

    # initialize parameters with zeros (≈ 1 line of code)
    w, b = initialize_with_zeros(X_train.shape[0])

    # Gradient descent (≈ 1 line of code)
    parameters, grads, costs = optimize(w, b, X_train, Y_train, num_iterations, learning_rate, print_cost)
    
    # Retrieve parameters w and b from dictionary "parameters"
    w = parameters["w"]
    b = parameters["b"]
    
    # Predict test/train set examples (≈ 2 lines of code)
    Y_prediction_test = predict(w, b, X_test)
    Y_prediction_train = predict(w, b, X_train)

    # Print train/test Errors
    print("train accuracy: {} %".format(100 - np.mean(np.abs(Y_prediction_train - Y_train)) * 100))
    print("test accuracy: {} %".format(100 - np.mean(np.abs(Y_prediction_test - Y_test)) * 100))

    
    d = {"costs": costs,
         "Y_prediction_test": Y_prediction_test, 
         "Y_prediction_train" : Y_prediction_train, 
         "w" : w, 
         "b" : b,
         "learning_rate" : learning_rate,
         "num_iterations": num_iterations}
    
    return d

# Plot learning curve (with costs)
learning_rates = [0.01, 0.001, 0.0001]
models = {}
for i in learning_rates:
    print ("learning rate is: " + str(i))
    models[str(i)] = model(train_set_x, train_set_y, test_set_x, test_set_y, num_iterations = 1500, learning_rate = i, print_cost = False)
    print ('\n' + "-------------------------------------------------------" + '\n')

for i in learning_rates:
    plt.plot(np.squeeze(models[str(i)]["costs"]), label= str(models[str(i)]["learning_rate"]))

plt.ylabel('cost')
plt.xlabel('iterations')

legend = plt.legend(loc='upper center', shadow=True)
frame = legend.get_frame()
frame.set_facecolor('0.90')
plt.show()
{% endhighlight %}

## Logic Regression in TensorFlow
 TBD