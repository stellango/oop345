# OOP345 - General Notes:   Summary Notes

## Week 1 - Introduction

Topics: Introduction
 * Overview
 * C++ Building Blocks
 * Compilation and Execution

### Overview - [Reading Notes](https://advoop.sdds.ca/A-Introduction/overview)

#### Object Oriented Paradigm

* Encapsulation - Putting the data and behaviour together
* Inheritance -   Reusing design/reusing code
  * "is a kind of"
* Polymorphism -  Doing the same thing in different ways


#### Building Blocks

* **Object**:
  * Occupies memory
  * Holds a value
  * MAY have a name 
    * Does NOT have to have a name.

* **Variable**:
  * A named object
  * Occupies memory
  * Holds a value 
  * AND has name

* **Value**:  
  * Held by an object
  * CONTENTS of the allocated memory region
  * Defines its current state



### [C++ Building Blocks](https://advoop.sdds.ca/A-Introduction/cpp-building-blocks)




### [Compilation and Execution](https://advoop.sdds.ca/A-Introduction/compilation-and-execution)

* main() is just a function with return type int
  * Paramaters are either nothing or command-line arguments

```cpp
int main();                       // no command-line arguments
int main(int argc, char *argv[]); // two command-line arguments
```



---

## Week 2

Topics:
* Fyndamental Types
* Pointers, References and Arrays
* Classes and Scoped Enumerations


## Week 2 - Types
### [Fundamental Types](https://advoop.sdds.ca/B-Types/fundamental-types)


#### Initialization

Example - Initialization Expressions
```cpp
// initializer.cpp
int main () {
  int n0 = 7;     // C-style
  int n1 = 7.2;   // C-style - narrowing (loss of information) - outputs 7
  int n2 {6};     // universal form braces-enclosed list
  int n3 = {5};   // = is redundant
  int n4 = {5.5};       // @Asam: This won't compile

  // Section from @Fardad
  Student S{ "jack", 234567, 3.5 };     // Universal way of initialization, don't need parenthesis
}
```



### [Pointers, References and Arrays](https://advoop.sdds.ca/B-Types/pointers-references-and-arrays)


#### References

 * Reference is an alias for an existing object
   * A nickname for an object; objects can have multiple names
 * Pointer is just an address

 * Two reference declarations:   `&` and `&&`
   * Both of them are variables and have names, with locations in memory

* lvalue reference - references lvalue expressions
* rvalue reference - references rvalue expressions

Reference Types
 * lvalue reference
 * rvalue reference

##### RHS vs. LHS

 * Usually only things on RHS:
   * Literals                   e.g. 5, 6, 7, 8
   * Temporary Objects          e.g. `a = int(5);`
   * Moved Objects              TBD

What can be on LHS:
 * Things that can have a name
   * Meaning you can access them
   * AKA have accessible region of memory


### Classes and Scoped Enumerations: 

```cpp
// Calls copy-assignment
b = a;

// Calls move-assignment
b = std::move(a);

// Calls move-constructor
Array b (std::move(a));
Array b = (std::move(a));
```

