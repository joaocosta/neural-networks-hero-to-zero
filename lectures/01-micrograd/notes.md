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
### Derivative of a function with multiple inputs
## Summary

## Key concepts

## Questions

## Code experiments

## Further study
