# How C++ Works Internally

---
# Table of Contents

1. Why Learn How C++ Works?
2. Life of a C++ Program
3. Compilation Pipeline
4. Preprocessor
5. Compiler
6. Assembler
7. Linker
8. Executable File
9. Loader
10. Program Memory Layout
11. Text Segment
12. Data Segment
13. BSS Segment
14. Heap
15. Stack
16. Object Lifetime
17. Where Classes Live
18. Where Objects Live
19. Where Functions Live
20. Static Members
21. String Literals
22. Common Interview Questions
23. Production Notes
24. Summary

---

# 1. Why Learn This?

Almost every advanced OOP topic depends on understanding memory.

Interviewers don't ask:

> "What is a constructor?"

They ask:

> "Why is a constructor needed?"

To answer that, you must understand memory allocation.

Similarly,

- Why virtual functions work
- Why objects have size
- Why empty classes occupy one byte
- Why stack objects are destroyed automatically
- Why heap objects need delete
- Why static variables behave differently

All originate from the program memory model.

---

# 2. Life of a C++ Program

Your source code is **not** directly executed by the CPU.

```
You Write Code

↓

Preprocessor

↓

Compiler

↓

Assembler

↓

Linker

↓

Executable

↓

Loader

↓

Running Program
```

Each stage has a specific responsibility.

---

# 3. Step 1: Source Code

Suppose we write

```cpp
#include <iostream>

int main()
{
    std::cout << "Hello";
}
```

This file is

```
main.cpp
```

It is simply text.

The CPU cannot execute this.

---

# 4. Step 2: Preprocessor

The preprocessor runs first.

It performs:

- include expansion
- macro expansion
- conditional compilation

Example

```cpp
#include <iostream>
```

becomes

```
thousands of lines from iostream
```

The compiler never sees `#include`.

It only sees the expanded file.

---

## Why Preprocessor Exists?

Imagine manually copying

100,000 lines

into every source file.

Impossible.

Instead,

```
#include
```

acts as copy-paste before compilation.

---

## Example

Source

```cpp
#define PI 3.14

float area = PI * r * r;
```

After preprocessing

```cpp
float area = 3.14 * r * r;
```

---

## Interview Question

Does the compiler understand

```
#include
```

No.

The preprocessor removes it first.

---

# 5. Step 3: Compiler

The compiler converts C++ into assembly language.

It performs

- syntax checking
- type checking
- optimization
- code generation

Example

```cpp
int a = 10;
```

becomes assembly instructions.

---

## Compiler Responsibilities

- Detect errors
- Generate machine instructions
- Optimize code

---

## Example Error

```cpp
int x = "Hello";
```

Compiler reports

```
Type mismatch
```

because this is invalid C++.

---

# 6. Step 4: Assembler

Assembly is still text.

Example

```
MOV AX,10
ADD AX,5
```

Assembler converts it into machine code.

```
101001101010...
```

Only machine code runs on the CPU.

---

# 7. Step 5: Linker

This is one of the most misunderstood stages.

Suppose

```
main.cpp

math.cpp

student.cpp
```

All are compiled separately.

The linker combines them.

```
main.o

math.o

student.o

↓

Executable
```

---

## Example

main.cpp

```cpp
int add(int,int);

int main()
{
    add(2,3);
}
```

math.cpp

```cpp
int add(int a,int b)
{
    return a+b;
}
```

Compiler compiles independently.

Linker connects

```
add()
```

to

```
main()
```

---

## Why Linker Exists?

Large projects may contain

```
5000 cpp files
```

Compiling everything every time would be too slow.

Separate compilation solves this.

---

# 8. Executable

After linking,

we obtain

```
program.exe

or

a.out
```

This file contains

- machine code
- metadata
- symbols
- relocation information

Still not running.

---

# 9. Loader

When you double-click

```
program.exe
```

the Operating System loads it into RAM.

Memory is allocated.

Only then

```
main()
```

starts.

---

# 10. Program Memory Layout

Every running C++ program roughly looks like this.

```
High Memory

+---------------------+
|      Stack          |
+---------------------+

|                     |

|                     |

+---------------------+
|       Heap          |
+---------------------+

|   BSS Segment       |
+---------------------+

|   Data Segment      |
+---------------------+

|   Text Segment      |
+---------------------+

Low Memory
```

Every interviewer expects this diagram.

---

# 11. Text Segment

Also called

```
Code Segment
```

Contains

- functions
- compiled instructions
- read-only machine code

Example

```cpp
void display()
{
    cout<<"Hi";
}
```

Only ONE copy exists.

Not one per object.

---

## Interview Question

Where are member functions stored?

Answer

```
Text Segment
```

