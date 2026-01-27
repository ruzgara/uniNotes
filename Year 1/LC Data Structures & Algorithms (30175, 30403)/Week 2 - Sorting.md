![[02-sorting.pdf]]
### Basics
Given a set of records or objects, these can be sorted by a criteria. Eg: 
- Decreasing score
- Alphabetic by surname then first name
- Increasing order of name then decreasing order of mark

Sorting algorithms mostly work on the basis of a comparison function that is supplied that defines the order required between any two objects or records.
These algorithms are **comparison based**
This means they do not depend on what is being sorted, but only compare elements `x` and `y`:
> `x < y` or `x = y` or `x > y`

This means that `int`, `float`, `str` (alphabetically), `char` (by ascii value) etc can all be sorted.

In some special cases, the nature of the data allows for sorting without a comparison function.

> [!example]- Comparing in Java
> Java provides two interfaces to implement comparison functions:
> - `Comparable`: A `Comparable` object can compare itself with another object using its `compareTo(...)` method. There can only be one such method for any class, so it should implement the default ordering of objects.
> 	- `x.compareTo(y)` should return a negative `int`, 0, or a positive `int` if `x` is less than, equal to, or greater than `y` respectively.
> - `Comparator`: A `Comparator` object can be used to compare two objects of some type using the `compare(...)` method. This does not work on the current object, but rather both objects to be compared are passed as arguments. There can be many different comparison functions implemented this way.
> 	- `compare(x, y)` should return a negative `int`, 0, or a positive `int` if `x` is less than, equal to, or greater than `y` respectively.

---
## Sorting Algorithms

### Bubble Sort
- Bubble sort does multiple passes over an array of items, swapping neighbouring items that are out of order as it goes.
- Each pass guarantees that at least one extra element ends up in its correct ordered location at the start of the array, so consecutive passes shorten to work only on the unsorted part of the array, until the last pass only sorts the remaining 2 elements at the end.

> [!example] Pseudocode
> 
