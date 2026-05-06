# 📌 XOR Memory Efficient Doubly Linked List

A comprehensive educational repository on the **XOR Memory Efficient Doubly Linked List**, also known as the **XOR Linked List**, implemented in **C/C++** with deep theoretical explanations of:

* Pointer XOR operations
* Memory optimization
* Bidirectional traversal using a single pointer
* Pointer arithmetic
* `uintptr_t`
* Low-level memory representation
* Time and space complexity analysis
* `nullptr` and modern C++ pointer safety

This repository demonstrates how a traditional Doubly Linked List can be optimized to reduce pointer memory overhead while still supporting forward and backward traversal. 

---

# Repository Link

🔗Repository: [https://github.com/AvinandanBose/XOR-Memory-Efficient-Doubly-Linked-List](https://github.com/AvinandanBose/XOR-Memory-Efficient-Doubly-Linked-List)

---

# Introduction

In a traditional Doubly Linked List, every node stores:

* Data
* Pointer to next node
* Pointer to previous node

```cpp
typedef struct DLL
{
    int data;
    DLL* next;
    DLL* prev;
} Node;
```

This requires **two pointers per node**.

The XOR Linked List reduces memory usage by storing:

```cpp
typedef struct XNode
{
    int data;
    struct XNode *npx;
} Node;
```

Where:

```text
npx = prev XOR next
```

Thus only **one pointer field** is needed instead of two. 

---

# Core Idea

The XOR Linked List uses the mathematical property of the XOR operator:

```text
A XOR B XOR A = B
```

This allows traversal in both directions while storing only one combined pointer.

---

# XOR Function

```cpp
Node *XOR(Node *a, Node *b)
{
    return (Node *)((uintptr_t)(a) ^ (uintptr_t)(b));
}
```

This function computes:

```text
prev ⊕ next
```

and stores it in the `npx` field. 

---

# Structure of XOR Linked List

```text
Node A:
npx = NULL ⊕ B

Node B:
npx = A ⊕ C

Node C:
npx = B ⊕ D

Node D:
npx = C ⊕ NULL
```

---

# How Traversal Works

Suppose current node is:

```text
C
```

and:

```text
C.npx = B ⊕ D
```

If we already know `D`, then:

```text
(B ⊕ D) ⊕ D = B
```

because:

```text
D ⊕ D = 0
```

Thus we recover the previous node.

Similarly:

```text
(B ⊕ D) ⊕ B = D
```

Hence traversal is possible in both directions using only one pointer field. 

---

# XOR Properties Used

| Property                  | Meaning            |
| ------------------------- | ------------------ |
| X ⊕ X = 0                 | Same values cancel |
| X ⊕ 0 = X                 | Identity           |
| X ⊕ Y = Y ⊕ X             | Commutative        |
| (X ⊕ Y) ⊕ Z = X ⊕ (Y ⊕ Z) | Associative        |

---

# Memory Efficiency

## Traditional Doubly Linked List

Each node stores:

| Component    | 64-bit Size |
| ------------ | ----------- |
| data         | 4 bytes     |
| padding      | 4 bytes     |
| next pointer | 8 bytes     |
| prev pointer | 8 bytes     |

Total:

```text
24 bytes
```

---

## XOR Linked List

Each node stores:

| Component   | 64-bit Size |
| ----------- | ----------- |
| data        | 4 bytes     |
| padding     | 4 bytes     |
| npx pointer | 8 bytes     |

Total:

```text
16 bytes
```

Thus memory usage is significantly reduced.



---

# Why uintptr_t is Necessary

The repository explains why raw pointers cannot directly use bitwise XOR.

Invalid:

```cpp
Node* c = a ^ b;
```

C++ does not allow bitwise operations on pointers.

Therefore pointers are converted to integer form using:

```cpp
uintptr_t
```

which is guaranteed to safely hold memory addresses.

```cpp
(Node *)((uintptr_t)(a) ^ (uintptr_t)(b))
```



---

# Why uintptr_t is Safe

## 32-bit System

```text
Pointer Size = 4 bytes
uintptr_t = 4 bytes
```

---

## 64-bit System

```text
Pointer Size = 8 bytes
uintptr_t = 8 bytes
```

Guarantee:

```text
sizeof(uintptr_t) == sizeof(void*)
```

Thus no address truncation occurs. 

---

# Empty XOR Linked List

```cpp
Node *head;

void createEmptyList()
{
    head = nullptr;
}
```

An empty XOR Linked List is represented by:

```text
head = nullptr
```



---

# Global Head Pointer

The repository explains why:

```cpp
Node *head;
```

is often declared globally:

* Easy traversal access
* Shared across functions
* Simplifies modular operations
* Maintains list state globally



---

# Zero Initialization Rule

Global pointers in C++ have:

```text
Static Storage Duration
```

Thus they are automatically initialized to:

```text
nullptr
```

even before explicit assignment.



---

# nullptr vs NULL

The repository discusses modern C++11 pointer safety.

## Problems with NULL

Older C++ versions used:

```cpp
NULL
```

or:

```cpp
0
```

which could accidentally behave as integers.

---

## Advantages of nullptr

```cpp
nullptr
```

provides:

* Type safety
* Better readability
* Reduced ambiguity
* Safer pointer handling

`nullptr` has type:

```text
std::nullptr_t
```



---

# Operations Covered

The repository includes theoretical explanation and implementations of:

* Creation of Empty XOR Linked List
* Forward Traversal
* Backward Traversal
* Insertion at Beginning
* Insertion at End
* Insertion at Position
* Deletion
* Searching
* Node Counting
* Displaying Nodes
* XOR Address Computation

---

# Time Complexity Analysis

| Operation              | Time Complexity |
| ---------------------- | --------------- |
| Traversal              | O(N)            |
| Search                 | O(N)            |
| Insertion at Beginning | O(1)            |
| Insertion at End       | O(N)            |
| Deletion               | O(N)            |

---

# Space Complexity

## Traditional DLL

```text
Θ(2 pointer fields per node)
```

---

## XOR Linked List

```text
Θ(1 pointer field per node)
```

Memory overhead is reduced by approximately one pointer per node.

---

# Advantages

* Reduced memory consumption
* Bidirectional traversal possible
* Efficient pointer utilization
* Useful in memory-constrained systems
* Demonstrates low-level memory operations

---

# Disadvantages

* Complex implementation
* Difficult debugging
* Pointer arithmetic complexity
* Less readable than normal DLL
* Unsafe in garbage-collected languages
* Difficult compatibility with modern debuggers

---

# Applications

XOR Linked Lists are useful in:

* Memory-constrained systems
* Embedded systems
* Low-level systems programming
* Custom allocators
* Experimental data structures
* Understanding pointer arithmetic

---

# Compilation

## Compile

```bash
g++ xor_linked_list.cpp -o xorlist
```

---

## Run

```bash
./xorlist
```

---

# Educational Concepts Covered

This repository helps in understanding:

* XOR Bitwise Operations
* Pointer Arithmetic
* Memory Optimization
* uintptr_t
* Dynamic Memory Allocation
* Data Structure Internals
* Linked List Traversal
* Type Safety in C++
* Modern C++ Pointer Management
* Space Complexity Optimization

---

# Learning Outcomes

After studying this repository, learners will understand:

* How XOR linked lists work internally
* How addresses are manipulated using XOR
* How memory optimization is achieved
* Why pointer-size-safe integer types matter
* The relationship between pointers and bitwise operations
* Advanced linked list design techniques

---

# Educational Importance

This repository is highly useful for:

* Computer Science students
* Advanced Data Structure learners
* Systems Programming
* Operating System concepts
* Competitive Programming
* Memory optimization studies
* Interview preparation

---

## 📜 License

This project is licensed under the **MIT License**.

---

## 👨‍💻 Author

**Avinandan Bose**
- GitHub: [@AvinandanBose](https://github.com/AvinandanBose)

---

## ⭐ Support

If you found this helpful:

* ⭐ Star the repository
* 🍴 Fork it
* 📢 Share with others

---
