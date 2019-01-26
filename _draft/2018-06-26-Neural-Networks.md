---
layout: post
title: Nerual Networks and Deep Learning - Week 3 Nerual Networks
categories: Deep-Learning
tags: Deep-Learning
---

----
## Table of Content
{:.no_toc}

* TOC
{:toc}
----
## Nerual Networks

<img src="/images/dl-neural-network-1.png" style="width:600px;align:left;" />

<!-- excerpt -->

##  Vectorizing across multiple examples

<img src="/images/dl-neural-network-2.png" style="width:600px;align:left;" />

Some explanation here:

{% raw %}
$$A^{[0]} = X={ \left[ \begin{array}{cccc} | & | & ... & |\\ x^{(1)} & x^{(2)} & ... & x^{(m)}\\ | & | & ... & | \end{array}  \right ]}, X.shape=(n_x, m)$$
{% endraw %}

{% raw %}
$$Z^{[1]} = W^{[1]}X + b^{[1]}={ \left[ \begin{array}{cccc} | & | & ... & |\\ z^{[1](1)} & z^{[1](2)} & ... & z^{[1](m)}\\ | & | & ... & | \end{array}  \right ]}, Z^{[1]}.shape=(\text{hidden units of Layer [1]}, m)$$
{% endraw %}

{% raw %}
$$A^{[1]} = \sigma(Z)={ \left[ \begin{array}{cccc} | & | & ... & |\\ a^{[1](1)} & a^{[1](2)} & ... & a^{[1](m)}\\ | & | & ... & | \end{array}  \right ]}, A^{[1]}.shape=(\text{hidden units of Layer [1]}, m)$$
{% endraw %}

## Activation Functions

<img src="/images/dl-neural-network-3.png" style="width:600px;align:left;" />

Sigmoid activation function

{% raw %}
$$g(z)=\sigma(z)=\frac{1}{1+e^{-z}}=a$$
{% endraw %}

{% raw %}
$$g'(z)=\frac{-1}{(1+e^{-z})^2} (-e^{-z})=\frac{e^{-z}}{(1+e^{-z})^2}=\frac{1}{1+e^{-z}}(1-\frac{1}{1+e^{-z}})=a(1-a)$$
{% endraw %}

Tanh activation function

{% raw %}
$$g(z)=tanh(z)=\frac{e^z - e^{-z}}{e^z+e^{-z}}=a$$
{% endraw %}

{% raw %}
$$g'(z)=1-{(tanh(z))}^2=1-a^2$$
{% endraw %}

ReLU

{% raw %}
$$g(z)=max(0,z)$$
{% endraw %}

{% raw %}
$$g'(z)=\left\{\begin{array}{lr} 0 &  \text{if } z < 0\\ 1 & \text{if } z \geq 0 \end{array} \right.$$
{% endraw %}

Leaky ReLU

{% raw %}
$$g(z)=max(0.01z,z)$$
{% endraw %}

{% raw %}
$$g'(z)=\left\{\begin{array}{lr} 0.01 &  \text{if } z < 0\\ 1 & \text{if } z \geq 0 \end{array} \right.$$
{% endraw %}

## Gradient descent for neural networks

**Forward Propagation**

{% raw %}
$$Z^{[1]}=W^{[1]}X + b^{[1]}$$
{% endraw %}

{% raw %}
$$A^{[1]}=g^{[1]}(Z^{[1]})$$
{% endraw %}

{% raw %}
$$Z^{[2]}=W^{[2]}A^{[1]} + b^{[2]}$$
{% endraw %}

{% raw %}
$$A^{[2]}=g^{[2]}(Z^{[2]})=\sigma(Z^{[2]})$$
{% endraw %}

**Backward Propagation**

{% raw %}
$$dZ^{[2]}=A^{[2]} - Y, \text{where } Y=[y^{(1)}, y^{(2)}, ...., y^{(m)}]$$
{% endraw %}

{% raw %}
$$dW^{[2]}=\frac{1}{m}dZ^{[2]} A^{[1]T}$$
{% endraw %}

{% raw %}
$$db^{[2]}=\frac{1}{m}np.sum(dZ^{[2]}, axis-1, keepdims=True), \text{where } keepdims \text{ will shape into } (n^{[1]},1) \text{ instead of } (n^{[1]},)$$
{% endraw %}