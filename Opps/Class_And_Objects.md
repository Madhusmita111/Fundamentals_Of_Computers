# 02_Class_and_Object.md

# Classes and Objects in C++

> "A class is not memory. A class is information used by the compiler to generate memory layouts."

---

# Table of Contents

1. Why Classes Were Invented
2. What is a Class?
3. What is an Object?
4. Compiler's Perspective
5. Runtime Perspective
6. Memory Layout
7. Object Identity
8. State and Behavior
9. How Object Creation Works
10. Where is Class Stored?
11. Where are Objects Stored?
12. Where are Member Functions Stored?
13. Why Class Occupies No Memory
14. Why Object Occupies Memory
15. sizeof(Class)
16. Empty Class
17. Object Layout
18. Padding
19. Alignment
20. Hidden Members
21. Common Mistakes
22. Production Notes
23. Interview Questions
24. Summary

---

# 1. Why Were Classes Invented?

Imagine writing a banking application without classes.

```cpp
string customerName1;
int customerAge1;
double customerBalance1;

string customerName2;
int customerAge2;
double customerBalance2;

string customerName3;
int customerAge3;
double customerBalance3;
```

Now imagine **10 million customers**.

Impossible.

We need a template.

That template is called a **class**.

---

# 2. Definition

A class is a **user-defined data type** that defines

- data
- operations
- access control
- object layout
- object behavior

A class itself is **not an object**.

Think of it as a recipe.

---

# 3. First Example

```cpp
class Student
{
public:

    int age;

    string name;

    void study()
    {

    }
};
```

This code creates **zero objects**.

Memory used by objects?

```
0 Bytes
```

No object exists yet.

---

# 4. Why Doesn't Class Occupy Object Memory?

This is probably the most important beginner interview question.

Suppose

```cpp
class Student
{
public:

    int age;

};
```

Question

How many students exist?

Answer

None.

The compiler only learns

```
Every Student object
contains

↓

int age
```

No object has been created.

Therefore

No object memory.

---

# Compiler's View

Compiler stores metadata like

```
Class Name

↓

Member List

↓

Member Types

↓

Function List

↓

Access Rules

↓

Offsets

↓

Constructors

↓

Destructor

↓

Inheritance Information
```

It is blueprint information, not runtime object data.

---

# 5. What is an Object?

Object is an **instance** of a class.

```cpp
Student s;
```

Now memory is allocated.

```
Stack

+------------------+
| age              |
| name             |
+------------------+
```

The blueprint has become a real entity.

---

# 6. Class vs Object

| Class | Object |
|--------|--------|
| Blueprint | Real entity |
| Compile-time concept | Runtime concept |
| No object storage | Occupies memory |
| Defines structure | Stores actual values |
| One definition | Many instances |

---

# 7. Runtime

Suppose

```cpp
Student s1;
Student s2;
Student s3;
```

Memory

```
Stack

+-------------+
| s3          |
+-------------+

| s2          |
+-------------+

| s1          |
+-------------+
```

Every object has independent data.

Changing

```
s1.age
```

doesn't affect

```
s2.age
```

---

# 8. State and Behavior

Every object consists of

## State

Variables.

```
age

name

marks
```

---

## Behavior

Functions.

```
study()

sleep()

eat()
```

---

## Identity

Address in memory.

```
&s1

0x200

&s2

0x350
```

Different identities.

---

# 9. What Happens During Object Creation?

```cpp
Student s;
```

Compiler approximately performs

```
Step 1

Calculate object size

↓

Step 2

Reserve memory

↓

Step 3

Call constructor

↓

Step 4

Object becomes valid
```

Notice

Memory allocation happens **before** constructor executes.

---

# 10. Object Layout

Suppose

```cpp
class Student
{

int age;

double cgpa;

char grade;

};
```

Many beginners imagine

```
age

cgpa

grade
```

Reality

Compiler inserts padding.

Actual layout might become

```
+------------+
| age        |
+------------+

| padding    |
+------------+

| cgpa       |
+------------+

| grade      |
+------------+

| padding    |
+------------+
```

We'll study why shortly.

---

# 11. Where Are Objects Stored?

Depends.

```
Student s;
```

Stack.

---

```
new Student();
```

Heap.

---

```
static Student s;
```

Static Storage.

---

Global object

Global Storage.

---

# 12. Where Are Classes Stored?

Interview Trick.

Class itself is **not** stored like an object.

Compiler uses class information during compilation.

The class definition is part of the program metadata, not a runtime object.

---

