---
date: 2026-02-10
updated: 2026-07-28T13:20:00
tags:
  - lessons/lecture
---
![[04a-queues.pdf]]

# Queue Basics


> [!INFO] Recall: LIFO (Last-In-First-Out)
> ![[Week 3 - Stacks#^62c955]]

**Queues** are an [[Week 3 - Abstract Data Types, Lists & Complexity#Abstract Data Types|Abstract Data Type (ADT)]] which behave in a **First-In-First-Out (FIFO)** manner. This means that elements added first will also be removed first from the queue. 

**Queues** are defined by their 3 operations:
- `enqueue(x)`: puts value `x` at the *rear* of the queue
- `dequeue()`: returns the value at the *front* of the queue 
- `isEmpty()`: checks if the queue is empty
![[04a-queues.pdf#page=3&rect=131,69,322,137&width=600|04a-queues, p.2]]


> [!INFO] A simple definition of the Queue as an ADT in Java
> ```java
> public interface Queue<Type> {  
>     int size();  
>     boolean isEmpty();  
>     void enqueue(Type value);  
>     Type dequeue();  
> }
> ```

# Implementing Queue

As an **ADT**, the internal implementation of the Queue is not important when using them. This means that the Queue can be implemented in different ways internally.

## Queue as a Linked List

In order to have an efficient implementation, the location of the last element should be stored.

> [!INFO] Recall
> ![[Week 3 - Computer Memory & Linked Lists#^5f4f75]]

Having the front of the queue at the beginning of the linked list is the optimal implementation. 

> [!NOTE]
> This is because enqueuing (adding to the rear of the queue) can be done in $O(1)$ regardless of whether it is at the beginning or the end of the list.
> 
> On the other hand, dequeuing (removing from the front of the queue) is $O(1)$ at the beginning of the list, while being $O(n)$ at the end.
> (This is because, removing from the end of the linked list requires modifying the second to last node, which is not kept track of. Therefore it requires traversing the list which is expensive.)

