---
title: Expectation
description: About expectation.
date: '2025-12-10'
categories:
    - probability
published: true
---

## Random Variable
A random variable can be thought of as a function which assigns a real number to events in a sample space. 

Consider a coin toss. The sample space consists of two possible outpcomes: $S = \{H, T\}$. Let $X$ be the number of Heads. This is a random variable with possible values $0$ and $1$, i.e.
$$
X(H) = 1, X(T) = 0
$$

The probability of an outcome is described in terms of the probability of a random variable taking a given value. For example, $P(X = 1) = 1 / 2$.

Random variables can be either discrete or continuous.

## Probability Distribution

A **probability distribution** is a function that gives the likelihood of different possible outcomes for a random variable.

### Probability Mass Function

The **probability mass function** (PMF) of a discrete random variable $X$ is the function $p_X$  given by $p_X(x) = P(X = x)$. That is, it is a function that gives the probability that the random variable $X$ will take some value $x$. A PMF must satisfy the following criteria:

1. $p_X(x) \ge 0$ for all $x \in X$.
2. $\sum_k p_X(x_k) = 1$

### Cumulative Distribution Function

The distribution of a random variable can also be defined using the **cumulative distribution function** (CDF). The CDF is defined for all random variables: discrete as well as continuous.

The **cumulative distribution function** (CDF) of a random variable is the function $F_X$ given by $F_X(x) = P(X \le x)$ with following properties:

1. *Increasing:* If $x_1 \le x_2$, then $F_X(x_1) \le F_X(x_2)$.
2. *Right-continuous:* Except for possibly having jumps, the CDF is continuous. Wherever there is a jump, the CDF is continuous from the right. That is,
    $$
    F_X(a) = \lim_{x \to a^+} F_X(x)
    $$
3. Convergence to 0 and 1 in the limits: 
    $$
    \lim_{x \to -\infty} F_X(x) = 0 \text{ and } \lim_{x \to \infty} F_X(x) = 1
    $$

### Probability Density Function

The **probability density function (pdf)** of a continuous random variable $X$ with CDF $F$ is the derivative $f$ of the CDF, given by $f(x) = F'(x)$, and satisfies the following criteria:

1. $f(x) \ge 0$
2. $\int_{-\infty}^{\infty} f(x) dx = 1$

## Function of a Random Variable

A function of a random variable is also a random variable.

Consider an experiment with a sample space $S$, a random variable $X$, and a function $g : \mathbb{R} \rightarrow \mathbb{R}$. Then, $g(X)$ is a random variable that maps $s$ to $g(X(s))$ for all $s \in S$.

Let $X$ be a discrete random variable and $g : \mathbb{R} \rightarrow \mathbb{R}$, then the PMF of $g(X)$ is,
$$
P(g(X) = y) = \sum_{x \in g^{-1}(y)} P(X = x)
\label{eq:pmf_of_g(x)} \tag{1}
$$

Let $X$ be a continuous random variable and $Y = g(X)$ be a function of $X$. Then, the CDF of $Y$ is given by,
$$
\begin{align}
F_Y(y) &= P(Y \le y) \\
&= P(g(X) \le y) \\
&= P(X \le g^{-1}(y)) \\
&= F_X(g^{-1}(y))
\label{eq:cdf_of_y_cont} \tag{2}
\end{align}
$$
where, $F_X$ is the CDF of $X$.

Now, the PMF of $Y$ is given by,
$$
\begin{align}
f_Y(y) &= \frac{d}{dy}F_X\left(g^{-1}(y)\right) \\
&= f_X\left(g^{-1}(y)\right)\frac{d}{dy}g^{-1}(y) \\
&= f_X\left(g^{-1}(y)\right)\frac{1}{g'\left(g^{-1}(y)\right)}
\label{eq:pdf_of_y} \tag{3}
\end{align}
$$

## Expectation

The distribution of a random variable gives all the information about the probability that a random variable will fall into any particular set. However, it would be better if we had just one number summarizing the *average* value of the random variable.

The average (or mean) of a random variable is known as its **expected value**. 

The **expected value** of a discrete random variable $X$ with distinct possible values $x_1, x_2, \ldots$, and PMF $P$ is defined as
$$
\mathbb{E}[X] = \sum_{k = 1}^\infty x_k P(X = x_k)
\label{eq:expectation_drv_inf} \tag{4}
$$

If the possible values are finite then it is defined as
$$
\mathbb{E}[X] = \sum_x x P(X = x)
\label{eq:expectation_drv} \tag{5}
$$

The **expected value** of a continuous random variable $X$ with PDF $f$ is defined as
$$
\mathbb{E}[X] = \int_{-\infty}^\infty x f(x) dx
\label{eq:expectation_crv} \tag{6}

$$

Thus, the **expectation** of a random variable $X$ is the weighted average of its possible values, weighted by their probability of occurrence.

## Indicator Random Variable

For an event $A$, the **indicator random variable**, $I_A$ or $I(A)$ is $1$ if $A$ occurs, and $0$ otherwise:
$$
I_A = \begin{cases}
1 & \text{if $A$ occurs}, \\
0 & \text{otherwise}
\label{eq:irv} \tag{7}
\end{cases}
$$

The probability of an event $A$ can be expressed as the expectation of the indicator random variable of $A$:
$$
P(A) = \mathbb{E}(I_A)
\label{eq:fundamental_bridge} \tag{8}
$$

## Law of Unconscious Statistician

The expected value of a function of a random variable $X$, $g(X)$, can be determined using the **Law of Unconscious Statistician** (LOTUS).

### For Discrete Random Variable

Let $X$ be a discrete random variable with probability mass function $f_X(x)$, then the expected value of $g(X)$ is
$$
\mathbb{E}[g(X)] = \sum_x g(x) f_X(x)
\label{eq:lotus_drv} \tag{9}
$$

> **Proof**
>
> Let $Y = g(X)$ and $f_Y$ be its PMF. Then,
> $$
> \begin{align}
> \mathbb{E}[Y] &= \sum_y y f_Y(y) \\
> &= \sum_y y P\left(Y = y\right) \\
> &\stackrel{\eqref{eq:pmf_of_g(x)}}{=} \sum_y y \sum_{x \in g^{-1}(y)} P(X = x) \\
> &= \sum_y \sum_{x \in g^{-1}(y)} y  P(X = x) \\
> &= \sum_x g(x) f_X(x)
> \end{align}
> $$

### For Continuous Random Variable

Let $X$ be a continuous random variable with probability mass function $f_X(x)$, then the expected value of $g(X)$ is
$$
\mathbb{E}[g(X)] = \int g(x) f_X(x) dx
\label{eq:lotus_crv} \tag{10}
$$

> **Proof**
>
> Let $Y = g(X)$ with probability density function $f_Y$. Then,
> $$
> \begin{align}
> \mathbb{E}[Y] &= \int y f_Y(y) dy \\
> &\stackrel{\eqref{eq:pdf_of_y}}{=} \int y f_X(x) \frac{1}{g'(x)} g'(x) dx \\
> &= \int g(x) f_X(x) dx \\
> \end{align}
> $$

## Conditional Expectation

## Law of Total Expectation

## References

- [Introduction to Probability by Blitzstein and Hwang](https://drive.google.com/file/d/1VmkAAGOYCTORq1wxSQqy255qLJjTNvBI/edit)
- [The Book of Statistical Proofs](https://statproofbook.github.io/P/mean-lotus.html)