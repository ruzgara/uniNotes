---
date: 2026-02-03
updated: 2026-02-12T13:57:00
tags:
  - lessons/lecture
---
![[03-memory-and-linked-lists.pdf]]

# Abstract Data Types
A **type** is defined by:
- a set of possible **values**
- a set of possible **operations** on those values
An **abstract data type (ADT)** is a type whose internal representation is hidden to the user. Thus, users of an ADT may have no information about how the ADT is implemented, but depend only on the published information about how it behaves.
This means that the implementation of an ADT can be changed without having to change its usage.
The operations on an ADT may also have mathematically specified constraints, for example on the time complexity of the operations.

> [!example] Example: *Integers*
> An *Integer* is an ADT consisting of integer values with the operations `+`, `-`, `*`, `mod`, `div`, ...

## Lists
A **list** is an ordered collection of elements (values). For example:
$$\langle 2,5,17,1,8,7,23,1 \rangle$$
**Lists** are an **Abstract Data Type**. Its operations include:
- insert an entry at a position
- delete an entry
- get length
- access data by position
- concatenate two lists
- search an element in the list
- sort
- ...

Different representations of **Lists**: Depending on the operations required for an application, different types of List can be used. Some of them implement certain operations faster than others. These include:
- [[#Arrays]]
- [[#Dynamic Array]]
- Linked Lists
- Unrolled Linked Lists
- ...

> [!info]+ Lists in Java
> A specific instance of a List can be specified by the type of data it will store. For example `List<int>` or `List<String>`. There are different implementations of List in Java, for example `ArrayList<int>` or `LinkedList<int>`.
> Lists can be declared and allocated depending on the implementation required:
> ```java
> List<int> list1 = new ArrayList<int>();
> List<int> list2 = new LinkedList<int>();
> ```

### Arrays
The basic array type has a few operations:
```java
int[] nums = new int[4];    // array creation
value = nums[i];            // get value at index i
nums[i] = 23;               // replace value at index i
len = nums.length;          // get the length of the array
```

Arrays have to be defined with a fixed sized upon creation, in order to reserve memory. Resizing the array would require creating a new array and copying the contents.

More sophisticated List ADTs will often use basic arrays in their implementation, with additional complex operations such a variable sizing, sorting, concatenating etc.

> [!abstract] Data Structure Complexity
> Data Structures perform differently in terms of time and memory usage depending on the task. Measuring the performance is useful when choosing a structure to use. 
> > [!info] Recall
> > ![[Week 1 - Intro & Complexity#Types of complexities]]
> 
> **Amortised Complexity** is used to measure data structure complexity because it allows the measurement of efficiency over many operations, as spending some time or memory initially to rearrange or reorganising data may later lead to quicker operations.
> 

### Dynamic Array
A **Dynamic Array** functions like an array where the size is not predetermined, and can increase as required.
A Naive approach to implementing a **Dynamic Array** would be to:
1. Initially allocate an array of 128 entries
2. Whenever the array fills up, increase its size by 128

> [!NOTE] Measuring the **Worst Case complexity** of inserting values
> Assume $n$ entries, where $n = 128 + 128k, k \in \mathbb{Z}$ for simplicity.
> This will require:
> $$
> \begin{gathered}
> \begin{aligned}
> 128 \text{ insertions } &+ \\
> 128 \text{ copies } + 128 \text{ insertions } &+ \\
> 256 \text{ copies } + 128 \text{ insertions } &+ \\
> 384 \text{ copies } + 128 \text{ insertions } &+ \\
> 512 \text{ copies } + 128 \text{ insertions } &+
> \end{aligned}
> \\
> \vdots
> \end{gathered}
> $$
> Therefore, there will be:
> $$
> \begin{alignedat}{2}
> \text{insertions:} &\qquad& 128 + 128k &= n \\
> \text{copies:}     &\qquad& 128 (1+2+\dots+k) 
>                     &= 128\cdot \frac{k(k+1)}{2} 
>                     = 64k(k+1)
> \end{alignedat}
> $$
> In total: 
> $$ \begin{aligned}
> 128(k+1) + 64k(k+1) &= (64k + 128)(k+1)  \text{ operations }\\
> &= O(n^2)
> \end{aligned}
> $$ 


> [!NOTE] Measuring **Amortised complexity** of inserting one value at the end
> The cost of one insertion will be
> $$
> \frac{\text{Total Operations}}{\text{Insertion Operations}} =
> \frac{64k(k+1) + 128(k+1)}{128(k+1)} =
> \frac{1}{2}k + 1 = O(n)
> $$
> Therefore the amortised complexity of insertion is $O(n)$

As insertion is required to add data to the array, improving the time complexity of Dynamic Arrays requires minimising the number of copying operations.

A Better approach to **Dynamic Arrays** is:
1. Initially allocate an array of 128 entries
2. Whenever the array fills up, **double** its size

> [!NOTE] Measuring the **Worst Case complexity** of inserting values
> Assume $n$ entries, where $n = 128 \times 2^k, k \in \mathbb{Z}$ for simplicity.
> This will require:
> $$
> \begin{gathered}
> \begin{aligned}
> 128 \text{ insertions } &+ \\
> 128 \text{ copies } + 128 \text{ insertions } &+ \\
> 256 \text{ copies } + 256 \text{ insertions } &+ \\
> 512 \text{ copies } + 512 \text{ insertions } &+ \\
> 1024 \text{ copies } + 1024 \text{ insertions } &+
> \end{aligned}
> \\
> \vdots
> \end{gathered}
> $$
> Therefore, there will be:
> $$
> \begin{alignedat}{2}
> \text{insertions:}     &\qquad& 128+ 128(1+2+4+\dots+2^{k-1}) 
>                     &= 128 \times 2^k
>                     = n \\
> \text{copies:} &\qquad& 128(1+2+4+\dots+2^{k-1}) &= n - 128
> \end{alignedat}
> $$
> In total: 
> $$ \begin{aligned}
> n+n-128 &\leq 2n  \text{ operations }\\
> &= O(n)
> \end{aligned}
> $$ 

> [!NOTE] Measuring **Amortised complexity** of inserting one value at the end
> The cost of one insertion will be
> $$
> \frac{\text{Total Operations}}{\text{Insertion Operations}} =
> \frac{2n-128}{n} =
> 2 - \frac{128}{n} \leq 2 = O(1)
> $$
> Therefore the amortised complexity of insertion is $O(1)$

The second method is much more time efficient:
For filling the array:

|          | Worst Case |
| -------- | ---------- |
| Method 1 | $O(n^2)$   |
| Method 2 | $O(n)$     |
For inserting at the end:

|          | Best Case | Worst Case | Amortised |
| -------- | --------- | ---------- | --------- |
| Method 1 | $O(1)$    | $O(n)$     | $O(n)$    |
| Method 2 | $O(1)$    | $O(n)$     | $O(1)$    |
