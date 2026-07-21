
# Constructors in C++

> *"A constructor does not allocate memory. It initializes an object after memory has already been allocated."*

---

# Table of Contents

1. Introduction
2. Why Constructors Exist
3. What Happens During Object Creation
4. What is a Constructor?
5. Characteristics
6. Constructor Lifecycle
7. Types of Constructors
8. Default Constructor
9. Parameterized Constructor
10. Constructor Overloading
11. Constructor Initialization List
12. Copy Constructor
13. Move Constructor (Introduction)
14. Delegating Constructor
15. Explicit Constructor
16. Defaulted and Deleted Constructors
17. Order of Construction
18. Constructors and Inheritance
19. Constructors and Virtual Functions
20. Exception Safety
21. Common Mistakes
22. Production Best Practices
23. Interview Questions
24. Summary

---

# 1. Introduction

A constructor is a **special member function** that is automatically called when an object is created.

Unlike ordinary functions, a constructor's responsibility is not to perform business logic.

Its responsibility is to establish a **valid object**.

A well-designed constructor guarantees that every object starts life in a usable state.

---

# 2. Why Do Constructors Exist?

Imagine C++ had no constructors.

```cpp
class Student
{
public:
    int age;
    std::string name;
};
```

Now create an object.

```cpp
Student s;
```

Without a constructor, memory would be reserved but the object would have no defined initialization policy.

```
Stack

+----------------------+
| age      ???         |
| name     ???         |
+----------------------+
```

The language needs a mechanism that transforms **allocated memory into a valid object**.

That mechanism is the constructor.

---

# 3. What Actually Happens?

Consider

```cpp
Student s;
```

The simplified sequence is

```
Compiler knows object size

↓

Memory allocated

↓

Constructor called

↓

Object becomes valid

↓

Program continues
```

Notice the important point:

**Memory allocation happens before the constructor executes.**

The constructor initializes memory; it does **not** allocate it.

---

# 4. What is a Constructor?

A constructor

* has the same name as the class
* has no return type
* is called automatically
* runs exactly once per object

Example

```cpp
class Student
{
public:

    Student()
    {
        std::cout << "Constructor\n";
    }
};
```

```cpp
Student s;
```

Output

```
Constructor
```

---

# 5. Characteristics

A constructor

✅ has the same name as the class

✅ has no return type

✅ can be overloaded

✅ may have parameters

✅ cannot be inherited directly

✅ cannot be static

---

# 6. Why Doesn't It Have a Return Type?

Interview Favorite ⭐⭐⭐⭐⭐

Wrong idea

```cpp
int Student()
```

Question

What would it return?

The object already exists.

The constructor isn't creating a value to return.

Its job is to initialize the current object.

Therefore constructors have **no return type**, not even `void`.

---

# 7. Types of Constructors

C++ supports

* Default Constructor
* Parameterized Constructor
* Copy Constructor
* Move Constructor
* Delegating Constructor
* Defaulted Constructor
* Deleted Constructor

We'll study each individually.

---

# 8. Default Constructor

A constructor that takes no arguments.

```cpp
class Student
{
public:

    Student()
    {
        age = 18;
    }

    int age;
};
```

Usage

```cpp
Student s;
```

Result

```
age = 18
```

---

## Compiler Generated Default Constructor

Suppose

```cpp
class Student
{
public:
    int age;
};
```

You never wrote a constructor.

The compiler may generate one for you.

This generated constructor performs default initialization according to the language rules.

---

## When Doesn't the Compiler Generate One?

If you declare **any constructor**, the compiler no longer generates a default constructor automatically.

Example

```cpp
class Student
{
public:

    Student(int x)
    {

    }
};
```

Now

```cpp
Student s;
```

Compilation error.

Why?

Because no default constructor exists anymore.

---

# 9. Parameterized Constructor

```cpp
class Student
{
public:

    Student(int a)
    {
        age = a;
    }

    int age;
};
```

Usage

```cpp
Student s(25);
```

Memory

```
Stack

+----------------+
| age = 25       |
+----------------+
```

---

# 10. Constructor Overloading

Constructors can be overloaded.

```cpp
class Student
{
public:

    Student();

    Student(int);

    Student(int,std::string);
};
```

Compiler chooses the best matching constructor.

---

# 11. Initialization vs Assignment

Interview Favorite ⭐⭐⭐⭐⭐

Many beginners write

```cpp
Student(int a)
{
    age = a;
}
```

But the member is

1. initialized
2. then assigned

A better approach is

```cpp
Student(int a)
    : age(a)
{

}
```

This is called an **initializer list**.

---

# 12. Why Initialization List?

Suppose

```cpp
class Student
{
    int age;

public:

    Student(int a)
    {
        age = a;
    }
};
```

Sequence

```
age initialized

↓

assignment happens
```

With initializer list

```cpp
Student(int a)
    : age(a)
{

}
```

Sequence

```
age directly initialized
```

Less work.

More efficient.

---

# 13. Members That MUST Use Initializer Lists

These cannot be assigned later.

### const members

```cpp
const int id;
```

### References

```cpp
int& ref;
```

Both must be initialized before entering the constructor body.

