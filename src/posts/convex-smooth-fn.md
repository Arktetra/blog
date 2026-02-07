---
title: Convex Smooth Functions
description: About convex smooth functions.
date: '2026-02-04'
categories:
    - convex-optimization
published: true
---
A function $f: \mathbb{R}^d \rightarrow \mathbb{R}$ is said to be convex for any $\textbf{x}, 
\textbf{y} \in \mathbb{R}^d$ if,
$$
\forall \lambda \in [0, 1] \quad f((1 - \lambda) \textbf{x} + \lambda \textbf{y}) \le (1 - \lambda) f(\textbf{x}) + \lambda f(\textbf{y}) 
\label{eq:jensen_inequality} \tag{1}
$$
This inequality is known as Jensen's inequality.

Let us consider a convex funtion $f(x) = x^2$ as shown in the figure below:

![Convex Function Interpretation](convex/convex-interpretation.png)

Consider two points $x$ and $y$ with corresponding values of $f(x)$ and $f(y)$ as shown in the
above figure. The blue curve between $x$ and $y$ in the above figure represent the left hand
side of equation $\eqref{eq:jensen_inequality}$. The red curve connecting $f(x)$ with $f(y)$
represents the right hand side of equation $\eqref{eq:jensen_inequality}$. Then, in this simple
scenario, equation $\eqref{eq:jensen_inequality}$ can be interpreted as, *"a function is convex 
if, for every pair $x, y \in \mathbb{R}$, the blue curve between $x$ and $y$ is below the red line."*

From $\eqref{eq:jensen_inequality}$,
$$
\begin{align}
f\left(\textbf{x} + \lambda(\textbf{y} - \textbf{x})\right) &\le (1 - \lambda) f(\textbf{x}) + \lambda f(\textbf{y}) \\
f\left(\textbf{x} + \lambda(\textbf{y} - \textbf{x})\right) - f(\textbf{x}) &\le \lambda\left(f(\textbf{y}) - f(\textbf{x})\right) \\
\frac{f\left(\textbf{x} + \lambda(\textbf{y} - \textbf{x})\right) - f(\textbf{x})}{\lambda} &\le f(\textbf{y}) - f(\textbf{x}) \\
\end{align}
$$

Taking limit $\lambda \rightarrow 0$ on both sides, we get,
$$
\begin{align}
\lim_{\lambda \rightarrow 0} \frac{f\left(\textbf{x} + \lambda(\textbf{y} - \textbf{x})\right) - f(\textbf{x})}{\lambda} &\le f(\textbf{y}) - f(\textbf{x}) \\
\label{eq:first_order_characterization} \tag{2}
f'(\textbf{x}, (\textbf{y} - \textbf{x})) &\le f(\textbf{y}) - f(\textbf{x})
\end{align}
$$
where, $f'(\textbf{x}, (\textbf{y} - \textbf{x}))$ is directional derivative.

The Taylor series expansion of $f(\textbf{x} + \lambda (\textbf{y} - \textbf{x}))$ is given by,
$$
\begin{align}
f(\textbf{x} + \lambda (\textbf{y} - \textbf{x})) &= f(\textbf{x}) + \lambda \nabla f(x)^T (\textbf{y} - \textbf{x}) + \text{small error} \\
f(\textbf{x} + \lambda (\textbf{y} - \textbf{x})) &\approx f(\textbf{x}) + \lambda \nabla f(x)^T (\textbf{y} - \textbf{x}) \\
\frac{f(\textbf{x} + \lambda (\textbf{y} - \textbf{x})) - f(\textbf{x})}{\lambda} &= \nabla f(x)^T (\textbf{y} - \textbf{x}) \\
\end{align}
$$

Taking limit $\lambda \rightarrow 0$ on both sides, we get,
$$
f'(\textbf{x}, (\textbf{y} - \textbf{x})) = \nabla f(x)^T (\textbf{y} - \textbf{x})
\label{eq:directional_derivative} \tag{3}
$$