Objects do NOT contain function code.

---

# 12. Data Segment

Stores

```
Initialized global variables

Initialized static variables
```

Example

```cpp
int x = 10;

static int y = 20;
```

Stored here.

---

# 13. BSS Segment

Stores

```
Uninitialized globals

Uninitialized static variables
```

Example

```cpp
int x;

static int y;
```

Both initially become zero.

---

# 14. Heap

Dynamic memory.

Allocated using

```cpp
new
```

Example

```cpp
Student* s = new Student();
```

Memory comes from heap.

Must be released.

```cpp
delete s;
```

---

## Why Heap?

Size unknown at compile time.

Lifetime controlled by programmer.

---

# 15. Stack

Automatic memory.

Example

```cpp
Student s;
```

Lives on stack.

Destroyed automatically.

---

## Stack Characteristics

Fast allocation

Fast deallocation

Automatic cleanup

Limited size

---

# Stack Example

```cpp
int main()
{
    int a;

    Student s;

}
```

```
Stack

+---------------+
| Student s     |
+---------------+
| int a         |
+---------------+
```

When function returns

Entire stack frame disappears.

---

# Heap Example

```cpp
Student* s = new Student();
```

```
Stack

+---------------+
| pointer s ----+------------+
+---------------+            |
                             |
Heap                         |
+----------------------------+
| Student Object             |
+----------------------------+
```

Deleting pointer frees heap memory.

---

# 16. Object Lifetime

Stack Object

```cpp
Student s;
```

Created

↓

Destroyed automatically.

---

Heap Object

```cpp
Student* s = new Student();
```

Created

↓

Lives until

```
delete
```

---

Static Object

```cpp
static Student s;
```

Lives

Entire program.

---

Global Object

Lives

Entire program.

---

# 17. Where Classes Live?

Trick question.

A class does **not** occupy runtime memory as an object.

It is a compile-time blueprint.

The compiler uses it to determine

- object layout
- member offsets
- generated functions
- access rules

---

# 18. Where Objects Live?

Depends on creation.

```cpp
Student s;
```

Stack.

---

```cpp
Student* s = new Student();
```

Heap.

---

```cpp
static Student s;
```

Static storage.

---

# 19. Where Member Functions Live?

Very important.

```
Text Segment
```

Only one copy.

Suppose

```cpp
Student s1;

Student s2;

Student s3;
```

All call

```
display()
```

There are NOT three copies.

Only one.

---

# 20. Where Static Members Live?

Example

```cpp
class Student
{
static int count;
};
```

count

Lives in

```
Data Segment

or

BSS
```

NOT inside object.

---

# 21. String Literals

Example

```cpp
char* s = "Hello";
```

"Hello"

Lives in

```
Read-only memory
```

Attempting

```cpp
s[0]='A';
```

Produces undefined behavior.

---

# Common Interview Questions

### Q1

Why are functions not stored inside every object?

Memory efficiency.

---

### Q2

Why is stack faster?

Simple pointer adjustment.

No fragmentation.

---

### Q3

Why is heap slower?

Allocator management.

Fragmentation.

Synchronization.

---

### Q4

Why do stack objects automatically call destructor?

Stack unwinding.

---

### Q5

Can stack overflow happen?

Yes.

Deep recursion.

Huge local arrays.

---

### Q6

Can heap overflow happen?

Out-of-memory conditions or allocator failure.

---

### Q7

Where is `main()` stored?

Text Segment.

---

### Q8

Where are string literals stored?

Read-only memory.

---

### Q9

Where do virtual tables live?

Implementation-defined.

Typically in read-only program data.

---

### Q10

Where does the object actually begin existing?

Immediately after memory is allocated and construction completes.

We'll study this in detail with constructors.

---

# Production Notes

Modern C++ encourages:

- Prefer stack allocation when ownership is simple.
- Use RAII for automatic cleanup.
- Avoid raw `new` and `delete`; prefer `std::unique_ptr` or `std::shared_ptr`.
- Minimize unnecessary dynamic allocation.
- Be mindful of cache locality. A `std::vector` of objects often outperforms linked structures because contiguous memory improves cache utilization.
- Understand object lifetime before designing APIs.

---

# Summary

You should now understand:

✓ Complete C++ compilation pipeline

✓ Preprocessor

✓ Compiler

✓ Assembler

✓ Linker

✓ Loader

✓ Program memory layout

✓ Stack

✓ Heap

✓ Text Segment

✓ Data Segment

✓ BSS Segment

✓ Object lifetime

✓ Where classes live

✓ Where objects live

✓ Where functions live

✓ Where static variables live

These concepts form the foundation for every advanced OOP topic that follows.

---
