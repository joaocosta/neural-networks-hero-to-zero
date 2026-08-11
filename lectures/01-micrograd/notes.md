---
title: "The spelled-out intro to neural networks and backpropagation: building micrograd"
lecture: 1
video_url: https://www.youtube.com/watch?v=VMj-3S1tku0&list=PLAqhIrjkxbuWI23v9cThsA9GvCAUhRvKZ
status: in-progress
sources:
  - id: micrograd
    commit:
tags:
  - lecture
---

# The spelled-out intro to neural networks and backpropagation: building micrograd

## Sections
### Micrograd overview
Micrograd is a minimalistic auto gradient engine that implements back propagation. 
Neural networks are a class of mathematical expressions.
Backpropagation is a more general concept.
Micrograd works with individual scalars, as opposed to working with a tensor (array).  A tensor can be processed efficiently in parallel.  Micrograd is simpler and slower, but the math is exactly the same, so for pedagogic reasons, it's simpler to start with micrograd and worry about optimization later.
### Derivative of a simple function with one input

$f(x) = 3*x^2 - 4*x + 5$

![[attachments/Pasted image 20260808231434.png]]

What is the derivative of this function ? What does it represent ?
What is derivative measuring ? What does it tell us about the function ?

The derivative of $f(x)$ is defined as $f'(x) = \lim_{h \to 0} \frac{f(x+h) - f(x)}{h}$.

The limit as h goes towards 0 of $(f(x+h) - f(x) )$ divided by $h$.  In other words, if we add a small amount to $x$, how does the function respond, with what sensitivity ? What is the slope at that point ? Is the slope positive or negative, does it go up or down and by how much ?

$h$ represents how much we are moving across the x-axis of the chart.  If we choose a very small $h$ (approching 0), say $h=0.001$:
$$
\begin{aligned}
x &= 3 \\
f(x) &= 3*3^2-4*3+5 = 20 \\
f(x+h)-f(x) &= 0.01400300000000243
\end{aligned}
$$
We normalize that by $h$ (the run), ie, rise over run
$$
\begin{aligned}
\frac{f(x+h) - f(x)}{h} &= 14.00300000000243
\end{aligned}
$$
$14$ is a numerical approximation of the slope of $f(x)$ when $x = 3$. The slope is positive.

This can be confirmed with the formula for the derivative:

$$
\begin{aligned}
f'(x) &= 6*x - 4 \\
f'(3) &= 6 * 3 - 4 = 18 - 4 = 14
\end{aligned}
$$

If we were to consider $x = -3$, just by looking at the plot we can see that the slope would be negative, when $x = -3$, if we add to $x$ an $h$ approaching 0, the value of the function goes down.

At some point, the slope would be $0$, in other words, at a point, if we nudge in the positive direction, the function doesn't respond, it stays nearly the same.
### Derivative of a function with multiple inputs

$f(a,b,c) = ab+c$

Now we look at the derivatives of $f(a,b,c)$ with respect to $a$, $b$ and $c$.

Given
$$
\begin{aligned}
h &= 0.0001 \\
a &= 2.0 \\
b &= -3.0 \\
c &= 10.0
\end{aligned}
$$

#### Derivative with respect to $a$


$$
f(a,b,c) = 4
$$
If we nudge a by $h$, then:

$$
f(a+h, b, c) = 3.999699...
$$
Increasing $a$ slightly resulted in the value of the function decreasing, the slope is negative.  Because we are multiplying $a$ by a negative number.

The slope is
$$
\begin{aligned}
\frac{3.9997-4.0}{h} = -3
\end{aligned}
$$
We increased $a$ by $0.0001$, the value of the function $f$ decreased by $0.0003$.  The rate of change is $-3$, which is the value of $b$.
#### Derivative with respect to $b$

$$
\begin{aligned}
f(a, b+h, c) = 2*(-3+0.0001) + 10.0 = 4.0002
\end{aligned}
$$
As we nudge b in a positive direction, the result of the function is higher, the slope of the derivative at $a=2, b=-3, c=10$ is positive.

The slope is
$$
\begin{aligned}
\frac{4.0002-4.0}{h} = 2
\end{aligned}
$$
The rate at which $f$ increases as we increase $b$ by $h$ is 2.  We increased $h$ by $0.0001$, the function $f$ increased by $0.0002$, therefore the rate of increase, ie, the derivative, is $2$.  $2$ is the value of $a$.
#### Derivative with respect to $c$

$$
\begin{aligned}
f(a, b+h, c) = 2*(-3) + (10.0 + 0.0001) = 4.0001
\end{aligned}
$$
The slope is:

$$
\begin{aligned}
\frac{4.0001-4.0}{h} = 1
\end{aligned}
$$
The slope is 1, that is the rate at which $f$ increases as we increase $h$

### Micrograd core _Value_ object and its visualization

The _Value_ class of micrograd is a scalar number that supports automatic differentiation.

We start with a basic class:

```python
class Value:

  def __init__(self, data):
    self.data = data

  def __repr__(self):
    return f"Value({self.data})"
```

That initial class allows us to represent scalar values.  Now we would like to perform operations on them, ie:

