# Object-Oriented Programming (OOP)

> **Goal:** Understand *why* Object-Oriented Programming exists, the problems it solves, and how C++ implements it internally. This chapter builds the mental model required for advanced OOP concepts.

---

# Table of Contents

1. Why Programming Paradigms Evolved
2. Procedural Programming
3. Problems with Procedural Programming
4. Why OOP Was Invented
5. Core Philosophy of OOP
6. Real-World Analogy
7. OOP in C++
8. Why Companies Still Use OOP
9. OOP in Production Systems
10. Common Misconceptions
11. Interview Questions
12. Summary

---

# 1. Why Programming Paradigms Evolved

Programming languages evolved because software became increasingly complex.

In the 1960s and 1970s, programs were relatively small. A payroll calculator or a compiler could often fit into a few thousand lines of code.

Today, applications like Google Chrome, Photoshop, or Unreal Engine contain **millions of lines of code** and are developed by thousands of engineers.

The challenge shifted from **writing code** to **managing complexity**.

Every programming paradigm was introduced to solve a specific problem.

| Paradigm               | Solves                                 |
| ---------------------- | -------------------------------------- |
| Procedural             | Organizing instructions into functions |
| Object-Oriented        | Organizing large software systems      |
| Functional             | Predictable and immutable computations |
| Generic Programming    | Reusable algorithms                    |
| Concurrent Programming | Efficient use of multicore processors  |

OOP was created primarily to make large software systems easier to build and maintain.

---

# 2. Procedural Programming

Procedural programming organizes software around **functions**.

Example:

```cpp
deposit();
withdraw();
checkBalance();
```

The program is essentially a sequence of procedures operating on data.

Example:

```cpp
int balance = 1000;

deposit(balance);
withdraw(balance);
```

The functions and the data are separate.

---

# 3. Problems with Procedural Programming

As software grows, procedural programming introduces several issues.

## Problem 1: Global Data

```cpp
int balance = 5000;
```

Any function can modify it.

```cpp
withdraw();
deposit();
hackAccount();
```

There is no central control.

---

## Problem 2: Poor Scalability

Imagine a banking application with:

* Customers
* Accounts
* Employees
* Loans
* Transactions
* Cards

Hundreds of functions begin sharing the same data.

Understanding the system becomes difficult.

---

## Problem 3: Low Reusability

Functions often depend on shared variables.

Reusing them in another project becomes difficult.

---

## Problem 4: Maintenance Cost

Changing one variable can unexpectedly break multiple functions.

Large procedural projects become fragile.

---

# 4. Why OOP Was Invented

OOP organizes software around **objects** instead of functions.

Instead of asking:

> Which function should I call?

We ask:

> Which object is responsible for this behavior?

Example:

```cpp
Account account;

account.deposit(1000);
account.withdraw(500);
```

Notice how the data and behavior belong together.

This is the central idea of OOP.

---

# 5. Core Philosophy of OOP

Every object represents three things.

## Identity

Every object is unique.

Example:

```
Employee A

Employee B
```

Even if their values are identical, they are different objects.

---

## State

The data stored inside an object.

Example:

```
Name

Age

Salary

Department
```

---

## Behavior

The operations an object can perform.

```
calculateSalary()

promote()

transfer()
```

---

# 6. Why Objects?

Suppose we are building an e-commerce website.

Without OOP:

```
createOrder()

cancelOrder()

addItem()

removeItem()

calculateTotal()
```

Every function receives multiple variables.

As the project grows, tracking dependencies becomes difficult.

With OOP:

```cpp
Order order;

order.addItem();
order.removeItem();
order.calculateTotal();
```

Everything related to an order lives inside the `Order` object.

This improves readability and maintainability.

---

# 7. OOP in C++

C++ implements OOP through **classes**.

A class is a blueprint describing:

* Data members
* Member functions
* Access permissions
* Construction rules
* Destruction rules

Example:

```cpp
class Student
{
public:
    std::string name;
    int age;

    void study();
};
```

The class itself occupies no object storage.

Objects created from the class hold the data.

---

# 8. Why Companies Prefer OOP

Large software projects require:

* Maintainability
* Reusability
* Testability
* Modularity
* Team collaboration

OOP supports these goals.

Example:

A payment team owns the `Payment` class.

A user team owns the `User` class.

Each team can improve its module independently.

---

# 9. OOP in Production

Modern software often combines OOP with other paradigms.

Examples:

* Game engines
* Banking software
* ERP systems
* CAD applications
* IDEs
* Backend services

However, production engineers do **not** blindly use inheritance everywhere.

Modern C++ often prefers:

* Composition over inheritance
* Value semantics
* RAII
* Smart pointers
* SOLID principles

Understanding *when not to use OOP* is just as important as understanding OOP itself.

---

# 10. Common Misconceptions

## Misconception 1

"OOP is only about the four pillars."

Not true.

Those are important concepts, but they do not explain memory layout, object lifetime, constructors, move semantics, RAII, virtual dispatch, or ownership.

---

## Misconception 2

"A class stores functions."

Incorrect.

Objects store data.

Functions typically exist only once in the program's executable code section.

Multiple objects share the same function implementation.

---

## Misconception 3

"Everything should be inheritance."

Incorrect.

Inheritance introduces coupling.

Composition is often a better design choice.

---

## Misconception 4

"OOP automatically produces good software."

Incorrect.

Poor class design still results in poor software.

OOP is a tool, not a guarantee.

---

# 11. What We Will Learn

In the upcoming chapters we will study:

* Program memory layout
* Stack vs Heap
* Class internals
* Object memory layout
* Constructors
* Destructors
* `this` pointer
* Copy constructor
* Move constructor
* Copy assignment
* Move assignment
* Rule of Three
* Rule of Five
* Rule of Zero
* Static members
* Friend functions
* Inheritance
* Polymorphism
* Virtual functions
* VTable
* VPTR
* RTTI
* Object slicing
* Diamond problem
* Virtual inheritance
* Templates
* Exception safety
* RAII
* Smart pointers
* SOLID principles
* Design patterns
* Python OOP comparison

---

# 12. Interview Questions

## Basic

* What is OOP?
* Why was OOP introduced?
* Why is OOP better than procedural programming?
* What problems does OOP solve?
* What is an object?
* What is a class?

---

## Intermediate

* Why does C++ support multiple paradigms?
* Why do companies still use OOP?
* Is OOP always the best solution?
* Why is composition often preferred over inheritance?
* Why does C++ allow both procedural and OOP styles?

---

## Advanced

* Can a program be completely object-oriented?
* Why do modern C++ codebases use fewer inheritance hierarchies?
* What are the performance costs of OOP?
* Does OOP increase cache misses?
* How does virtual dispatch affect branch prediction?
* Why do some game engines avoid excessive inheritance?
* What problems arise from deep inheritance trees?

---

# Chapter Summary

By the end of this chapter you should understand:

* Why programming paradigms evolved.
* The limitations of procedural programming.
* Why Object-Oriented Programming was invented.
* The concepts of identity, state, and behavior.
* Why large organizations rely on OOP.
* Why OOP alone does not guarantee good software.
* The roadmap for mastering advanced C++ OOP.

---

# Next Chapter

**01_How_CPP_Works.md**

Before learning classes, we will study what actually happens when a C++ program is compiled.

Topics include:

* Source code to executable
* Preprocessor
* Compiler
* Assembler
* Linker
* Program memory layout
* Stack
* Heap
* Text segment
* Data segment
* BSS segment
* Static storage
* Object lifetime
* Where functions live
* Where objects live
* Where classes live
* Why understanding memory is essential before OOP
