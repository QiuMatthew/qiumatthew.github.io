---
title: Ch9-Fourier Transform Properties
date: 2021-08-12 16:24:41
categories:
- engineering
tags: 
- engineering
- signal and system
---

## Chapter 9 Fourier Transform Properties

### Symmetric Relation
If $ x(t) $ is real valued
then
$ X(-\omega) = X^*(\omega) $

### Scaling Property
$$ x(at) \leftrightarrow \frac{1}{|a|}X(\frac{\omega}{a}) $$

### Duality
$$ x(t) \leftrightarrow X(\omega) $$
$$ X(t) \leftrightarrow 2 \pi x(-\omega) $$

### Parseval's Relation
$$ \int_{-\infty}^{\infty} |x(t)|^2 dt = \frac{1}{2\pi} \int_{-\infty}^{\infty} |X(\omega)|^2 d\omega $$
$$ \frac{1}{T_0} \int_{-\infty}^{\infty} |\tilde{x}(t)|^2 dt = \sum_{-\infty}^{\infty} |a_k|^2 $$

### Time Shifting
$$ x(t - t_0) \leftrightarrow e^{-j \omega t_0} X(\omega) $$

### Differntiation
$$ \frac{\mathrm{d} }{\mathrm{d} t} x(t) \leftrightarrow j \omega X(\omega) $$

### Integration
$$ \int_{-\infty}^{t} x(\tau) d\tau \leftrightarrow \frac{1}{j \omega} X(\omega) + \pi X(0) \delta(\omega) $$

### Linearity
No need to state

### Convolution Property
$$ h(t) * x(t) \leftrightarrow H(\omega) X(\omega) $$
This is the basis of FILTERING

### Modulation Property
$$ h(t) x(t) \leftrightarrow \frac{1}{2\pi} H(\omega) * X(\omega) $$
This is the basis of AMPLITUDE MODULATION