# 13. Where Are Member Functions Stored?

Suppose

```cpp
class Student
{

public:

void display()
{

}

};
```

Question

Suppose

```cpp
Student s1;

Student s2;

Student s3;
```

Are there three copies of display()?

No.

Only one.

```
Text Segment

display()

↓

Objects

s1

s2

s3
```

Every object calls the same function.

---

# Why?

Imagine

10 million Student objects.

If every object stored display(),

memory usage would explode.

---

# 14. Why Member Variables Exist Per Object

Suppose

```
age
```

were shared.

Then

```
s1.age=20;

s2.age=30;
```

Which one wins?

Impossible.

Every object needs its own state.

Therefore

Variables exist inside every object.

Functions do not.

---

# 15. Hidden this Pointer

Question

How does

```cpp
s.display();
```

know which student called it?

Compiler secretly converts

```
display(&s);
```

That hidden pointer is

```
this
```

We'll study it later.

---

# 16. sizeof(Class)

Example

```cpp
class Student
{

int age;

double marks;

char grade;

};
```

Question

```
sizeof(Student)
```

Compiler calculates

```
int

+

double

+

char

+

padding
```

Not function size.

---

# 17. Empty Class

One of the most famous interview questions.

```cpp
class A
{

};
```

Question

```
sizeof(A)
```

Most beginners answer

```
0
```

Wrong.

Answer

```
1 Byte
```

---

# Why?

Suppose

```
0 bytes
```

Then

```cpp
A a;

A b;
```

Both occupy zero space.

Addresses

```
&a

0x100

&b

0x100
```

Same address.

Impossible.

Every object must have a unique address.

Compiler gives

```
1 Byte
```

---

# 18. Padding

Suppose

```cpp
class A
{

char a;

int b;

};
```

Naively

```
1+4=5
```

Reality

Usually

```
8
```

because compiler inserts padding.

---

Why?

CPU fetches aligned memory much faster.

Misaligned memory can require multiple memory accesses or special instructions on some architectures.

Padding trades a little memory for better performance.

---

# 19. Alignment

Each type has alignment requirements.

Typical example on 64-bit systems

```
char

1-byte alignment

int

4-byte alignment

double

8-byte alignment
```

Compiler arranges members to satisfy these requirements.

---

# Example

```cpp
class A
{

char c;

double d;

};
```

Layout

```
char

padding

padding

padding

padding

padding

padding

padding

double
```

---

# 20. Hidden Members

You write

```cpp
class A
{

int x;

};
```

Compiler may secretly add information later.

For example, if the class has virtual functions, it typically adds a hidden pointer (often called `vptr`) to support runtime polymorphism.

That is why object layout is determined by the compiler, not just your source code.

---

# 21. Common Mistakes

### Mistake 1

Thinking functions exist inside every object.

False.

---

### Mistake 2

Thinking class occupies runtime object memory.

False.

---

### Mistake 3

Thinking sizeof is just the sum of member sizes.

Padding changes it.

---

### Mistake 4

Ignoring alignment.

Can hurt performance.

---

### Mistake 5

Confusing blueprint with instance.

Class

≠

Object.

---

# 22. Production Notes

Large production systems care about object layout because

- billions of objects consume significant memory
- padding affects cache usage
- object size influences performance
- contiguous layouts improve cache locality
- unnecessary virtual functions increase object size

Memory layout directly impacts scalability.

---

# 23. Interview Questions

## Basic

- What is a class?
- What is an object?
- Difference between class and object?
- Why are classes needed?

---

## Intermediate

- Why does a class occupy no object memory?
- Why do objects occupy memory?
- Where are member functions stored?
- Where are objects stored?
- Why is sizeof(empty class) equal to 1?
- Why aren't functions counted in sizeof?
- How does compiler calculate object size?

---

## Advanced

- Explain object layout.
- Explain memory alignment.
- Explain padding.
- Why is padding beneficial?
- Can compiler reorder members?
- What determines member offsets?
- How does ABI affect object layout?
- Why is cache locality important?
- Why does adding a virtual function increase object size?
- What hidden information may the compiler add?

---

# Key Takeaways

✓ A class is a compile-time blueprint.

✓ An object is a runtime instance.

✓ Objects store data.

✓ Member functions are shared.

✓ Objects have identity, state, and behavior.

✓ Empty classes occupy one byte.

✓ Padding and alignment influence object size.

✓ Understanding object layout is essential for advanced OOP.

---

- Exception safety during construction
- Object lifetime begins exactly when?
- Production best practices
