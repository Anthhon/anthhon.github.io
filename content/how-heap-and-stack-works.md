---
title: "Differences between Heap and Stack"
date: 2023-04-24T19:27:19-03:00
tags: ['C','Memory','Concepts']
---

# Memory *Segmentation*

Actually a compiled program's memory is divided into five memory segments 
namely: `text, data, bss, heap and stack.` But for the sake of 
this article, we are gonna talk specifically about the heap and the stack 
segments, which are gonna help us to understand some really important concepts.\
\
Understanding how stack and heap works could be benefitial to every programmer 
because in the end of the day every application needs memory to work, and **a 
lot of problems we have while developing something is related to how computer 
treats memory.** So, understanding what is going under the covers can be great 
to you as a developer.

# *Stack* allocation

```
+--------------+
|     Stack    |
|              |
|       ↑      |
+--------------+
|     Heap     |
+--------------+
|      BSS     |
+--------------+
|     Data     |
+--------------+
|     Text     |
+--------------+
```

The stack allocation happens in what we call as a `memory block`, which is as likely 
represented in the ascii art above, the size of this memory block is known by 
the compiler which automatically allocate and deallocate the needed memory.\
\
It's also important to remember that **each function in a compiled program has its 
own memory block** which contain every single variable that is gonna be 
used into this function.\
\
This kind of memory allocation is also known as 
Temporary memory allocation, because **as soon as the function (method) 
finishes its execution, all the data belonging to this stack is thrown 
out.** (or deallocated to the systematics ones)

## Example of *stack allocated memory*

```c
int main(void){
	// All these variables are 
	// allocated in stack memory block
	int size;
	char string[10];
	int number = 20;
	int array[size];
	return 0;
}
```

When the `main()` function is called, all these variables are automatically 
allocated by the C compiler and deallocated whenever the function finishes.\
\
Something that's worth to talk about it's that **the stack memory block has a 
limited size,** which when surpassed gave the so well known StackOverflow error. 
(Which is an overflow of content in the stack memory block)

# *Heap* allocation

The heap memory block has some little differences compared to the stack, in general we can say that 
**the heap memory block can be controlled by the programmers,** and has its use during 
the execution of the program instructions.\
\
So, the heap can actually be much more manipulated which does not necessarily means that it's 
better. Because **at the same time that it's powerful, it is more prone to errors and instability.**
So whoever it's manipulating and storing object data in the heap memory, has to be 
really careful to not make mistakes.\
\
This memory allocation scheme has some differences from stack allocation, here we don't have such 
a thing as like automatic de-allocation feature provided by the compiler, **the programmer it's 
responsible to deallocate the memory at the right time to avoid memory leaks or unecessary 
memory usage.**

## Example of *heap allocated memory*

```c
int main(void){
	// Allocate 10 of whatever is 
	// the array type at the heap
	char *array = malloc(sizeof(*array) * 10);

	// Free array memory that was
	// allocated in the heap
	if (array != NULL){
		free(array);
		array = NULL;
	}

	return 0;
}
```

As you can see the `main()` is called and has a `array` variable declared and defined using the 
[malloc](https://www.gnu.org/software/libc/manual/html_node/Basic-Allocation.html) to allocate 
"custom-sized" memory in the heap to store 10 instances of whatever it's the array type. And 
just before the end of the function, we free the heap memory used by the array.\
\
**This type of memory is normally used when an object has a undefined size or when we have a 
defined size object which will be repeatedly created undefined times.** It's also good to 
remember that, badly managed memory can lead to memory leaks, which in a real applications 
can result in serious security problems.

# *Key differences* between stack and heap

In a stack the allocation and de-allocation it's automatically done by the compiler, whereas in 
the heap memory allocation it has to be done by the programmer itself.\
\
Memory overflow problems are more likely to happen in the stack, whereas the main heap issue is 
about security and memory fragmentation.\
\
A stack is not flexible, so the memory used in it cannot be re-sized just as like in the heap which 
can have it size altered.

## References

- https://www.geeksforgeeks.org/stack-vs-heap-memory-allocation/
- [Hacking: The Art of Exploitation, 2nd Edition | 0x270 Memory Segmentation](https://www.amazon.com/Hacking-Art-Exploitation-2nd-English-ebook/dp/B004OEJN3I)
