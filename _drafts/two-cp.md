---
title: "Why Two's Complement Works?"
date: 2021-03-20
categories: c++
permalink: "cpp/two-s-complement"
---

## A Refresher in Binary Representation of Positive Integers

Most of us are familiar with the concept that computers store data in binary forms (0 and 1), and that integers can be represented in binary as well.

In a decimal system, we use the digits 0-9, and whenever we add 1 to a number, we add 1 to the least significant (i.e the right-most) digit. If that still falls in the range 0-9, we are done. Otherwise, it is 10, then we set that digit to 0, and add repeat the same process on all the digits to its left.

Example:



$$
\begin{gather*}
42 + 1 \to 4\boxed{2+1} \to 43 \\ 
19 + 1 \to 19\boxed{9+1} \to \boxed{1+1}0 \to 20 \\ 
99 + 1 \to 9\boxed{9+1} \to \boxed{9+1}0 \to 100
\end{gather*}
$$