From equation $\eqref{eq:first_order_characterization}$ and $\eqref{eq:directional_derivative}$,
$$
\begin{align}
\nabla f(x)^T (\textbf{y} - \textbf{x}) \le f(\textbf{y}) - f(\textbf{x}) \\
f(\textbf{y}) \ge f(\textbf{x}) + \nabla f(x)^T (\textbf{y} - \textbf{x}) 
\end{align}
$$

Thus, if $f$ is differentiable, and its gradients $f(\textbf{x})$ exists for all $\textbf{x} \in \mathbb{R}^d$,
then,
$$
f(\textbf{y}) \ge f(\textbf{x}) + \nabla f(x)^T (\textbf{y} - \textbf{x}) 
\label{eq:gradient_characterization} \tag{4}
$$

Let us continue with our running example of $f(x) = x^2$ and try to decipher the meaning of equation 
$\eqref{eq:gradient_characterization}$ through the figure below:

![gradient characterization](convex/gradient_characterization.png)

If we make the gradient point towards the same direction as $y - x$, then we obtain the green arrow in
the above figure. According to equation $\eqref{eq:gradient_characterization}$, *"for a function $f$ to 
be convex, such green arrow at $f(x)$ must be below the red line."*

## Alpha-Strongly Convex Function

A function is said to be $\lambda$-strongly convex if,
$$
f(\textbf{y}) \ge f(\textbf{x}) + \nabla f(\textbf{x})^T (\textbf{y} - \textbf{x}) + \frac{\lambda}{2}\left\lVert\textbf{y} - \textbf{x}\right\rVert_2^2
\label{eq:lambda-strongly-convex} \tag{5}
$$

## Beta-Smooth Function

A function is said to be $\beta$-smooth if,
$$
f(\textbf{y}) \le f(\textbf{x}) + \nabla f(\textbf{x})^T (\textbf{y} - \textbf{x}) + \frac{\beta}{2}\left\lVert\textbf{y} - \textbf{x}\right\rVert_2^2
\label{eq:beta-smooth} \tag{6}
$$

The above condition is equivalent to the Lipschitz condition over gradients,
$$
\begin{equation}
\left\lVert \nabla f(\textbf{y}) - \nabla f(\textbf{x}) \right\rVert_2 \le \beta \left\lVert \textbf{y} - \textbf{x} \right\rVert_2
\label{eq:lipschitz-1} \tag{7}
\end{equation}
$$

The equations $(6)$ and $(7)$ are also equivalent with,
$$
\begin{align}
f(\textbf{y}) &\ge f(\textbf{x}) + \nabla f(\textbf{x})^T(\textbf{y} - \textbf{x}) - \frac{\beta}{2}\left\lVert \textbf{y} - \textbf{x} \right\rVert_2^2
\label{eq:lipschitz-2} \tag{8} \\
f(\lambda \textbf{x} + (1 - \lambda)\textbf{y}) &\ge \lambda f(\textbf{x}) + (1 - \lambda) f(\textbf{y}) + \frac{\lambda (1 - \lambda)}{2} \beta \left\lVert \textbf{y} - \textbf{x} \right\rVert_2^2
\label{eq:lipschitz-3} \tag{9} \\
f(\lambda \textbf{x} + (1 - \lambda)\textbf{y}) &\le \lambda f(\textbf{x}) + (1 - \lambda) f(\textbf{y}) - \frac{\lambda (1 - \lambda)}{2} \beta \left\lVert \textbf{y} - \textbf{x} \right\rVert_2^2
\label{eq:lipschitz-4} \tag{10} \\
\end{align}
$$