Example

```cpp
class Student
{
    const int id;

public:

    Student(int x)
        : id(x)
    {

    }
};
```

---

# 14. Order of Initialization

Another famous interview question.

Consider

```cpp
class A
{
    int x;
    int y;

public:

    A()
        : y(10), x(5)
    {

    }
};
```

Question

Which initializes first?

Many answer

```
y
```

Wrong.

Initialization order always follows **declaration order**, not the order in the initializer list.

```
x

↓

y
```

---

# 15. Copy Constructor

Definition

Creates a new object from an existing object.

```cpp
Student a;

Student b = a;
```

Compiler calls

```cpp
Student(const Student&);
```

---

## Why Reference?

Interview Favorite ⭐⭐⭐⭐⭐

Wrong

```cpp
Student(Student obj)
```

Passing `obj` by value would itself require copying.

That would call the copy constructor again.

Infinite recursion.

Therefore

```cpp
Student(const Student&)
```

uses a reference.

---

# 16. Shallow Copy

Compiler-generated copy constructors perform **member-wise copy**.

For primitive members, that's fine.

For owning pointers, it can be dangerous.

```cpp
class Student
{
public:

    int* marks;
};
```

Both objects now point to the same memory.

```
Object A ----+

              |

Heap Memory

              |

Object B ----+
```

Deleting one object leaves the other with a dangling pointer.

---

# 17. Deep Copy

Deep copy creates a new resource.

```
Object A ----> Heap A

Object B ----> Heap B
```

Each object owns its own memory.

We'll study this in detail in the Copy & Move chapter.

---

# 18. Move Constructor (Preview)

Instead of copying expensive resources

```
Heap

100 MB
```

Ownership can be transferred.

```cpp
Student(Student&& other);
```

Move semantics avoid unnecessary copies and improve performance.

---

# 19. Delegating Constructors

One constructor can call another.

```cpp
class Student
{
public:

    Student()
        : Student(18)
    {

    }

    Student(int age)
    {

    }
};
```

Useful for avoiding duplicate initialization logic.

---

# 20. Explicit Constructor

Without `explicit`

```cpp
Student s = 5;
```

The compiler may perform an implicit conversion.

To prevent this

```cpp
explicit Student(int age);
```

Production code frequently marks single-argument constructors as `explicit`.

---

# 21. Defaulted Constructors

Modern C++

```cpp
Student() = default;
```

This requests the compiler-generated implementation explicitly.

Useful for readability and maintaining special member function rules.

---

# 22. Deleted Constructors

Sometimes object creation should be forbidden.

```cpp
Student() = delete;
```

Now

```cpp
Student s;
```

fails to compile.

This is commonly used to enforce invariants or prevent incorrect usage.

---

# 23. Constructors and Inheritance

When creating a derived object

```cpp
class Base { };

class Derived : public Base { };
```

Construction order is

```
Base

↓

Derived
```

Destruction happens in the reverse order.

---

# 24. Can Constructors Be Virtual?

Interview Favorite ⭐⭐⭐⭐⭐

No.

Reason

Virtual dispatch requires the object to already have the appropriate runtime type information established.

During construction, the object is still being built.

The language therefore does not support virtual constructors.

---

# 25. Can Constructors Throw Exceptions?

Yes.

Example

```cpp
if(file_not_found)
    throw std::runtime_error("Unable to open file");
```

If construction fails

* the object is considered not fully constructed
* its destructor is **not** called
* already constructed members are cleaned up automatically

This behavior is one reason RAII is so powerful.

---

# 26. Common Mistakes

❌ Doing heavy work inside constructors

❌ Throwing without considering exception safety

❌ Using assignment instead of initializer lists unnecessarily

❌ Forgetting `explicit`

❌ Owning raw pointers without defining copy semantics

❌ Assuming initializer list order controls initialization order

---

# 27. Production Best Practices

* Prefer initializer lists.
* Make single-argument constructors `explicit`.
* Keep constructors lightweight.
* Establish all invariants before returning.
* Use RAII for resource ownership.
* Prefer smart pointers over owning raw pointers.
* Avoid calling virtual functions from constructors.
* Avoid throwing after partially acquiring unmanaged resources.

---

# 28. Interview Questions

## Basic

* What is a constructor?
* Why is it needed?
* Why no return type?
* Can constructors be overloaded?

## Intermediate

* Why use initializer lists?
* Difference between initialization and assignment?
* Why does copy constructor take a reference?
* What is a delegating constructor?
* What is an explicit constructor?
* What happens if you define only a parameterized constructor?

## Advanced

* Explain object construction step by step.
* What is the initialization order of members?
* Can constructors be virtual?
* Can constructors throw exceptions?
* What happens if a member constructor throws?
* When does an object's lifetime officially begin?
* What special member functions may the compiler generate?
* How do constructors interact with move semantics?

---

# Chapter Summary

By the end of this chapter, you should understand:

* Why constructors exist.
* What happens internally during object creation.
* The different types of constructors.
* Why initializer lists matter.
* Copy vs move construction at a high level.
* Constructor behavior in inheritance.
* Exception safety during construction.
* Production-grade constructor design.

---
