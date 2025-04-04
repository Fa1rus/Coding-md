# Understanding Pointers in C

## Address in Memory
Every variable in C has a memory address where its value is stored. To retrieve the address of a variable, we use the ampersand (`&`) operator.

```c
int number = 50;
printf("Address of number: %p\n", &number);

// Example Output: Address of number: 0x7ffeefbff4c4
```

---

## What is a Pointer?
A **pointer** is a variable that stores the **memory address** of another variable instead of holding a direct value like `int` or `char`. Think of a pointer as a "house address" that tells you where to find a value in memory.

> **A pointer stores the address of another variable, allowing it to access and modify that variable directly.**

### Key Concept: Address ≠ Value  
A pointer holds an **address**, not the actual value.

Since a pointer already stores an address, you do **not** need to use the `&` operator when assigning a value to it.

---

## Pointer Syntax

When initializing a pointer, its type must match the type of the variable it points to:

```c
int age = 21;
int *pAge = &age;  // ✅ Correct: pAge is a pointer to an int

char *pAge = &age; // ❌ Incorrect: pAge expects a char, but age is an int
```

---

## Modifying Values via Pointers
You can change a variable’s value through a pointer by dereferencing it with `*`.

```c
int myNum = 9; 
int *myPointer = &myNum; 

printf("Before: %d\n", myNum); // Output: 9

*myPointer = 88;  // Modify value through pointer

printf("After: %d\n", myNum);  // Output: 88
```

---

## Using Pointers to Modify Values in Functions
Passing a pointer to a function allows direct modification of a variable.

```c
#include <stdio.h>

void addFive(int *number) {
    *number += 5;  // Modify value at the memory address
}

int main() {
    int myNum = 9;
    printf("Before: %d\n", myNum); 

    addFive(&myNum); // Pass address of myNum

    printf("After: %d\n", myNum); 
    return 0;
}
```

**Output:**
```
Before: 9
After: 14
```

Using pointers like this can improve performance, especially with large data structures.

---

## Pointer Declaration Examples
```c
int *a;   // 'a' is a pointer to an int
int *b, c; // Only 'b' is a pointer, 'c' is a normal int
int d, *e; // Only 'e' is a pointer
char *p;  // 'p' is a pointer to a char
```

It’s a good practice to initialize pointers to **NULL** to prevent unintended behavior.

---

## Pointer Operations
```c
int x = 1, *ptr1, *ptr2; // Declaration
ptr1 = &x;  // Assign address of x to ptr1
ptr2 = ptr1; // ptr2 now also points to x

*ptr1 = 7;  // Modify x via ptr1

printf("ptr1: %d\n", *ptr1); // Output: 7
printf("ptr2: %d\n", *ptr2); // Output: 7
```

### Understanding Dereferencing (`*`)
- `*ptr1 = 7;` → Assigns 7 to the memory address `ptr1` points to.
- `ptr1 += 1;` → Moves the pointer to the next memory location (not the same as `*ptr1 += 1;`).
  
---

## Pointers and Arrays
An **array** is essentially a **constant pointer** to its first element.

```c
int arr[5] = {10, 20, 30, 40, 50};
printf("%d\n", arr[2]);   // Output: 30
printf("%d\n", *(arr + 2)); // Output: 30 (Pointer arithmetic)
```

### Pointer Arithmetic
- `arr` → Points to the first element
- `arr + 1` → Moves to the next element
- `*(arr + 2)` → Retrieves the third element

> Since an array name is a constant pointer, you cannot increment `arr` directly. Instead, use a separate pointer.

### Example: Summing an Array with Pointers
```c
int sum(int *start, int *end) {
    int total = 0;
    while (start <= end) {
        total += *start;
        start++;  // Move pointer forward
    }
    return total;
}
```
Usage:
```c
sum(&arr[0], &arr[4]); // Sum from first to last element
```

---

## Pointers to Structures
Consider a struct for storing a point's coordinates:
```c
struct Point {
    int x;
    int y;
};
typedef struct Point Point;
```

### Accessing Structure Members
Normally:
```c
Point p;
p.x = 10;
p.y = 20;
```
Using a pointer:
```c
Point *ptr = &p;
(*ptr).x = 10;  // Use parentheses to ensure correct dereferencing
(*ptr).y = 20;
```
Or, using the arrow (`->`) operator for cleaner syntax:
```c
ptr->x = 10;
ptr->y = 20;
```
