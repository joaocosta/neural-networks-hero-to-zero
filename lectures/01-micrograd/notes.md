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
## Summary

## Key concepts

## Questions

## Code experiments

## Further study