```python
a = Value(2.0)
b = Value(-3.0)
c = Value(10.0)

print(a*b + c)
```

So we increment the class with:

```python
  def __add__(self, other):
        out = Value(self.data + other.data)
        return out

  def __mul__(self, other):
        out = Value(self.data * other.data)
        return out
```

This gives me basic operations over values.  Next comes the connective tissue, how to keep track of the expression graph by keeping pointers of what values produced what other values.

We are going to keep a set of children, ie:

```python
  def __init__(self, data, _children=()):
    self.data = data
    self._prev = set(_children)
```

When we create a value originally, _children_ will be the empty set, but when we apply operations on values, we are going to feed in the children of this value.  In the case of our operations (add,mul), the children will be _self_ and _other_.

```python
  def _add__(self, other)
    out = Value(self.data + other.data, (self, other), '+')
    return out

  def _mul__(self, other)
    out = Value(self.data * other.data, (self, other), '*')
    return out
```

We need one more element to keep track of the operation:

```python
  def __init__(self, data, _children=(), _op = ''):
    self.data = data
    self._prev = set(_children)
    self._op = _op
```

This allows us to build mathematical expressions using only add/mul operations so far.  We can do a forward pass of the expressions towards the final result.

![[attachments/Pasted image 20260809234956.png]]

To do back propagation, we need to also track grad

```python
  def __init__(self, data, _children=(), _op = ''):
    self.data = data
    self.grad = 0
    self._prev = set(_children)
    self._op = _op
```

At initialization the gradient is 0, ie, it does not impact the output of the function.

### Manual backpropagation

Take this DAG:

![[attachments/Pasted image 20260811003411.png]]

Manual backpropagation starting with $L$.  How much does $L$ change if we add $h$ ? Obviously it changes by $h$, ie:

$$
\begin{aligned}
\frac{(L+h) - L}{h} = 1
\end{aligned}
$$
The gradient is 1.  What about the gradient of $d$ ?

$$
\begin{aligned}
\frac{((d + h) * f) - (d * f)}{h} = \frac{d*f + h*f - d*f}{h} = f
\end{aligned}
$$
So the gradient of $d$ is $-2$.

And the gradient of $f$ ?

$$
\begin{aligned}
\frac{(d * (f + h)) - (d * f)}{h} = \frac{d*f + d*h - d*f}{h} = d
\end{aligned}
$$
The gradient of $f$ is $4$.

Going further backwards, what is the derivative of $c$  (ie: $\frac{dL}{dc}$)

If we nudge $c$, how does that impact $L$ ?  It impacts $L$ through $d$. So first, what is the derivative of $d$ with respect to $c$  ($\frac{dd}{dc}$)? If we nudge $c$, how does it impact $d$ ?

We know $d = c + e$

$$
\begin{aligned}
\frac{dd}{dc} = \frac{(c+e+h) - (c+e)}{h} = 1
\end{aligned}
$$
Equally, $\frac{dd}{de} = 1$

$$
\begin{aligned}
\frac{dd}{dc} &= 1 \\
\frac{dL}{dd} &= -2
\end{aligned}
$$
So what is $\frac{dL}{dc}$ ?  The answer is the [Calculus chain rule](https://en.wikipedia.org/wiki/Chain_rule).

> ** Calculus Chain Rule **
> If a variable z depends on the variable $y$, which itself depends on the variable $x$ (that is, $y$ and $z$ are [dependent variables](https://en.wikipedia.org/wiki/Dependent_variable "Dependent variable")), then z depends on x as well, via the intermediate variable y. In this case, the chain rule is expressed as:

$$
\frac{dz}{dx} = \frac{dz}{dy}.\frac{dy}{dx}
$$

In our case, $z$ is $L$, $y$ is $d$ and $x$ is $c$.
$$
\frac{dL}{dc} = \frac{dL}{dd}.\frac{dd}{dc} = \frac{-2}{1} = -2
$$

>The chain rule states that knowing the instantaneous rate of change of $z$ relative to $y$ and that of $y$ relative to $x$ allows one to calculate the instantaneous rate of change of $z$ relative to $x$ as the product of the two rates of change.

>If a car travels twice as fast as a bycicle and a bycicle travels four times as fast as a man, a car travels eight times as fast as a man.

So finally, what are $\frac{dL}{da}$ and $\frac{dL}{db}$ ?

We know that $e = a * b$ and * $\frac{dL}{de} = -2$ .

To apply the chain rule, we need to know $\frac{de}{da}$ and $\frac{de}{db}$.

$$
\begin{aligned}
\frac{de}{da} = \frac{(a+h)*b - a*b}{h} = b = -3
\end{aligned}
$$

$$
\begin{aligned}
\frac{de}{db} = \frac{a*(b+h) - a*b}{h} = a = 2
\end{aligned}
$$

$$
\begin{aligned}
\frac{dL}{da} &= -3 * -2 = 6 \\
\frac{dL}{db} &= 2 * -2 = -4
\end{aligned}
$$

Back propagation is just a recursive application of the calculus chain rule backwards through the computation graph.

![[attachments/Pasted image 20260811011735.png]]
## Summary

## Key concepts

## Questions

## Code experiments

## Further study
