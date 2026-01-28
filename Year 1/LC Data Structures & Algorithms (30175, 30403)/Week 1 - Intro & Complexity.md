---
date: 2026-01-20
updated: 2026-01-27T18:57
tags:
  - lessons/lecture
---
![[01-efficiency-and-complexity.pdf]]

# Intro
Programs are composed of Algorithms and Data Structures:
- **Data Structures** efficiently organise data in computer memory
- **Algorithms** manipulate data structures to achieve a given goal

In order for a program to terminate fast (or in time) in has to use *appropriate* data structures and *efficient* algorithms
This module focuses on:
- various data structures
- basic algorithms
- understanding the strengths & weaknesses in terms of time and space complexities

Learning Objectives:
- Design and implement DS&A`
- Argue that algorithms are correct, derive time and space complexity measure for them
- Explain and apply data structures in solving programming problems
- Make informed choices between alternative data structures, algorithms and implementations, justifying choices on grounds such as computational efficiency

### Assessment
- Continuous assessment (20%):
	- Theory assessment in Week 3 (8%)
	- Coding assessment 1 in Week 7 (6%)
	- Coding assessment w in Week 10 (6%)
- Exam (80%):
	- 2 hour pen and paper exam in summer exam period. Calculators allowed
	- Algorithms written using pseudocode, some Java.

# Complexity & Efficiency
Time taken to execute a function is measured in basic steps such as: additions, multiplications, memory accesses etc.)
It is accepted that this will be off by a constant factor. 

## Big-O notation
The precise number of steps is often too detailed to be useful

> [!example] Example
> If an algorithm does a comparison and a multiplication on each element in an array of length $n$, some processors/compilers may do the comparison and multiplication:
> - Together in **one** step, leading to a complexity of $n$
> - One-by-one, in **two** steps, leading to a complexity of $2n$
>
> The difference between a time complexity of $n$ and $2$ is minuscule in comparison to the difference between $n$ and $n^2$
> 
> Similarly, for an algorithm with complexity $n^2 + n$, the $n$ makes a small contribution

Often times, measuring how an algorithm **scales** with the input size is more important than the actual value of the complexity

The number of steps is measured up to a constant factor, independent of the machine. 
Often times the upper bound is used:
This is presented as:
$$
f(n) \leq C \cdot g(n)\ \text{for some constant} \ C > 0
$$
$C \cdot g(n)$ in this case refers to multiplying each element in $g(n)$ by a constant $C$

> [!example] Example:
> If $g(n) = x^2 + x$ , then $C \cdot g(n) = C_0x^2 + C_1x$

Instead of writing $f(n) \leq C \cdot g(n)$ for some $C > 0$, we use the **Big-O** notation.
### Definition:
We write $f(n) \in O(g(n))$ if
$$
\text{there exists constants} \ C, n_0 >0 \ \text{such that for all} \ n \geq n_0,\ |f(n)| \leq |C \cdot g(n)|
$$
Formally, $O(g(n))$ is a *set of functions*. The set contains all functions that grow at a faster rate than $g$
Informally, we may treat the expression $f(n) \in O(g(n))$ as a comparison of functions $f(n) \preccurlyeq g(n)$, meaning $f$ does not grow at a quicker rate than $g$

> [!warning] Abuse of notation
> It is common to write $f(n) = O(g(n))$ instead of $f(n) \in O(g(n))$ which is an *abuse of notation*. 
> Therefore, statements such as $3n^2 + 4 = O(n^2)$ is used as a shorthand for $3n^2 + 4 \in O(n^2)$ which in turn means 
> $$
> 3n^2 + 4 \leq C_0n^2 \ \text{for all}\ n \geq n_0 \ \text {for some}\ C > 0
> $$
> Or:
> $$
> \exists C > 0,\forall n \geq n_0, 3n^2 + 4 \leq Cn^2
> $$


> [!info] Intermission: Logarithms
> Definition of the logarithmic function:
> $$\log_ba = x \Leftrightarrow b^x = a$$
> Identities:
> $$\log_bx + \log_by = \log_b(x \cdot y)\quad \log_ax = \frac{\log_bx}{\log_ba} = (\log_ab) \cdot (\log_bx) \quad y^x = 2^{x\log_2y}$$
> In this module (and **CS** more broadly), the base will be $2$ by default: $\log n \Leftrightarrow \log_2n$

### Big-O and Others

There are others like Big-O:
- **Big-O** $f(n) = O(g(n))$: $g$ is an **upper bound** on how fast $f$ grows as $n$ increases
$$f \in O(g) \Leftrightarrow \exists C, \exists n_0 > 0, \forall n > n_0,\ |f(n)| \leq C|g(n)|$$
- **Omega** $f(n) = \Omega(g(n))$: $g$ is a **lower bound** on how fast $f$ grows as $n$ increases
$$f \in \Omega(g) \Leftrightarrow \exists C, \exists n_0 > 0, \forall n > n_0,\ |f(n)| \geq C|g(n)|$$
- **Theta** $f(n) = \Theta(g(n))$: $g$ is **both the upper and lower bound**, except with different constant factors. i.e., $f$ and $g$ grow at the same rate
$$f \in \Theta(g) \Leftrightarrow \exists C_0, \exists C_1, \exists n_0 > 0, \forall n > n_0,\ C_0|g(n)| \leq |f(n)| \leq C_1|g(n)|$$

> [!NOTE] Note
> Since $\Omega$ is the lower-bound, and $O$ is the upper bound, $f \in \Omega(g) \Leftrightarrow g \in O(f)$
> $f\in \Theta(g) \rightarrow f \in \Omega(g) \text{ and } f \in O(g)$

## Worst case complexity
An algorithm can take different amounts of steps on two inputs of the same size n.
Usually, the **upper bound of the complexity** is used when denoting the time complexity, i.e. **worst case complexity**.

> [!example] Example: Linear Search
> *Linear search is an algorithm to find a value in an array, by comparing each item in the array against the value, until a match is found or the array ends*
> For this algorithm, the **worst case** time complexity is when the value does not appear in the array.
> In this case, there will be $n$ comparisons for an array of size $n$, therefore it can be said that the algorithm has time complexity of $O(n)$.


> [!example] Example: Binary Search
> *Binary search is an algorithm to find a value in a sorted array, by recursively comparing the value against the middle value of the array, then removing whichever side of the array the value is not in; until the value is found, or there is no items left in the array*
> For this algorithm, the **worst case** time complexity is when the value does not appear in the array.
> In this case, the number of comparisons will be the number of times the array can be halved, which can be found out by $\log_2n$ (assuming n is a factor of 2 for simplicity, if not it will be $\lceil \log_2n \rceil$)
> Therefore, this algorithm has time complexity of $O(\log n)$

### Types of complexities
- Worst Case
	- Worst complexity over all possible inputs/situations (complexity upper bound)
- Average
	- Average complexity over all random choices
	- For randomised algorithms, or algorithms where the likelihood of each of the inputs are known
- Amortised
	- Average time taken over a sequence of consecutive operations
	- used for measuring data structure performance