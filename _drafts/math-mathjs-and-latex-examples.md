---
layout: post
title:  "MathJs and Latex Equation Examples"
author: sma
categories: [ math ]
image: assets/images/posts/antoine-dautry-428776-unsplash.jpg
description: "MathJs and Latex Equation Examples"
---

you can use LaTeX equations. 

Simple inline equations like $f(x) = ax+b$ can be written simply by using

```
$f(x) = ax + b$
```

while displayed equations just like

\\[
f(t) = \frac{a_0}{2} + \sum\limits_{n=0}^{\infty} \left[a_n \mathrm{cos}(\omega_n t) + \mathrm{sin}(\omega_n t) \right]
\\]


can be done by typing

 ```
 \\[ f(t) = \frac{a_0}{2} + \Sum\limits_{n=0}^{\infty} \left[a_n \mathrm{cos}(\omega_n t) + \mathrm{sin}(\omega_n t) \right]. \\]
 ```

Numbered equations can be done by using `\tag{}` inside equation environment as show in

\\[
\vec{F} = m \ddot{\vec{r}}, \tag{1} \label{eq:sample}
\\]

by using

```
\\[
\vec{F} = m \ddot{\vec{r}} \tag{1} \label{eq:sample}
\\]
```

Thus the equation can be referenced by using `\ref{}` latex command. For exemple, the eq. ($\ref{eq:sample}$) is referenced by using `$\ref{eq:sample}$`

For more information see [Mathjax documentation](http://docs.mathjax.org/en/latest/tex.html)