---


 **Proof of $\eqref{eq:beta-smooth}$:** 

 From $\eqref{eq:lipschitz-1}$, we have,
 $$
 \left\lVert \nabla f(\textbf{y}) - \nabla f(\textbf{x})\right\rVert_2 \le \beta \left\lVert \textbf{y} - \textbf{x}\right\rVert_2.
 \label{eq:proof-1} \tag{11} \\
 $$
 Multiplying both sides by $\left\lVert \textbf{y} - \textbf{x}\right\rVert_2$, we get,
 $$
 \left\lVert \nabla f(\textbf{y}) - \nabla f(\textbf{x})\right\rVert_2\left\lVert \textbf{y} - \textbf{x}\right\rVert_2 \le \beta \left\lVert \textbf{y} - \textbf{x}\right\rVert_2^2.
 \label{eq:proof-2} \tag{12} \\
 $$
 From Cauchy-Schwarz inequality, we have,
 $$
 \left(\nabla f(\textbf{y}) - \nabla f(\textbf{x})\right)^T(\textbf{y} - \textbf{x}) \le \left\lVert \nabla f(\textbf{y}) - \nabla f(\textbf{x})\right\rVert_2\left\lVert \textbf{y} - \textbf{x}\right\rVert_2.
 \label{eq:proof-3} \tag{13} \\
 $$
 From equation $\eqref{eq:proof-2}$ and $\eqref{eq:proof-3}$, we get,
 $$
 \begin{align}
 \left(\nabla f(\textbf{y}) - \nabla f(\textbf{x})\right)^T(\textbf{y} - \textbf{x}) &\le \beta \left\lVert \textbf{y} - \textbf{x}\right\rVert_2^2
 \\
 \nabla f(\textbf{y})^T(\textbf{y} - \textbf{x}) - \nabla f(\textbf{x})^T(\textbf{y} - \textbf{x}) &\le \beta \left\lVert \textbf{y} - \textbf{x} \right\rVert_2^2
 \label{eq:proof-4} \tag{14} \\
 \end{align}
 $$
 Equation $\eqref{eq:proof-4}$ can be written as,
 $$
 \left(f(\textbf{x}) - f(\textbf{y}) + \nabla f(\textbf{y})^T(\textbf{y} - \textbf{x})\right) + \left(f(\textbf{y}) - f(\textbf{x}) - \nabla f(\textbf{x})^T(\textbf{y} - \textbf{x})\right) \le \beta \left\lVert \textbf{y} - \textbf{x} \right\rVert_2^2
 \label{eq:proof-5} \tag{15} \\
 $$
 The Taylor expansion of a function $f(x)$ about a point $h$ is given by,
 $$
 f(x) = f(h) + \nabla f(h) (x - h) + \frac{\nabla^2 f(h)}{2!} (x - h)^2 + \ldots
 \label{eq:taylor-expansion} \tag{16} \\
 $$
 From equation $\eqref{eq:taylor-expansion}$, we have,
 $$
 \begin{align}
 f(x) &\ge f(h) + \nabla f(h) (x - h) \\
 f(x) - f(h) &\ge \nabla f(h) (x - h) \\
 f(h) - f(x) &\le \nabla f(h) (h - x)
 \label{eq:taylor-expansion-trunc} \tag{17} \\
 \end{align}
 $$
 From equation $\eqref{eq:proof-4}$ and $\eqref{eq:taylor-expansion-trunc}$, we get,
 $$
 \begin{align}
 f(\textbf{y}) - f(\textbf{x}) - \nabla f(\textbf{x})^T (\textbf{y} - \textbf{x}) &\le \beta \left\lVert \textbf{y} - \textbf{x} \right\rVert_2^2
 \label{eq:proof-6} \tag{18} \\
 f(\textbf{x}) - f(\textbf{y}) + \nabla f(\textbf{y})^T (\textbf{y} - \textbf{x}) &\le \beta \left\lVert \textbf{y} - \textbf{x} \right\rVert_2^2
 \label{eq:proof-7} \tag{19} \\
 \end{align}
 $$
 Subtracting equation $\eqref{eq:proof-6}$ from $\eqref{eq:proof-7}$, we get,
 $$
 \left(f(\textbf{x}) - f(\textbf{y}) + \nabla f(\textbf{y})^T (\textbf{y} - \textbf{x})\right)
 - \left(f(\textbf{y}) - f(\textbf{x}) - \nabla f(\textbf{x})^T (\textbf{y} - \textbf{x})\right)
 \le 0
 \label{eq:proof-8} \tag{20}
 $$
 Let 
 $$
 a = f(\textbf{x}) - f(\textbf{y}) + \nabla f(\textbf{y})^T (\textbf{y} - \textbf{x}),
 $$
 and
 $$
 b = f(\textbf{y}) - f(\textbf{x}) - \nabla f(\textbf{x})^T (\textbf{y} - \textbf{x}),
 $$
 Then, $\eqref{eq:proof-5}$ becomes,
 $$
 a + b \le \beta \left\lVert \textbf{y} - \textbf{x} \right\rVert_2^2,
 \label{eq:proof-9} \tag{21}
 $$
 and, $\eqref{eq:proof-8}$ becomes,
 $$
 a - b \le 0, 
 \label{eq:proof-10} \tag{22}
 $$
 Solving equation $\eqref{eq:proof-9}$ and $\eqref{eq:proof-10}$, we get,
 $$
 \begin{align}
 a &\le \frac{\beta}{2}\left\lVert \textbf{y} - \textbf{x} \right\rVert_2^2 
 \label{eq:sol-a} \tag{23} \\
 b &\le \frac{\beta}{2} \left\lVert \textbf{y} - \textbf{x} \right\rVert_2^2
 \label{eq:sol-b} \tag{24} \\
 \end{align}
 $$
 From equation $\eqref{eq:sol-a}$, 
 $$
 \begin{align}
 f(\textbf{x}) - f(\textbf{y}) + \nabla f(\textbf{y})^T (\textbf{y} - \textbf{x}) 
 \le \frac{\beta}{2} \left\lVert \textbf{y} - \textbf{x} \right\rVert_2^2 \\
 f(\textbf{x}) \le f(\textbf{y}) + \nabla f (\textbf{y})^T (\textbf{x} - \textbf{y}) + \frac{\beta}{2} \left\lVert \textbf{y} - \textbf{x} \right\rVert_2^2
 \end{align}
 $$
 Interchanging $\textbf{x}$ and $\textbf{y}$, we get,
 $$
 f(\textbf{y}) \le f(\textbf{x}) + \nabla f(\textbf{x})^T(\textbf{y} - \textbf{x}) + \frac{\beta}{2} \left\lVert \textbf{y} - \textbf{x} \right\rVert_2^2.
 $$
 And this concludes our proof of $(6)$.

