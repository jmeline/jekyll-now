---
title: Topics in computer science - Stack, heap, value types vs reference types
---

Before we discuss what the differences between Value Types and Reference Types are, let us give some bulleted comparisons between the stack and the heap.

### Stack

* Stored in computer RAM
* Variables created will automatically deallocate when they fall out of scope (don't have to explicitly deallocate variables)
* Faster memory allocation
* Implemented with an actual stack data structure ("LIFO" last in, first out)
* Stores local data, returns addresses, and used for parameter passing
  - stack grows and shrinks as functions push and pop local variables
* Can throw stack overflow if limit is exceeded (i.e. infinite recursive functions)
* Stack limit is determined on program startup and it is a fixed size that is calculated by your system architecture, operating system, and compiler

### Heap

* Stored in computer RAM
* In languages where the memory isn't managed (c/c++), variables on the heap must be manually destroyed as they do not fall out of scope
* Slower memory allocation
* Used on demand to allocate (there is no enforced pattern to the allocation/deallocation)
* Allocation errors can happen if a too big of a buffer is attempted
* Use the heap if you don't know how much data you will need at runtime
* Memory leaks can happen if a developer isn't careful

### Value Types & Reference Types

There are two types of data types:
  Value based types - memory allocated on stack
  Reference based types - memory allocated on heap

#### TO be continued

### Tons of references to further knowledge:

* [programmer interview: What's the difference between a stack and a heap?](http://www.programmerinterview.com/index.php/data-structures/difference-between-stack-and-heap/)
* [.Net concepts: Stack, heap, value types, reference types, boxing and unboxing](https://www.codeproject.com/Articles/76153/Six-important-NET-concepts-Stack-heap-value-types)
* [SO: What and where are the stack and heap?](https://stackoverflow.com/questions/79923/what-and-where-are-the-stack-and-heap)
* [value types vs reference types](https://www.scribd.com/document/6959429/Value-Types-Vs-Reference-Types-in-C)
