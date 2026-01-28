---
date: 2026-01-27
updated: 2026-01-28T22:06:00
tags:
  - lessons/lecture
---
![[02-sorting.pdf]]

# Sorting Basics
Given a set of records or objects, these can be sorted by a criteria. Eg: 
- Decreasing score
- Alphabetic by surname then first name
- Increasing order of name then decreasing order of mark

Sorting algorithms mostly work on the basis of a **comparison function** that is supplied that defines the order required between any two objects or records.
These algorithms are **comparison based**
This means they do not depend on what is being sorted, but only compare elements $x$ and $y$: 
$$x <y \quad \text{or} \quad  x = y \quad \text{ or } \quad x>y$$

This means that `int`, `float`, `str` (alphabetically), `char` (by ascii value) etc can all be sorted.

In some special cases, the nature of the data allows for sorting without a comparison function.

> [!info]+ Comparing in Java
> Java provides two interfaces to implement comparison functions:
> - `Comparable`: A `Comparable` object can compare itself with another object using its `compareTo(...)` method. There can only be one such method for any class, so it should implement the default ordering of objects.
> 	- `x.compareTo(y)` should return a negative `int`, 0, or a positive `int` if `x` is less than, equal to, or greater than `y` respectively.
> - `Comparator`: A `Comparator` object can be used to compare two objects of some type using the `compare(...)` method. This does not work on the current object, but rather both objects to be compared are passed as arguments. There can be many different comparison functions implemented this way.
> 	- `compare(x, y)` should return a negative `int`, 0, or a positive `int` if `x` is less than, equal to, or greater than `y` respectively.

---
# Sorting Algorithms

## Bubble Sort
- Bubble sort does multiple passes over an array of items, swapping neighbouring items that are out of order as it goes.
- Each pass guarantees that at least one extra element ends up in its correct ordered location at the start of the array, so consecutive passes shorten to work only on the unsorted part of the array, until the last pass only sorts the remaining 2 elements at the end.

> [!example]+ Pseudocode
> ```pseudo
> \begin{algorithm}
> \caption{Bubble Sort}
> \begin{algorithmic}
> 	\Procedure{BubbleSort}{array $a$}
> 	\State $n \gets \text{length}(A)$
> 	\While{$a$ is not sorted}
> 		\For{$i \gets 0$ to $n-2$}
> 			\If{$a[i] > a[i+1]$}
> 				\State Swap $a[i]$ and $a[i+1]$
>             \EndIf
>         \EndFor
>     \EndWhile
>     \EndProcedure
> \end{algorithmic}
> \end{algorithm}
> ```

### Bubble Sort Complexity
- The inner for-loop is iterated $n-1$ times, with each iteration executing a single comparison.
- After each iteration of the while, one more element is in its correct spot, thus the algorithm finishes in $n-1$ iterations of the while-loop.
- Therefore the time complexity can be calculated as:
$$(n-1) \times (n-1) = n^2 - 2n +1 = O(n^2)$$

## Selection Sort
- Works by **selecting** the smallest remaining element of an input array and appending it at the end of all the elements that have been inserted thus far.
- It does this by partitioning the array into a sorted part at the front, and an unsorted part at the end.
- In each pass, it finds the smallest element in the unsorted part, and swaps it with the first element of the unsorted part of the array. Then it moves the split position between the sorted and unsorted parts of the array by one cell.

> [!example] Pseudocode
>  ```pseudo
> \begin{algorithm}
> \caption{Selection Sort}
> \begin{algorithmic}
> \Procedure{SelectionSort}{$A$}
>     \State $n \gets \text{length}(A)$
>     \For{$i \gets 0$ to $n - 2$}
>         \State $minIndex \gets i$
>         \For{$j \gets i + 1$ to $n - 1$}
>             \If{$A[j] < A[minIndex]$}
>                 \State $minIndex \gets j$
>             \EndIf
>         \EndFor
>         \If{$minIndex \neq i$}
>             \State swap $A[i]$ and $A[minIndex]$
>         \EndIf
>     \EndFor
> \EndProcedure
> \end{algorithmic}
> \end{algorithm}
> ```

### Selection Sort Complexity
- The outer loop is iterated $n-1$ times
- In the worst case, the inner loop is iterated $n-1$ times for the first outer loop iteration, $n-2$ times for the 2nd outer iteration, etc. Thus, the total number of comparisons is:
$$
\begin{aligned}
\sum_{i=0}^{n-2}\sum_{j=i+1}^{n-1}1 &= \sum_{i=0}^{n-2}(n-1-i)\\
	&= (n-1)+\cdots+2+1\\
	&= \frac{n(n-1)}{2}
\end{aligned}
$$
- Hence, the worst case complexity is: $O(n^2)$