---

**Proof of $\eqref{eq:lipschitz-2}$:**
 
 To prove $(8)$, we can rewrite $\eqref{eq:proof-2}$ as,
 $$
 \left\lVert \nabla f(\textbf{x}) - \nabla f(\textbf{y})\right\rVert_2\left\lVert \textbf{y} - \textbf{x}\right\rVert_2 \le \beta \left\lVert \textbf{y} - \textbf{x}\right\rVert_2^2.
 $$
 Then, again using Cauchy-Schwarz inequality as before,
 $$
 \nabla f(\textbf{x})^T(\textbf{y} - \textbf{x}) - \nabla f(\textbf{y})^T(\textbf{y} - \textbf{x}) \le \beta \left\lVert \textbf{y} - \textbf{x} \right\rVert_2^2
 $$
 If we solve it as before, then we obtain,
 $$
 \begin{align}
 f(\textbf{x}) - f(\textbf{y}) + \nabla f(\textbf{x})^T (\textbf{y} - \textbf{x}) 
 \le \frac{\beta}{2} \left\lVert \textbf{y} - \textbf{x} \right\rVert_2^2 \\
 f(\textbf{y}) \ge f(\textbf{x}) + \nabla f(\textbf{x})^T (\textbf{y} - \textbf{x}) - \frac{\beta}{2}\left\lVert \textbf{y} - \textbf{x} \right\rVert_2^2.
 \end{align}
 $$
 And this concludes our proof of $\eqref{eq:lipschitz-2}$.

