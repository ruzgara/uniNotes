---
date: 2026-02-03
updated: 2026-07-26T20:00:00
tags:
  - lessons/lecture
---
![[03-memory-and-linked-lists.pdf#page=24|03-memory-and-linked-lists, p.13]]

# Memory Basics

- Data is stored, managed and manipulated in computer memory.
- The hardware and the operating system allow each program to see an illusion that they have the whole memory to themselves.
- The memory is organised as a long list of memory cells, each 1 byte = 8 bits large. Each of the cells have an address, from 0 to the highest possible address for the system (eg 64 bit systems have the max address `0xFFFFFFFFFFFFFFFF`).
- The hardware usually manipulates memory a word at a time (4 byes for 32 bit systems, and 8 bytes for 64 bit systems)

# Memory Management

Computers cannot hold such gigantic amounts of memory, and if a program were to try and access each of those bytes of memory, it would run out and an error would be triggered.

To support the illusion, programs have to explicitly request from the OS to add large sections of the memory to their memory space via *OS calls*.

However, programs must manage their own memory with finer control than the expensive OS calls.

The *Runtime System*, a library integral to each programming language, typically provides one of two options:

1. [[#Explicit Memory Management]], with `allocate` and `free` functions.
2. [[#Implicit Memory Management]] with Garbage Collection.

## Explicit Memory Management

Examples of language with explicit memory management are C and C++.
C's runtime system maintains it's own data structure to organise the memory it has obtained from the OS. This is usually accessed by two functions:
- `malloc()`: Allows the program to request a contiguous amount of memory. If available, it is recorded into the memory management data structure, and the address of the start of the block is returned to the program. Otherwise, a system call requests more memory. If this call is successful, the block is added to the structure, and continues as before. Otherwise, it returns an error.
- `free()`: Takes in the address of a previously allocated block, and marks it as available for future allocation.

Explicit Memory Management is simple and efficient. However it suffers from some disadvantages:
- The program must keep track of every requested block in order to free it later. Otherwise the program will have a ***memory leak***: a block of allocated memory that cannot be used or freed. This causes the program to use more memory than it needs, causing performance problems and possible crashes if it runs out of memory.
- If an error is made by giving an address to `free()` which was not allocated previously by `malloc()`, or one that had already been freed, than the memory management structure can become corrupted, causing unpredictable errors or crashes.
## Implicit Memory Management

Implicit Memory Management more sophisticated: the language itself, via its runtime system, provides mechanisms to allocate data structures without exposing memory addresses to programmers. It also identifies allocated structures that can no longer be accessed by the running program and adds them back into its data structure of available free memory without requiring the program to specifically ask for the memory to be freed.

Examples of languages with implicit memory management are Java, Python, Haskell, OCaml and many more.

It relieves the burden of keeping track of allocated memory from the programmer, and avoids many of the bugs and problems that are caused by address manipulation in explicit memory management languages.

# Arrays in Memory

Arrays can be stored in many different ways within memory.

## Simple Arrays
```java
int[] nums = new int[4];
for (int i = 0; i<nums.length; i++)
	nums[i] = i * 10;
```
This code allocates 4 sets of 4 bytes of memory, and assigns values to them.
The memory layout can be described as such:
![[03-memory-and-linked-lists.pdf#page=30&rect=76,77,227,120|03-memory-and-linked-lists, p.19]]
In this example, a pointer is created that points to the memory location of the first element of the array, and 4 locations are filled with the values:
![[03-memory-and-linked-lists.pdf#page=30&rect=297,79,431,250&width=200|03-memory-and-linked-lists, p.19]]
It is assumed that the word size is 4 bytes (32 bits), and integers and pointers are also 4 bytes in size.

- `nums` is a variable stored at address 3100. The contents of the cell is address 3324, which is whee the array starts.
- Every `int` in the array occupies one word or 4 bytes in memory.
- Because the array is declared as an array of integers, the compiler knows that every entry in the array takes 4 byres, and if the start location is 3324, then it knows that:
	- `nums[0]` is at $3324$
	- `nums[1]` is at $3324 + (1 \times 4)$
	- `nums[2]` is at $3324 + (2 \times 4)$
	-  $\ldots$

## More complicated Arrays

A java class can be used to collect variables together into a single structure.
```java
class Point {
	float x;
	float y;
}
Point[] locations = new Point[3];
locations[1].x = 25.2;
locations[1].y = 38.6;
```
![[03-memory-and-linked-lists.pdf#page=32&rect=293,111,413,249&width=200|03-memory-and-linked-lists, p.20]]
 
 Given that a float is 4 bytes, the following happens in memory:
- cell at the address `locations` $+\:(1 \times 2 \times 4) + (0 \times 4)$ is set to $25.2$.
- cell at the address `locations` $+\:(1 \times 2 \times 4) + (1 \times 4)$ is set to $38.6$.
The $1 \times 2 \times 4$ is because, $1$ is the index into locations, $2$ is the number of words in a `Point` object, and $4$ is the size of the word.
sd

> [!WARNING] Array Overflows
> A common error is to try and access the last cell in an array incorrectly:
> ```java
> int[] a = new int[5];
> a[5] = 1000;
> ```
> This will cause an `ArrayIndexOutOfBoundsException` in Java whereas in C or C++, this would go through without a warning and could lead to corruption of data in memory.

# Linked Lists

Linked Lists store data of a particular type by using structures called *nodes*. The nodes can be stored in different locations in memory. They each have a ==*value*== variable which holds the data, and one or more ==*node*== variables to identify the next node in the list.

An advantage of Linked Lists over arrays is that their length is not fixed. It is possible to insert and delete items as required. However, they are less efficient in terms of time complexity and memory usage. For example, accessing an entry in a specific position requires traversing the list.

A Linked List is then a collection of Node structures each connected to others in a chain.

> [!INFO] Linked List definition in Java
> ```java
> class Node {
> 	int val;
> 	Node next;
> }
> Node list = null;
> ```
> No `Node` has yet been allocated therefore the list is empty `null` is a special value representing an impossible memory address.

## Linked Lists in Memory

A Linked List representing $[93, 23, 12, 53]$ can be represented by the following diagram:
![[03-memory-and-linked-lists.pdf#page=36&rect=327,29,424,253&width=200|03-memory-and-linked-lists, p.22]]
In memory, each node is stored in different blocks of memory, each of them two words long.

Linked List operations can be implemented as folllows:

> [!INFO]+ Inserting at the beginning of a Linked List
> ```java
> void insert_beginning(Node list, int value) {
> 	newNode = new Node();
> 	newNode.val = value;
> 	newNode.next = list;
> 	list = newNode;
> }
> ```
> This operation has a time complexity of $O(1)$, as the number of operations is independent of the size of the list

> [!INFO]+ Deleting at the beginning of a Linked List
> ```java
> boolean is_empty(Node list) {
> 	return (list == null);
> }
> 
> void delete_beginning(Node list) throws EmptyListException {
> 	if is_empty(list)
> 		throw new EmptyListException;
> 	list = list.next;
> }
> ```
> This operation has a time complexity of $O(1)$, as the number of operations is independent of the size of the list

> [!INFO]+ Lookup in a Linked List
> ```java
> int value_at(Node list, int index) throws OutOfBoundsException {
> 	int i = 0;
> 	Node nextnode = list;
> 	while (true) {
> 		if (nextnode == null)
> 			throw new OutOfBoundsException();
> 		if (i == index)
> 			break;
> 		nextnode = nextnode.next;
> 		i++;
> 	}
> 	return nextnode.val
> }
> ```
> It is required to traverse the list one node at a time until it arrives at the index which was requested. This is one of the drawbacks of using a Linked List.
> 
> This operation has a time complexity of $O(n)$, as the number of operations is scales linearly with the size of the list.


> [!INFO]+ Inserting at the end of a Linked List
> ```java
> void insert_end(Node list, int value) {
> 	Node newNode = new Node();
> 	newNode.val = value;
> 	newNode.next = null;
> 	if (isEmpty()) { 
> 		head = newNode;
> 		return; 
> 	}
> 	Node cursor = list;
> 	while (cursor.next != null) { // find the last node
> 		cursor = cursor.next;
> 	}
> 	cursor.next = newNode;
> }
> ```
> Due to the nature of Linked Lists, in order to add to the end of them, the entire list must be traversed.
> This operation has a time complexity of $O(n)$, as the number of operations is scales linearly with the size of the list.


> [!INFO]+ Deleting from the end of a Linked List
> ```java
> void delete_end(Node list) throws EmptyListException {
> 	if (isEmpty()) { 
> 		throw new EmptyListException;
> 	}
> 	if (list.next == null)
> 		list = null;
> 		
> 	Node cursor = list;
> 	while (cursor.next.next != null) { // find the second last node
> 		cursor = cursor.next
> 	}
> 	cursor.next = null;
> }
> ```
> Due to the nature of Linked Lists, in order to delete the value at the end, the entire list must be traversed.  
> This operation has a time complexity of $O(n)$, as the number of operations is scales linearly with the size of the list.

## Comparing Linked Lists and Arrays

|                              | Array  | Linked List |
| ---------------------------- | ------ | ----------- |
| Access data by position      | $O(1)$ | $O(n)$      |
| Insert at the beginning      | $O(n)$ | $O(1)$      |
| Insert at the end            | $O(n)$ | $O(n)$*     |
| Insert in a certain position | $O(n)$ | $O(n)$      |
| Delete first entry           | $O(n)$ | $O(1)$      |
| Delete $i$'th entry          | $O(n)$ | $O(n)$      |
| concatenate two lists        | $O(n)$ | $O(n)$*     |
\* indicates that it could be improved to $O(1)$ with a slightly different representation.

## Modified Linked Lists

- Linked List with a pointer to the last node: ^5f4f75
	- This improves `insert_end `($O(n) \rightarrow O(1)$), but does not improve `delete_end`, as to delete the last node, the node before it must be modified, and that still requires traversing the list.![[03-memory-and-linked-lists.pdf#page=45&rect=61,153,265,227&width=500|03-memory-and-linked-lists, p.28]]
- Doubly linked list:
	- In this implementation, each node has a pointer to the previous node in addition to the pointer to the next node. Additionally, there is a pointer to the last node as well.
	- This improves `insert_end`and `delete_end`. ![[03-memory-and-linked-lists.pdf#page=45&rect=23,61,251,133&width=500|03-memory-and-linked-lists, p.28]]

|                              | Array  | Linked List | Doubly Linked List |
| ---------------------------- | ------ | ----------- | ------------------ |
| Access data by position      | $O(1)$ | $O(n)$      | $O(n)$             |
| Insert at the beginning      | $O(n)$ | $O(1)$      | $O(1)$             |
| Insert at the end            | $O(n)$ | $O(n)$      | ==$O(1)$==         |
| Insert in a certain position | $O(n)$ | $O(n)$      | $O(n)$             |
| Delete first entry           | $O(n)$ | $O(1)$      | $O(1)$             |
| Delete $i$'th entry          | $O(n)$ | $O(n)$      | $O(n)$             |
| concatenate two lists        | $O(n)$ | $O(n)$      | ==$O(1)$==         |
|                              |        |             |                    |
Other options:
- Circularly Linked List:
	- Singly Linked: ![[03-memory-and-linked-lists.pdf#page=48&rect=181,165,378,251&width=400|03-memory-and-linked-lists, p.30]]
	- Doubly Linked: ![[03-memory-and-linked-lists.pdf#page=48&rect=170,60,389,160&width=400|03-memory-and-linked-lists, p.30]]