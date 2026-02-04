---
title: Convex Smooth Functions.
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