---
**Proof of $\eqref{eq:lipschitz-3}$:**

 Let $\textbf{z} = \lambda \textbf{x} + (1 - \lambda) \textbf{y}$ for all $\lambda \in [0, 1]$. Then from $\eqref{eq:beta-smooth}$, we have,
 $$
 \begin{align}
 f(\textbf{x}) &\le f(\textbf{z}) + \nabla f(\textbf{z})^T(\textbf{x} - \textbf{z}) + \frac{\beta}{2} \left\lVert \textbf{x} - \textbf{z} \right\rVert_2^2
 \label{eq:proof-11} \tag{25} \\
 f(\textbf{y}) &\le f(\textbf{z}) + \nabla f(\textbf{z})^T(\textbf{y} - \textbf{z}) + \frac{\beta}{2} \left\lVert \textbf{y} - \textbf{z} \right\rVert_2^2
 \label{eq:proof-12} \tag{26} \\
 \end{align}
 $$
 Multiplying equation $\eqref{eq:proof-11}$ by $\lambda$ and equation $\eqref{eq:proof-12}$ by $(1 - \lambda)$, we get, 
 $$
 \begin{align}
 \lambda f(\textbf{x}) &\le \lambda f(\textbf{z}) + \lambda \nabla f(\textbf{z})^T(\textbf{x} - \textbf{z}) + \frac{\lambda \beta}{2} \left\lVert \textbf{x} - \textbf{z} \right\rVert_2^2
 \label{eq:proof-13} \tag{27} \\
 (1 - \lambda) f(\textbf{y}) &\le (1 - \lambda) f(\textbf{z}) + (1 - \lambda) \nabla f(\textbf{z})^T(\textbf{y} - \textbf{z}) + \frac{(1 - \lambda)\beta}{2} \left\lVert \textbf{y} - \textbf{z} \right\rVert_2^2
 \label{eq:proof-14} \tag{28} \\
 \end{align}
 $$
 Adding $\eqref{eq:proof-13}$ and $\eqref{eq:proof-14}$, we get,
 $$
 \begin{align}
 \lambda f(\textbf{x}) + (1 - \lambda) f(\textbf{y}) &\le f(\textbf{z})
 + \nabla f(\textbf{z})^T\left(\lambda (\textbf{x} - \textbf{z}) + (1 - \lambda)(\textbf{y} - \textbf{z})\right)
 + \frac{\lambda \beta}{2} \left\lVert \textbf{x} - \textbf{z} \right\rVert_2^2
 + \frac{(1 - \lambda) \beta}{2} \left\lVert \textbf{y} - \textbf{z} \right\rVert_2^2 \\
 \lambda f(\textbf{x}) + (1 - \lambda) f(\textbf{y}) &\le f(\textbf{z})
 + \nabla f(\textbf{z})^T \left(\lambda (1 - \lambda)(\textbf{x} - \textbf{y}) - (1 - \lambda)\lambda(\textbf{x} - \textbf{y})\right)
 + \frac{\lambda \beta}{2}\left\lVert (1 - \lambda)(\textbf{x} - \textbf{y})\right\rVert_2^2
 + \frac{(1 - \lambda) \beta}{2}\left\lVert \lambda(\textbf{x} - \textbf{y})\right\rVert_2^2 \\
 \lambda f(\textbf{x}) + (1 - \lambda) f(\textbf{y}) &\le f(\textbf{z})
 + \frac{\beta}{2} \left(\lambda(1 - \lambda)^2 + \lambda^2(1 - \lambda)\right)\left\lVert (\textbf{x} - \textbf{y})\right\rVert_2^2 \\
 \lambda f(\textbf{x}) + (1 - \lambda) f(\textbf{y}) &\le f(\textbf{z})
 + \frac{\lambda (1 - \lambda) \beta}{2} \left\lVert (\textbf{x} - \textbf{y})\right\rVert_2^2 \\
 f(\textbf{z}) &\ge \lambda f(\textbf{x}) + (1 - \lambda) f(\textbf{y})
 - \frac{\lambda (1 - \lambda) \beta}{2} \left\lVert (\textbf{x} - \textbf{y})\right\rVert_2^2 \\
 f(\lambda\textbf{x} + (1 - \lambda) \textbf{y}) &\ge \lambda f(\textbf{x}) + (1 - \lambda) f(\textbf{y})
 - \frac{\lambda (1 - \lambda) \beta}{2} \left\lVert (\textbf{x} - \textbf{y})\right\rVert_2^2 \\
 \end{align}
 $$
 And this concludes our proof of $\eqref{eq:lipschitz-3}$.

---
**Proof of $\eqref{eq:lipschitz-4}$:**

 The proof of $\eqref{eq:lipschitz-4}$ follows similarly starting from $\eqref{eq:lipschitz-2}$.
