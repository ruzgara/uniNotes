---
date: 2026-02-03
updated: 2026-07-26T20:00:00
tags:
  - lessons/lecture
---
![[03a-stacks.pdf]]

# Stacks Basics

Stacks are an [[Week 3 - Abstract Data Types, Lists & Complexity#Abstract Data Types|Abstract Data Type (ADT)]] defined by three operations:
- `push(x)`: puts value `x` on top of the stack.
- `pop()`: takes out a value from the top of the stack.
- `isEmpty()`: checks whether the stack is empty.

![[03a-stacks.pdf#page=2&rect=125,36,289,145|03a-stacks, p.1]]
This means that the stack behaves in a **Last In First Out (LIFO)** manner. ^62c955

As part of the specification of stacks, it is usually also said that `push(x)` followed by `isEmpty()` must return `false`, and that `push(x)` followed by `pop()` must return `x`.

> [!INFO] A simple definition of the Stack as an ADT in Java
> ```java
> public interface Stack<Type> {  
>     int size();  
>     boolean isEmpty();  
>     void push(Type value);  
>     Type pop();  
> }
> ```
# Analysing Space Complexity with Stacks

Example:
```java
int fib (int n) {
	if (n < 2)
		return n;
	else
		return fib(n - 1) + fib(n - 2);
}
```

In order to analyse how much memory this function will use, we can put each recursive call into a stack.
For example, for `fib(3)`:![[03a-stacks.pdf#page=6&rect=26,31,430,131|03a-stacks, p.3]]

# Implementing Stacks

Stacks can be implemented in a variety of ways. A simple way would be to use arrays which are doubled in size as required.

> [!INFO]- A simple implementation of a stack in Java
> This example implements the previously defined Stack interface.
> ```java
> import java.util.Arrays;  
> import java.util.NoSuchElementException;  
>   
> public class SimpleStack<Type> implements Stack<Type> {  
>     private static final int DEFAULT_CAPACITY = 10;  
>   
>     private int size;  
>     private Object[] elements;  
>   
>     public SimpleStack() {  
>         elements = new Object[DEFAULT_CAPACITY];  
>         size = 0;  
>     }  
>   
>     @Override  
>     public int size() {  
>         return this.size;  
>     }  
>   
>     @Override  
>     public boolean isEmpty() {  
>         return (size == 0);  
>     }  
>   
>     @Override  
>     public void push(Type value) {  
>         ensureCapacity();  
>         elements[this.size++] = value;  
>     }  
>   
>     @SuppressWarnings("unchecked")  
>     @Override  
>     public Type pop() {  
>         if (isEmpty())  
>             throw new NoSuchElementException("Stack is empty");  
>         Type value = (Type) elements[--size];  
>         elements[size] = null;  
>         return value;  
>     }  
>   
>     private void ensureCapacity() {  
>         if (size == elements.length) {  
>             elements = Arrays.copyOf(elements, elements.length * 2);  
>         }  
>     }  
> }
> ```

Another way is to store it as a [[Week 3 - Computer Memory & Linked Lists#Linked Lists|Linked List]]. When doing this, having the top value at the beginning of the linked list is more efficient, as it does not require traversing the list (in a singly-linked list).

> [!INFO]- Stack implementation with a Linked List in Java
> ```java
> import java.util.Arrays;  
> import java.util.NoSuchElementException;  
>   
> public class SimpleStack<Type> implements Stack<Type> {  
>     private static final int DEFAULT_CAPACITY = 10;  
>   
>     private int size;  
>     private Object[] elements;  
>   
>     public SimpleStack() {  
>         elements = new Object[DEFAULT_CAPACITY];  
>         size = 0;  
>     }  
>   
>     @Override  
>     public int size() {  
>         return this.size;  
>     }  
>   
>     @Override  
>     public boolean isEmpty() {  
>         return (size == 0);  
>     }  
>   
>     @Override  
>     public void push(Type value) {  
>         ensureCapacity();  
>         elements[this.size++] = value;  
>     }  
>   
>     @SuppressWarnings("unchecked")  
>     @Override  
>     public Type pop() {  
>         if (isEmpty())  
>             throw new NoSuchElementException("Stack is empty");  
>         Type value = (Type) elements[--size];  
>         elements[size] = null;  
>         return value;  
>     }  
>   
>     private void ensureCapacity() {  
>         if (size == elements.length) {  
>             elements = Arrays.copyOf(elements, elements.length * 2);  
>         }  
>     }  
> }
> ```

In both implementations, `push`, `pop` and `isEmpty` finish in *constant time*.
