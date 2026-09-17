# General concepts I often forget

### Ternary Operator

```cpp
// Syntax
variable = condition ? expressionTrue : expressionFalse;

// Example
status = (age >= 18) ? "Adult" : "Minor"
```

### Syntax vs. Semantics

Syntax  -   Structure and grammar rules
Semantics - Meaning, logic, and behaviour


### What Fardad said not to include??

 * Never write `using` inside a **header file**.
   * Manually add in `std::`
   * Hidden logic when not following this rule
   * [Source](https://youtu.be/HiDHcPTKtIs?si=AZMNpnHNO9CnsElP&t=2230)
     * OOP244NAA Sep 30 Lecture


### Three Pillars of OOP

In Fardad's terms:

Encapsulation - Putting the data and behaviour together
Inheritance -   Reusing design/reusing code
Polymorphism -  Doing the same thing in different ways


### Compilers

*compile-time* - during the compiler's translation of a module's source code into binary code
*link-time* - during the linker's assembly of the binary code components for the modules that constitute an application, or
*run-time* - during the user's execution of the binary executable.



## Fundamental Types

 * `unsigned` types
   * Positive numbers for `signed` int are always 1 less than negative
* The `void` type is an incomplete type
   * A type is incomplete if it is missing some information. 
   * C++ does not allow the creation of objects of type void.
 * Used for:
   * Functions without a return value
   * Generic pointers

