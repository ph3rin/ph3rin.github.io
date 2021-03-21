---
title: "Why Two's Complement Works?"
date: 2021-03-20
categories: c++
permalink: "cpp/two-s-complement"
---

;; Intended audience: Explain "Why 2-s-complement work" to C++ programmers who already know the basic binary notation, potentially know binary arithmetic, but do not think in terms of modular arithmetic.

## Basic binary notation

We know that every integer[^1] can be written **uniquely** as:

$$d_n \times 2^n + d_{n-1} \times 2^{n-1} + \ldots + d_0 \times 2^0$$

where each $d$ is either $0$ and $1$. For example, 42 can be written as:

$$\boxed{1} \times 2^5 + \boxed{0} \times 2^4 + \boxed{1} \times 2^3 + \boxed{0} \times 2^2 + \boxed{1} \times 2^1 + \boxed{0} \times 2^0$$

The sequence of the bits (a digit that is either 0 or 1) in the boxes is then the binary representation for 42: $$101010$$.

## Binary Arithmetic

The method used for adding decimal numbers still apply in the binary world, except the "carrying" happens whenever the sum is $\geq 2$. Here is an example:

$$
\begin{array} {rr} & & & \\ & 1 & 1 & 0 \\  + & 1 & 1 & 1 \\ \hline & & &  \\ \end{array} \to

\begin{array} {rr} & & & \\ & 1 & 1 & 0 \\  + & 1 & 1 & 1 \\ \hline & & &1  \\ \end{array} \to

\begin{array} {rr} &_1& & \\ & 1 & 1 & 0 \\  + & 1 & 1 & 1 \\ \hline & &0 &1  \\ \end{array}  \\ \to

\begin{array} {rr} _1& & & \\ & 1 & 1 & 0 \\  + & 1 & 1 & 1 \\ \hline &1 &0 &1  \\ \end{array} \to

\begin{array} {rr} & & & \\ & 1 & 1 & 0 \\  + & 1 & 1 & 1 \\ \hline 1&1 &0 &1  \\ \end{array} 

$$

Multiplication is significantly simpler in the binary world. If we want to multiply that $110$ and $101$, we notice that:

$$
\begin{aligned}
110 \times 101 
&= 110 \times (1 \times 100 + 0 \times 10 + 1 \times 1) \\
&= 1 \times 110 \times 100 + 0 \times 110 \times 10 + 1 \times 110 \times 1 \\
&= 11000 + 0 + 110 = 11110
\end{aligned}$$

Here we leverage the fact that multiplying by a number of the form $10\ldots0$ is easy: we just append the 0-s to the right. In C++, this is done by a the left shift (`<<`) operator.

## Unsigned integers and modular arithmetic

The above rule can work for arbitarily large integers, but we programmers usually only care about integers that fits in a certain number of bits (e.g 16 and 32 bits). To keep the discussion simple, let us assume that our integers are stored in 8-bit units.

We first study the case of non-negative integers. (And since they are 8-bits, they are just `uint8_t`).

To represent an unsigned integer in 8 bits, we simply write down its binary notation. If there less than 8 bits in the notation, we append 0-s to the left to fill the gap. For example, $42$ has $6$ bits, so we append two 0-s to the left to make it 8 bits:

$$101010 \to 00101010$$

The aforementioned method for addition and multiplication still works here, but with a twist: **What if the result of addition or multiplcation doesn't fit in 8 bits** (i.e $\geq 256$)?

## Dealing with Negative Numbers

Things becomes tricky when we start dealing with negative numbers. When we humans write negative numbers, we just stick a negative sign ("-") in front of a number to indicate that it is negative.

Arguably we could do the same thing in computers. We could first ignore the sign, and write down the number using the binary notation of a non-negative numbers, then add a bit to the left which represents the sign: if the sign is "-", we use $1$, otherwise we use $0$.

Take $-42$ for example. We first ignore the sign, and represent $42$ in binary, which gives us $101010$. Then since the sign is negative, we add a $1$ to the left: $\boxed{1}101010$.

[^1]: Or more precisely, non-negative integers.
