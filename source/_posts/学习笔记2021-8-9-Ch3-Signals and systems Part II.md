---
title: Ch3-Signals and systems Part II
date: 2021-08-09 10:48:47
categories:
tags: 
    - engineering
    - signal and system
---

## Chapter 3 Signals and systems Part II
### Properties of systems
#### Memoryless
The output at any time only depends on the input at that time
#### Invertability
For any output, there's only one input that can cause this output
#### Causality
The output at any time only depends on the input at past and present, namely, the system cannot anticipate the future input
If x1(t) = x2(t) when t <= t0
Then y1(t0) = y2(t0)
The impulse response h(t) is 0 when t < 0
The Region of Convergence of H(s) is at the right side of the right most pole
#### Stability
If the input is bounded, the output is also bounded
The impulse response h(t) is absolute integrable.
The Region of Convergence of H(s) must include the jw-axis
#### Time invariance
If x(t) -> x(t - t0)
Then y(t) -> y(t - t0)
#### Linearity
If x(t) -> ax1(t) + bx2(t)
Then y(t) -> ay1(t) + by2(t)
