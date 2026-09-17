# OOP345 - General Notes:   Master Notes

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

* Common building blocks:
  * values
  * objects
  * variables
  * references
  * functions
  * types
  * class members
  * templates
  * namespaces

* Object:
  * Occupies memory
  * Holds a value
  * MAY have a name 
    * Does NOT have to have a name.

* Variable:
  * A named object
  * Occupies memory
  * Holds a value 
  * AND has name

* Value:  
  * Held by an object
  * CONTENTS of the allocated memory region
  * Defines its current state


##### Types

* Fundamental Types - come with the system
  * e.g. `int`, `double`
* Built-in Types - made up of fundamental types
  * e.g. An array of `int`
* Custom Types - created from the prior types
  * Concrete vs. Abstract
    * Representation is part of definition AND known vs. not


##### Declarations

 * Scope
   * local scope - the name has been declared within a function or code block
   * class scope - the name has been declared as a member of a class
   * namespace scope - the name has been declared as a member of a named block
   * global scope - the name has not been declared in any one of the above scopes


#### Compilers

#####  Statically and Dynamically Allocated



---


### [C++ Building Blocks](https://advoop.sdds.ca/A-Introduction/cpp-building-blocks)

### C++ Building Blocks - [Fardad Lecture Jan 12](https://www.youtube.com/watch?v=8LJIMynQkbg)

#### Declarations 

* Declarations vs. Definitions
  * Declarations are the introduction
  * Definitions allocate memory (for variables) or provide a body (for classes/functions)


#### Declarations:  Scope

* Scope
  * Block Scope
  * Class Scope
  * Global Scope
  * Shadowing


#### Declarations:  Linkage

 * `extern` - global scope variable
   * Use `extern` keyword only for declaration, NOT definition

Example
```cpp
extern int share_me;    // declaration
int share_me = 0;       // definition
```

 * `static` - file scope variable
   * Global lifetime, local scope

Example 1:  Static Local Variables
```cpp
void foo() {
    // int a = 10;                      // Output is 11 11 11 11 11
    static int a = 10;                  // Output is 11 12 13 14 15
    a++;
    std::cout << a << std::endl;
}

int main() {
    for (int i = 0; i < 5; i++) {
        foo();
    }
}
```

Example 2:  Returning Ref of Static Variables
```cpp
// Illegal - a dies
int& faa() {
    int a;
    cin >> a;
    return a;
}

// Legal
const int& getInt() {
    static int a{};
    cin >> a;
    return a;
}
```

@@ Why Static Variable Reference Returned


#### Declarations:  Type

* cv Qualifiers
  * `const` - the type is NOT modifiable and NOT subject to any side-effects
  * `voltile` - the type IS modifiable and subject to SOME side-effects
  * `const volatile` - the type is NOT modifiable and subject to SOME side-effects


* `volatile` variables - values are not buffered
  * Every time you want to access, it'll go to the target and bring it back
  * Regular ones are cached into the CPU when it's working and is much faster
  * NTS: Fardad explains but we don't really need it for OOP345




### [Compilation and Execution](https://advoop.sdds.ca/A-Introduction/compilation-and-execution)

* main() is just a function with return type int
  * Paramaters are either nothing or command-line arguments

```cpp
int main();                       // no command-line arguments
int main(int argc, char *argv[]); // two command-line arguments
```

#### Command-Line Arguments

* Operating system will tell the main() function how many command-line arguments were provided in first param (`argc`)
  * We count normally here
* The second param `argv` will the *address* for an *array of pointers* to all of C-style strings
* Each pointer will hold the address of a string that holds one of the command-line arguments


##### Example

Command-line instruction
```
my_prg Assignments Workshops Tests Exam
```

Source code
```cpp
// my_prg.cpp

#include <iostream>

int main (int argc, char *argv[])
{
    int i;

    std::cout << "Application: " << argv[0] << std::endl;
    for (i = 1; i < argc; i++)
        std::cout << "- " << argv[i] << std::endl;
}
```

Output
```
  Application: my_prg
  - Assignments
  - Workshops
  - Test
  - Exam
```


#### Compile-time Evaluations

* Constant Expressions
  * `constexpr` keyword declares a run-time constant and can be evaluated at compile time
* Static Assertions
  * Custom programmer-inserted checks that run during compile time instead of runtime
  * Format is:

```cpp
static_assert(bool condition, const char* message);
```


---


## Week 2 - Types

### [Fundamental Types](https://advoop.sdds.ca/B-Types/fundamental-types)


### Fundamental Types - [Fardad Lecture Jan 12 cont.](https://youtu.be/8LJIMynQkbg?si=lFBwmn_rXi9nyBsq&t=2153)

 * Platform - operating system + compiler = platform
 * `unsigned` types
   * Positive numbers for `signed` int are always 1 less than negative
 * Variables have a circular way of setting things
   * Overflow


#### `void` Type

 * The `void` type is an incomplete type
   * A type is incomplete if it is missing some information. 
   * C++ does not allow the creation of objects of type void.
 * Used for:
   * Functions without a return value
   * Generic pointers


#### Initialization

Initialization Expressions
Example
```cpp
// initializer.cpp

#include <iostream>

int main ()
{
  int n0 = 7;     // C-style
  int n1 = 7.2;   // C-style - narrowing (loss of information)
  int n2 {6};     // universal form braces-enclosed list
  int n3 = {5};   // = is redundant
  int n4 = {5.5};       // @Asam: This won't compile

  // Section from @Fardad
  Student S{ "jack", 234567, 3.5 };     // Universal way of initialization, don't need parenthesis
  int i {};
  int* p {};
  Student X{};

  std::cout << "n0 = " << n0 << std::endl;          // Outputs 7
  std::cout << "n1 = " << n1 << std::endl;          // Outputs 7
  std::cout << "n2 = " << n2 << std::endl;          // Outputs 6
  std::cout << "n3 = " << n3 << std::endl;          // Outputs 5
}
```

#### Type Inference

 * The keyword `auto` specifies type inference
   * `auto` cannot appear in the top-level declaration of an array type.

Example
```cpp
// Type Inference
// auto.cpp

#include <iostream>

int main ()
{
  int a[] {1, 2, 3, 4, 5, 6};
  // auto a[] {1, 2, 3, 4, 5, 6};       // This won't work
  const auto n = 6;

  for (auto i = 0; i < n; i++)
    std::cout << a[i] << ' ';
  std::cout << std::endl;
}
```


### Compilation and Execution - [Fardad Lecture Jan 12 cont.](https://youtu.be/8LJIMynQkbg)

@@



#### Type Alignment 

 * `sizeof` is an operator, not a function
   * Operator from C, not C++
 * `alignof()` operator returns the alignment requirement of its argument type

@@@ Rewatch end of lecture to learn


---


### [Pointers, References and Arrays](https://advoop.sdds.ca/B-Types/pointers-references-and-arrays)

### Pointers, References and Arrays:    Asam Lecture - Jan 13

[Lecture Link](https://senecapolytechnic.zoom.us/rec/play/L9nI3MuVBCg-QkozOekERcyObrWGin0UwuTsCpxvOFMJFIYd4qzS_i_tT-Trv8CggxrmZMjHz6tX.Gs7U2wNury7CBJ0A)

Passcode:   YbCG@0!q

#### References

 * Reference is an alias for an existing object
   * A nickname for an object; objects can have multiple names
 * Pointer is just an address

 * Two reference declarations:
   * lvalue reference - denoted by `&`
   * rvalue reference - denoted by `&&`
   * Both of them are variables and have names, with locations in memory

* lvalue reference - references lvalue expressions
* rvalue reference - references rvalue expressions

Reference Types
 * lvalue reference
 * rvalue reference

Expression Types

* What is an expression?
```cpp
// All of these are expressions
3;
a + b;
```

 * lvalue expression
   * Can be placed legally on the LHS of an assignment
 * rvalue expression
   * Can only be on the RHS of an expression

Example 1:
```cpp
// Illegal
3 = a;          // You can't do that

// Legal
a = 3;          // 3 can ONLY be on RHS
b = a;          // a can be on ANY side
```

Example 2:
```cpp
add(int & lref);
add(int && rref);

a = 3;

add(a);         // Calls lref
add(5);         // Calls rref
```

 * Usually only things on RHS:
   * Literals
     * e.g. 5, 6, 7, 8
   * Temporary Objects
     * e.g. `a = int(5);`
   * Moved Objects
     * TBD

What can be on LHS:
 * Things that can have a name
   * Meaning you can access them
   * AKA have accessible region of memory

#### References - Standard Library

Constructors and Assignment Operators
 * Come as 2 types each:
   * Copy vs. Move versions
   * Copy - for lvalue references
     * therefore being supplied lvalue expression
   * Move - for rvalue references
     * therefore being supplied rvalue expression


---

### Classes and Scoped Enumerations:    Asam Lecture - Jan 13 cont.

```cpp
function(formal parameters);        // declaration

function(arguments);                // invoking
```


```cpp
// Calls copy-assignment
b = a;

// Calls move-assignment
b = std::move(a);

// Calls move-constructor
Array b (std::move(a));
Array b = (std::move(a));
```


Example: Move Constructor 
```cpp
Array( Array && src ) {
    *this = src;            // This would envoke copy assignment operator
}

// Corrected version
Array(Array && src) {
    *this = std::move(src);     // This would correct the move constructor
}
```
* Explanation
  * in `*this = src`, `src` itself is an lvalue expression
    * even though it is being passed as an rvalue reference
      * in this line `Array( Array && src ) {`
  * Since it is an lvalue expression, it would call the COPY assignment operator 
    * instead of the intended MOVE assignment operator
  * To fix this, we set `src` as an rvalue expression using `std::move`
    * Changing the line to `*this = std::move(src);`
  * This makes the move constructor reliant on the move assignment operator


#### Move Operators

Syntax
```cpp
// Prototype for move-constructor
class-name(class-name&&);

// Prototype for move-assignment operator
class-name& operator=(class-name&&);
```

* Steps for Move Assignment Operator
  1. Check for self-assignment
  2. Clean up the resource used by the current instance
  3. Shallow copy
  4. Move the resource from parameter into current instance
     1. Copy address to current object
     2. Set parameter to nullptr
        * The parameter doesn't have the resource anymore


Example:   Move assignment operator
```cpp

```

#### Move Operators

* For an instance of a class **with resources** won't be used again
  * Can move the object's resources by copying their address 
    * instead of copying everything over to new locations

**Prototype for a move-constructor**
```
class-name(class-name&&);
```

**Prototype for a move-assignment operator**
```
class-name& operator=(class-name&&);
```

EVERYTHING
```cpp
// Copy and Move
// copy_move.cpp
#include <iostream>
#include <utility>

class Array
{
    int* a = nullptr;
    unsigned n = 0u;
    int dummy = 0;

public:
    Array(){}
    Array(unsigned no) : a(new int[no]), n(no) {}

    // the COPY constructor
    Array(const Array& src) { *this = src; }

    // the MOVE constructor
    Array(Array&& src) { *this = std::move(src); }

    // the COPY assignment operator
    Array& operator=(const Array& src)
    {
        // 1. check for self-assignment
        if (this != &src)
        {
            // 2. clean-up the resource used by the current instance
            delete [] a;

            // 3. shallow copy
            n = src.n;
            dummy = src.dummy;

            // 4. deep copy
            a = new int[src.n];
            for (unsigned i = 0u; i < src.n; ++i)
                a[i] = src.a[i];
        }
        return *this;
    }

    // the MOVE assignment operator
    Array& operator=(Array&& src)
    {
        // 1. check for self-assignment
        if (this != &src)
        {
            // 2. clean-up the resource used by the current instance
            delete [] a;

            // 3. shallow copy
            n = src.n;
            dummy = src.dummy;

            // 4. move the resource from parameter into current instance
            a = src.a;       // copy address to current object
            src.a = nullptr; // the parameter doesn't have the resource anymore
        }
        return *this;
    }

    ~Array() { delete [] a; }

    int& operator[](unsigned i)
    {
        return n > 0u && i < n ? a[i] : dummy;
    }

    int operator[](unsigned i) const
    {
        return n > 0u && i < n ? a[i] : dummy;
    }

    unsigned size() const { return n; }
};


int main()
{
    const unsigned size = 5;

    Array a(size), b;
    for (unsigned i = 0u; i < a.size(); ++i)
        a[i] = 3 * i;


    std::cout << "Copy-Assignment\n";

    std::cout << "a : ";
    for (unsigned i = 0u; i < a.size(); ++i)
        std::cout << a[i] << ' ';
    std::cout << std::endl;

    b = a; // calls copy-assignment

    std::cout << "b : ";
    for (unsigned i = 0u; i < b.size(); ++i)
        std::cout << b[i] << ' ';
    std::cout << std::endl;

    std::cout << "a : ";
    for (unsigned i = 0u; i < a.size(); ++i)
        std::cout << a[i] << ' ';
    std::cout << std::endl;


    std::cout << "Move-Assignment\n";

    std::cout << "a : ";
    for (unsigned i = 0u; i < a.size(); ++i)
        std::cout << a[i] << ' ';
    std::cout << std::endl;

    b = std::move(a); // calls move-assignment

    std::cout << "b : ";
    for (unsigned i = 0u; i < b.size(); ++i)
        std::cout << b[i] << ' ';
    std::cout << std::endl;

    std::cout << "a : ";
    for (unsigned i = 0u; i < a.size(); ++i)
        std::cout << a[i] << ' ';
    std::cout << std::endl;
}
```

##### == **NOTE**: FOR MORE CHILL COPY CONSTRUCTORS ==

```cpp
class Array {
    int* a = nullptr;           // This is already initialized as nullptr
    unsigned n = 0u;
    int dummy = 0;
public:
    Array(){}
    Array(const Array& src) { *this = src; }    // Copy constructor
    Array& operator=(const Array& src) {        // Copy assignment operator
        if (this != &src)
        {
            // Clean up
            delete [] a;
            // shallow copy
            // deep copy
        }
        return *this;
    }
}
```
* Explanation
  * The copy constructor relies on the copy assignment operator to copy over resources for newly created objects
    * This means if `delete [] a;` runs, it is deleting whatever a is from memory
    * Since `a` is already initialized as null, this is safe and fine
  * If `a` was not already initialized as null, this poses a problem because it would be trying to delete resources from a newly created object
    * Newly created objects do not have resources, so it would be deleting garbage data
      * This can cause an error, so we need to make sure `a` is set to null before `*this = src;` runs from the copy constructor

Error
```cpp
class Array {
    int* a;                   // No longer intialized to null
    unsigned n = 0u;
    int dummy = 0;
public:
    Array(){}
    Array(const Array& src) { *this = src; }    // a needs to be set to null before this runs
    Array& operator=(const Array& src) {
        if (this != &src)
        {
            // Clean up
            delete [] a;      // This line would cause an error
            // shallow copy
            // deep copy
        }
        return *this;
    }
}
```
  * To correct this, we initialize `a` to null prior to the copy constructors call to the copy assignment operator
Solution
```cpp
class Array {
    int* a;                   // No longer intialized to null
    unsigned n = 0u;
    int dummy = 0;
public:
    Array(){}
    Array(const Array& src) { 
        a = nullptr;          // a set to null before call to copy assignment
        *this = src; 
    }
    Array& operator=(const Array& src) {
        if (this != &src)
        {
            // Clean up
            delete [] a;      // No more error
            // shallow copy
            // deep copy
        }
        return *this;
    }
}
```


### Pointers, References and Arrays:    [Reading Notes](https://advoop.sdds.ca/B-Types/pointers-references-and-arrays)


#### Pointer Types:   Null Address Constant
+ *Null Address*
  + Null address cannot be dereferenced
  + `nullptr` refers to the constant that stores this address
  + dereferencing a pointer that holds value `nullptr` causes run-time error

Example
```cpp
int* ptr = nullptr;

// Dereferencing to read
int x = *ptr;   // ❌ runtime error

// Dereferencing to modify
*ptr = 10;      // ❌ runtime error
```

+ *Wild pointer*
  + a pointer that isn't initialized to a valid address.
  + Good practice to initialize to `nullptr`


#### Pointer Types:   Synonym Pointer Types

+ *Synonym Pointer Type*
  + Simplifies repeated definitions of a pointer type
  + Does not need repeated `*` before identifiers

```cpp
// Pointer to synonym type (normal pointer)
typedef unsigned long long int ullint;
ullint* p;

// Synonym pointer type
typedef unsigned long long int* ullint_ptr;
ullint_ptr p; // a pointer to ullint

// Pointer declaration vs. Synonym pointer syntax
unsigned long long int* pp, * qq;  // we need * before each identifier
ullint_ptr p, q;                   // no need for a repeated *
```

#### Pointer Types:   Generic Pointer Types

+ *Generic Pointer Type*
  + Can hold address of an object without holding the objects type information

Syntax:
```cpp
void* p; // generic pointer type
```

Notes:
* Converting any pointer type into generic type (and vice versa) does not lose its address
```cpp
  int i;
  void* v = &i;
  int* j;
  j = static_cast<int*>(v);  // OK - j now holds the address of i
  std::cout << &i << std::endl;
  std::cout << j << std::endl;      // Outputs same address
``` 
* Normally converting from one pointer type to another requires an explicit cast
```cpp
  int* i;
  char* c;
  i = c; // ERROR - Incompatible: Different Pointer Types

  i = static_cast<int*>(static_cast<void*>(c)); // OK
```
* Cannot dereference Generic Pointer
  * To get to the information stored at the address, the type of information is required
  * Must cast generic pointer to the type associated with the address
```cpp
  int i = 42;
  void* v = &i;   // generic pointer, holds address of i

  std::cout << *v << std::endl;   // ERROR

  int* p = static_cast<int*>(v); // cast back to correct type
  std::cout << *p << std::endl;  // Outputs 42
```

#### References:    lvalue and rvalue References

+ *Reference*
  + An **alias** for an existing object
+ *lvalue reference* - denoted by `&`
  + declaration of an lvalue reference identifies an accessible region of memory
+ *rvalue reference* - denoted by `&&`
  + declaration of an rvalue reference identifies:
    + an object near the end of its lifetime
    + a temporary object or subobject
    + a value not associated with an object

#### References:    Standard Library

+ `std::ref()`
  + returns an *lvalue* reference to its argument
+ `std::move()`
  + returns an *rvalue* reference to its argument
+ Note: these prototypes are declared in <utility> header


#### Array Types
* *Array Types*
  * We declare an array type using the `[]` declarator operator. 
  * An array can be constructed from any one of the
    * fundamental types (except void)
    * pointer types
    * pointer to member types
    * class types
    * enumeration types
  * Note that an array cannot be constructed directly from reference types.
* *One-Dimensional Array*
  * Definition
```cpp
Type identifier[ c ];             // allocated on the stack
Type* identifier = new Type[ n ]; // allocated on the heap
```

#### Array Types:   Aggregate Initialization
* We can initialize an array through aggregate initilization
* Syntax
```cpp
Type identifier[ c ] = { initializer-list };
Type identifier[ c ] = { };
Type identifier[ c ] { initializer-list };
Type identifier[ c ] { };
Type identifier[ ] = { initializer-list };
Type identifier[ ] { initializer-list };
Type* identifier = new Type[ n ] { initializer-list };
Type* identifier = new Type[ n ] { };
```
* Example
```cpp
// Aggregate Initialization
// initializers.cpp
#include <iostream>
int main()
{
    const int n = 6;
    int  a[ ] = { 1,2,3 };
    int  b[ ] { 1,2,3 };
    int  c[5] = { 1,2,3 };
    int  d[5] = {};
    int  c[5] { 1,2,3 };
    int  d[5] {};
    int* f = new int[n]{ 1,2,3 };
    int* g = new int[n]{};
}
```

#### Array Types:   Range-Based `for`

+ *range-based `for`* 
  + an iteration construct specifically designed for use with collections
  + collection type must carry information about its size or a mechanism to detect when the boundary has been reached
    + statically-allocated array types carry such information, but pointers don't
  + can infer the type of each element in the array from the array declaration itself
```cpp
// for_each.cpp
int main ()
{
    int a[]{1, 2, 3, 4, 5, 6};

    for (int& e : a)
        std::cout << e << ' ';
    std::cout << std::endl;

    // auto
    for (auto& e : a) // the type of an element will be inferred by the compiler
        std::cout << e << ' ';
    std::cout << std::endl;
}
```

### Classes and Scoped Enumerations:    Asam Lecture - Jan 14

[Lecture Link](https://senecapolytechnic.zoom.us/rec/play/Z0xuuRRl5nKEiiPFcNCg3BhCe2WIi05LYyuHP8KZbrqtD_txNxuWl53OuYwdEkDujH8aIm3q8IZYhO1a.AW7Ek7m8Eu54sDeY)

Passcode:   9d$N5J8t

##### Practice Exercise
```cpp
class A {
    int xyz;
public:
    A() { std::cout << " Class-A Default Constructor" << std::endl; };
    A(int num) : xyz(num) {};
    A(const A& src) { std::cout << " Class-A Copy Constructor - L-value " << std::endl; };
    A(const A&& src) { std::cout << " Class-A Move Constructor - R-value " << std::endl; };
    A& operator=(const A& src) { std::cout << " Class-A Copy Assignment - L-value " << std::endl; };
    A& operator=(const A&& src) { std::cout << " Class-A Move Assignment - R-value " << std::endl; return *this; };
};

class X {
    int ddd;
public:
    X(int num) : ddd(num) {};
    X() { std::cout << " Class-X Default Constructor " << std::endl; };
    X(const X& src) { std::cout << " Class-X Copy Constructor - L-value " << std::endl; };
    X(const X&& src) { std::cout << " Class-X Move Constructor - R-value " << std::endl; };
    X& operator=(const X& src) { std::cout << " Class-X Copy Assignment - L-value " << std::endl; };
    X& operator=(const X&& src) { std::cout << " Class-X Move Assignment - R-value " << std::endl; return *this; };
    A bb;
};

A s;
A b;

A foo() { return b; }
A& bar() { return s; }

int main() {
    std::cout << "********************     [1]     ********************" << std::endl;
    s = A(20);              // 1

    std::cout << "********************     [2]     ********************" << std::endl;
    s = foo();              // 2

    std::cout << "********************     [3]     ********************" << std::endl;
    A s2 = bar();           // 3

    std::cout << "********************     [4]     ********************" << std::endl;
    s = X().bb;             // 4
}
```
* Questions:
  * Code 1.0 Line highlighted as 1 in main function will print:
    * Class-A Move Assignment - R-value
    * Class-A Move Assignment - L-value
    * Class-A Copy Assignment - R-value
    * Class-A Copy Assignment - L-value
    * All of the above
    * None of the above
    * 
  * Code 1.0 Line highlighted as 2 in main function will print:
    * Class-A Move Assignment - R-value
    * Class-A Move Assignment - L-value
    * Class-A Copy Assignment - R-value
    * Class-A Copy Constructor - L-value
    * Answers 'A' & 'B'
    * Answers 'D' & 'A'
    * All of the above
    * None of the above
    * 
  * Code 1.0 Line highlighted as 3 in main function will print:
    * Class-A Move Assignment - R-value
    * Class-A Move Assignment - L-value
    * Class-A Copy Assignment - R-value
    * Class-A Copy Constructor - L-value
    * Answers 'A' & 'B'
    * Answers 'D' & 'A'
    * All of the above
    * None of the above
    * 
  * Code 1.0 Line highlighted as 4 in main function will print:
    * Class-A Default Constructor
    * Class-X Default Constructor
    * Class-A Move Assignment R-Value
    * All of the above
    * None of the above
  * Solution:
    1. Class-A Move Assignment - R-value
    2. Answers 'D' & 'A'
    3. Class-A Copy Constructor - L-value
    4. All of the above

##### Practice Exercise Takeup

s = A(20);

 * `s` is already created as a global object
 * `A(20)` is creating a new object
   * It is a temporary object; we're not giving it a name
   * Uses a single-argument constructor
     * Nothing printed
 * `s.operator = (A(20))`
   * This means we're supplying `s` with a temporary object
     * **Temporary objects are rvalue**
   * Therefore between lvalue vs. rvalue assignment:
 * Prints:
   Class-A Move Assignment - R-value


s = foo();

 * `s` is already created
 * `foo()` returns `A(b)`
   * `b` is already created

 * `A(b)` is `A(src)`
   * Therefore a constructor
   * `b` is lvalue expression
 * Prints:
   Class-A Copy Constructor - L-value

 * `s = A(b)`
   * This is a `s.operator=(A(b))`
     * Therefore an assignment operator
   * `b` is lvalue but
     * `A()` is rvalue since it is a temporary object
     * Therefore `A(b)` is rvalue expression
 * Prints:
   Class-A Move Assignment - R-value


A s2 = bar();

 * First evaluate `bar()`
   * `A& bar() { return s; }`
     * `s` already exists
     * Since we return by reference, we're essentially left with
 * `A s2 = s`
   * We're constructing a new instance named `s2`
     * This means we're using a constructor
   * Since `s` is an already existing object
     * This means `s` is an lvalue expression
 * Prints:
   Class-A Copy Constructor - L-value


s = X().bb;

 * First evaluate `X().bb`
   * This is creating a temporary object of type `X`
     * First part of creating an object is creating its data members
     * Must create `A bb;` first
       * This uses `A()` default constructor
 * Prints:
   Class-A Default Constructor
   * After `A bb` is created, call `X` constructor
     * `X()` uses default constructor
 * Prints:
   Class-X Default Constructor
 * `s = X().bb;`
   * Assignment time
     * Assignment means assignment operator
     * Assigning `bb` into `s`
       * Therefore assigning `s` to `class A`
       * `bb` is inside `X()`
         * `X()` is a temporary object
         * If `bb` is inside a tempoary object, then `bb` is ALSO temporary
       * Therefore `s` is being assigned to a temporary object
         * This indicated rvalue expression == MOVE
 * Prints:
   Class-A Move Assignment - R-value



### [Classes and Scoped Enumerations](https://advoop.sdds.ca/B-Types/classes-and-scoped-enumerations)

> ==Reminder from W1 - Overview:==
> + *User-defined Types*
>   + *Concrete Types*
>     + their representation is part of their definition and is known
>   + *Abstract types*
>     + their representation is not part of their definition and is unknown

+ *User-Defined Types*
  + types that we construct from fundamental, built-in and possibly other user-defined types
  + Divided into two groups
    + *Class types*
      + represent data that has its own dedicated logic
      + encapsulate their own logic, protect their objects' data through access modifiers and manage access to that data through public member functions.
    + *Enumeration types*
      + represent sets of discrete values using symbolic names, which makes the source code more readable and less error-prone.

#### Class Basics

+ *Class*
  + a set of types
+ *Array*
  + a set of types where data types are identical
+ *Members*
  + data and function types in a class

* Three class-keys
  + `class`
    + members are **private** by default
  + `struct`
    + members are public by default
  + `union`
    + members are public by default

#### Class Basics:    Declarations and Definitions

**Class Definitions**
```
class-key Identifier
{
    sub-object declarations

    member function declarations
};
```

**Sub-Object Declarations**
```cpp
class Subject
{
    unsigned number;
    char desc[41];
    Subject* prerequisite; // OK
    Subject subject        // ERROR - keeps creating subjects in subjects
};
```

**Forward Declarations**
```
class-key Identifier;
```

**Instance Definition**
```
class-key Identifier
{
	sub-object declarations

	member function declarations
} object;
```
* Where `object` is the name of the instance of type `Identifier`.

#### Class Basics:    Data Member Initialization

* We can initialize modifiable non-static data members in three ways:
  * in the member declaration (default member initializers)
  * in the member-list initializers when implementing the constructor
  * in the constructor body

**Default Member Initializers**
Syntax
```cpp
class-key Identifier
{
    sub-object declaration   {initial value}; // braces-enclosed - type-safe
    sub-object declaration = {initial value}; // braces-enclosed - type-safe
    sub-object declaration =  initial value;  // equals initializer
    sub-object declaration   {initial value, ... }; // braces-enclosed list
    sub-object declaration = {initial value, ... }; // braces-enclosed list

    member function declarations
};
```
Example
```cpp
class Student
{
    int grades {0};                    // braces-enclosed - type-safe
    int grades = {0};                  // braces-enclosed - type-safe
    int grades = 0;                    // equals initializer
    int grades[3] {85, 90, 78};        // braces-enclosed list
    int grades[3] = {85, 90, 78};      // braces-enclosed list
};
```
== **Note**: Must include size of array for class members ==

**Member List Initializers**
* FOR CONSTRUCTORS to initialize data members directly
```cpp
Class-name(type x, ...) : data-member-name{x},...
{
	// logic
}
```

#### Class Basics:    Copying

Copy Construction and Copy Assignment
== **Note**: THIS EXAMPLE KINDA MESSED WITH MY, CHECK READINGS ==
```cpp
// Copy-Construction, Copy-Assignment and Subscripting Operator
// copy_assign.cpp
#include <iostream>

class Array
{
    int* a = nullptr;
    unsigned n = 0u;
    int dummy = 0;

public:
    Array(){}
    Array(unsigned no) : a(new int[no]), n(no) {}
    Array(const Array& src) { *this = src; }

    Array& operator=(const Array& src)
    {
        if (this != &src)
        {
            delete [] a;
            n = src.n;
            dummy = src.dummy;
            a = new int[src.n];
            for (unsigned i = 0u; i < src.n; ++i)
                a[i] = src.a[i];
        }
        return *this;
    }

    ~Array() { delete [] a; }

    int& operator[](unsigned i)
    {
        return n > 0u && i < n ? a[i] : dummy;
    }

    int operator[](unsigned i) const
    {
        return n > 0u && i < n ? a[i] : dummy;
    }

    unsigned size() const { return n; }
};


int main()
{
    const unsigned size = 5;
    Array a(size), b;

    for (unsigned i = 0u; i < a.size(); ++i)
        a[i] = 3 * i;

    for (unsigned i = 0u; i < a.size(); ++i)
        std::cout << a[i] << ' ';
    std::cout << std::endl;

    b = a;

    for (unsigned i = 0u; i < b.size(); ++i)
        std::cout << a[i] << ' ';
    std::cout << std::endl;
}
```

* This example also shows how to code lvalue and rvalue versions of the subscripting operator ([]).

#### Class Basics:    Anonymous Classes

* Single use classes
* Doesn't have an `identifier`
* When defining an anonymous type, must include either:
  * instance's identifier or synonym name

```cpp
// defining an instance of an anonymous type (name)
struct // definition - no tag
{
    char shortName[7];
    char fullName[41];
} name;       // Name this instance

// declaring a synonym type (Course)
typedef struct // definition - no tag
{
    unsigned number;
    char desc[41];
} Course;     // "Course" is a synonym for the structure

Course c;     // Use course
```

### Classes and Scoped Enumerations:    Asam Lecture - Jan 14 cont.

[Lecture Link](https://senecapolytechnic.zoom.us/rec/play/Z0xuuRRl5nKEiiPFcNCg3BhCe2WIi05LYyuHP8KZbrqtD_txNxuWl53OuYwdEkDujH8aIm3q8IZYhO1a.AW7Ek7m8Eu54sDeY)

Passcode:   9d$N5J8t

#### Class Members:       Class Variables

* *Class Variable*
  * A class variable lasts the lifetime of the program and holds a value that all instances of the class share
  * The keyword `static` declares a variable in a class definition to be a class variable
  * Initialized in the implementation file

* QT2. Class Variable:
  1. Holds a value that all instances of the class can share.
  2. Can still be accessed even if there is no instance of the class
  3. The keyword `static` declares a class variable
  4. We define and initialize the class variable in the implementation file
  5. Good application is to use it as a counter to track how many objects instantiated but not yet destroyed

Example
```cpp
// Class Variables - Header
// classVariable.h

class Horse
{
    unsigned age;             // <-- this is an instance variable
    unsigned id;              // <-- this is an instance variable
public:
    static unsigned noHorses; // <-- this is a class variable

    Horse(unsigned a);
    ~Horse();
};

// Class Variables - Implementation
// classVariable.cpp

unsigned Horse::noHorses = 0;     // this is how class variables are initialized

// the constructor increments the class variable, but is not initialize it
Horse::Horse(unsigned a) : age{a}, no{++Horse::noHorses} {}

// the destructor decrements the class variable
Horse::~Horse() { --Horse::noHorses; }
```

* Note: 
  * In this line:
    `Horse::Horse(unsigned a) : age{a}, no{++Horse::noHorses} {}`
    * Class name and scope resolution is included
    * This is the only way to access it
      * Since the variable belongs to the class, not the instance
  * Also this data member is public which breaks encapsulation
    `static unsigned noHorses;`


#### Class Members:       Class Functions
* *Class Functions*
  * A class function provides access to private class variables. 
  * To identify a class function, we preface its declaration in the definition with the keyword `static`
    * The keyword `static` declares a function in a class definition to be a class function. 


#### Good Asam Questions
```cpp
  int n0 = 7;   // C-style
  int n1 = 7.2; // C-style - narrowing (loss of information)
  int n2 {6};   // universal form braces-enclosed list
  int n3 = {5}; // = is redundant
  int n4 = {5.5};       // @Asam: This won't compile
```
Safe initialization needs to match type.

Code 12.0
```cpp
int foo(10);
auto bar = std::ref(foo);
++bar;
++foo;
std::cout << foo << '\n';
```

Code 13.0
```cpp
int foo(10);
int bar;
bar = std::ref(foo);
++bar;
std::cout << foo << '\n';
std::cout << bar << '\n';
```

Code 14.0
```cpp
int a[]{1, 2, 3, 4, 5, 6};
for (auto e : a) {
    e += 2;
}
for (auto& e : a) {
    e++;
}
for (auto& e : a) {
    std::cout << e << '';
}
std::cout << std::endl;
```

Code 17.0
```cpp
void func_ranges0(){
    unsigned char x = 0;
    unsigned char y = 150;
    x = 2*y;
    std::cout << " x = " << (int)x << std::endl;
}
```

---

## Week 3 - Class Relationships
### Inheritance, Inclusion, and Polymorphism:   Asam Lecture - Jan 21
[Jan 21 Lecture](https://senecapolytechnic.zoom.us/rec/play/6eoDJJ6emZkreZQbqC6r06pE9RAa9SkPATiDX_usbXU9yEs6Jol5XkYRjlXkjzs85Re21iBjw7Al3X_q.58w7964mPk_gmoNG)

Passcode:   Qg1ABh**

#### Good Asam Questions
A derived class in an inheritance hierarchy:
    A. Includes the entire structure of its base class
    B. Defines those additional features that specialize its base class

An abstract class:
    A. Base class where some or all functions are declared purely virtual
    B. Cannot be instantiated
    C. It is an incomplete class in the hierarchy

If a derived class allocates resources, it needs own destructor to release those resources. To ensure that the executable code always calls this destructor, we declare the base class destructor virtual?


Which statement is correct:
    A. Parametric polymorphism allows functions. definitions that share identical logic independently of type. The logic is common to all possible types, without restriction. The types do not need to be related in any way.
    B. By defining that structure in generic form, we reduce code duplication. Class and function templates serve this purpose.
    C. The compiler generates the class and function definitions from our tempaltes for those types that we specify explicitly.
    D. All of the above




### Class Templates:      Asam Lecture - Jan 27

[Jan 21 Lecture](https://senecapolytechnic.zoom.us/rec/play/-xhLkpt2Fo4mzhO9OYRy2MJBlg5_dk89a9h_AzY61rpl1_kpg7aEhPIkvjHFHH9JrKw3WBO30biSJJ38.f8RZk8QYKLLbpBdI)
Passcode:   3MIfZ=*W

#### SIDE BAR OLD NOTES:    Template Basics   
[Fardad Lecture Notes - Mar 25](https://www.youtube.com/watch?v=g_tOvmbAk3E)

##### Function Template

Syntax
```cpp
template<Type identifier[, ...]>
```
* Where:
  * `Type` can be 
    * `typename` or `class`
    * `int`, `long`, `short`, `char`
  * `identifier` is whatever placeholder for argument


Example: Convert function to template
```cpp
// Original function
int displaySum(int f, int s) {
    int sum = f + s;
    cout << "sum: " << sum << endl;
    return sum;
}

// Template format
template <typename type>
type displaySum(type f, type s) {
    type sum = f + s;
    cout << "sum: " << sum << endl;
    return sum;
}

// Calling template
int main() {
    double a = 2.3, double b = 4.5;
    displaySum(a, b);
}
```

* Notes:
  * You cannot modularize a template
    * Templates only have one file, and it's the header file.
      * This means template definitions are in the header file
    * Compiler needs the entire code; it creates templates at compile time.
      * If compiler finds it needs to create it, it will create it as needed
  * You can overload templates
    * Overloads take priority over templates
      * Template only comes into play if the function doesn't already exist


#### Function Templates:  Specialization
```cpp
// Template Specialization
// maximum.h

#include <iostream>
#include <cstring>

template <typename T>
T maximum(T a, T b)
{
    std::cout << "in template body\n";
    return a > b ? a : b;
}

// specialization for char* types
//
template <> // denotes specialization
const char* maximum<const char*>(const char* a, const char* b)
{
    std::cout << "in specialization\n";
    return std::strcmp(a, b) > 0 ? a : b;
}
```
#### OLD NOTES:           Class Templates 
[Reading Notes - Templates](https://intro2oop.sdds.ca/E-Polymorphism/templates)

Example: Convert class to template
```cpp
// Original class
class Array {
  int arr[SIZE];
public:
    int& operator[](int i) { return arr[i] }
};

// Template format
template <class type, int SIZE>
class TemplateName {
  type arr[SIZE];
public:
    type& operator[](int i) { return arr[i]}
};


// Calling template
int main() {
  TemplateName<int, 5> a, b;      
    // This makes 2 arrays
    // Both hold 5 ints
    // One named a, another named b
}
```
  
#### OLD NOTES:           Overloading vs. Specialization
[Fardad Lecture Notes - Mar 22 '24](https://www.youtube.com/watch?v=AA5DMK45Ylw)

Example
```cpp
// Original template
template <typename type>
type displaySum(type f, type s) {
    type sum = f + s;
    cout << "sum: " << sum << endl;
    return sum;
}
// Overload for characters
char* displaySum(char* f, char* s) {
    char* res{};
    if (f && s) {
        res = new char[ut.strlen(f) + ut.strlen(s) + 1];
        ut.strcpy(res, f);
        ut.strcat(res, s);
    }
    return res;
}

// Template Specialization
template <>
char* displaySum<char*>(char* f, char* s) {
    char* res{};
    if (f && s) {
        res = new char[ut.strlen(f) + ut.strlen(s) + 1];
        ut.strcpy(res, f);
        ut.strcat(res, s);
    }
    return res;
}

// Calling Template
int main() {
    char* res;
    char s1[20] = "Firstname";
    char s2[20] = "Lastname";

    res = displaySum(s1, s2);
    cout << "displaySum func returned: " << res << endl;
    delete[] res;
}
```

@@@

## Week 4 - Processing

### Expressions:          Asam Lecture - Jan 28
[Jan 28 - Lecture](https://senecapolytechnic.zoom.us/rec/play/apiCD8ik3xG5MCkzwwkRQQaIc4r28F_bKElr9X2UpbffDLRInqQVg_r2cWTOxtLXs08dcVlgjAvSUYlP.YjT6_hyV3W9bPYau)
Passcode:     9@S35i81

+ prvalue - a value that does not occupy a location in storage
+ xvalue - an expiring value that does occupy a location in storage (an object near the end of its lifetime)
+ lvalue - a locator value that occupies a location in storage

#### OLD NOTES:           Constrained Casts
[Reading Link](https://intro2oop.sdds.ca/E-Polymorphism/templates#constrained-casts)

*Constrained Casts*:
* Improve type safety
* 4 Types:
  * `static_cast<Type>(expression)`
  * `reinterpret_cast<Type>(expression)`
  * `const_cast<Type>(expression)`
  * `dynamic_cast<Type>(expression)`

* `static_cast<Type>(expression)`
   * Good for related types
     * e.g. `int` to `double`
   * Most common

* `reinterpret_cast<Type>(expression)`
   * Unrelated types
     * e.g. `int` to pointer
   * Don't use this it's kinda whack

* `const_cast<Type>(expression)`
   * Removes const when passing const variable to function that doesn't accept const
   * Use only when you're sure function doesn't modify value

* `dynamic_cast<Type>(expression)`
   * For polymorphism
   * Use when casting within an inheritance hierarchy and unsure if cast is valid
   * Returns `nullptr` when cast fails, so you can check if successful
   * STELLA DEFINITION:
     * This is like a boolean checker to see which class an object comes from
     * If it's coming from the class we want, it'll return a pointer to that type
     * If it doesn't we get a nullptr.
   * Overlord's version of my definition:
     * "Try to treat this object as type X - if it works, give me a pointer to use it as X, if not, give me nullptr"


## Week 4 - Class Relationships

### Compositions, Aggregations, and Associations:   Asam Lecture - Jan 28 cont.
[Jan 28 Lecture](https://senecapolytechnic.zoom.us/rec/play/apiCD8ik3xG5MCkzwwkRQQaIc4r28F_bKElr9X2UpbffDLRInqQVg_r2cWTOxtLXs08dcVlgjAvSUYlP.YjT6_hyV3W9bPYau)
Passcode:     9@S35i81


@@@


## Week 5 - Processing
### Functions:              Asam Lecture - Feb 3
[Feb 3 Lecture](https://senecapolytechnic.zoom.us/rec/play/zmsb1pFnADSAVMt6Rl1nUZhU7fKe2FzLh1bgGSH8FiqaJMqT3_EJ1E8xcGUMTSBKuho3YEK1_6N6zq7o.Vkedl_987uV6lQAj)

Passcode:     T298@Kip





### Asam Lecture - Feb 4
[Feb 4 Lecture - 30m](https://senecapolytechnic.zoom.us/rec/play/wD0o5tgv3EzikR7QWPmq69PNXij6cNUXKF_Kjlsdh_5VXCAMeWTw-qIbM_nko7EXsds1eZC_AgHac4bf.kx1bVgtHVgwhN04_)

Passcode:       .?5Y1q1R




### Asam Lecture - Feb 10
[Feb 10 Lecture - 1h](https://senecapolytechnic.zoom.us/rec/play/IzMSfbEifoiqIwSZAoUS8UA2_CAGD9kH1ntr4lkemmqUx6e8bR3lQvQx9XOBiaezoSlIBY4pJ6NOn29R.tutm22SnVVa-uCRx)

Passcode:       kNn!jLL9









## Midterm Topics
* Major topics
  * Copy semantics
  * Move semantics
  * Class functions
  * Inheritance
  * Polymorphism
  * Expressions
  * Composition, Aggregation, Associations








### Asam Lecture - Mar 3
[Mar 3 Lecture - 1h 10m](https://senecapolytechnic.zoom.us/rec/play/JR46DGbix9NL1WJGJzfdZKqz7xkGM_AMWYRt9arMeuZ7wZZcCEb97rBXLV67nc6PWZW2hKfT3Sf2qrcR.OvzOpBGTutUd7qks)

Passcode:       ?T6=1.2w







### Asam Lecture - Mar 10
[Mar 10 Lecture - 1h](https://senecapolytechnic.zoom.us/rec/play/wmw5e2davfUNIkZYA2mMgB2nZE6XpobqG86YGgjBhP1bZViYe1Ifg5S-bL4fU9pOFJVsX0VADBGtkFc.qyZcEuHIdN5a6tKK)

Passcode:       %AdHi3Mn







### Asam Lecture - Mar 17
[Mar 17 Lecture - 1h](https://senecapolytechnic.zoom.us/rec/play/p-3J-3GgwQN69oel9Mo-bjkj0KdNgUkwxqIK2eZ7J_onCpgdUipMOU_miyTgsz-alLy8_epuU-LWYfhH.I9q9ZYYypZ0CGk1T)

Passcode:       #7d3BwI6






### Asam Lecture - Mar 24
[Mar 24 Lecture - 50m](https://senecapolytechnic.zoom.us/rec/play/RskX94x4z-r0hzAl-1DOC1GcWXvhONxq8hdEAGrNxYMEQtqlTJGfZDyxM0lwfCobLcwfvdn1Cs3ep9-C.VIQpq4csvIrtA4Gm)

Passcode:       A8B4+P8y






### Asam Lecture - Mar 31
[Mar 31 Lecture - 1h](https://senecapolytechnic.zoom.us/rec/play/uhRy4AwhIn3W6yUCiJjkQs0PrVsvIe6RNwCd8Y3Q-40C5J5BVLaO8ZfKrEb4CEWOjLZriRZygaw00HWT.-ydx_M6L3D8Ycj6G)

Passcode:       EWs=gy2t






### Asam Lecture - Apr 7
[Apr 7 Lecture - 1h 10m](https://senecapolytechnic.zoom.us/rec/play/1WR04VPkZDZ73fzF33qskT6C6-Uk1NyLSlJbnSdAs1vDk96YYsey5CFVFyVwCq4daI5SphMjCbaFdgmS.FlHgNAITyvQyL-DU)

Passcode:       Em$ZhC15






### Asam Lecture - Apr 14
[Apr 14 Lecture - 1h](https://senecapolytechnic.zoom.us/rec/play/SpwgIi7sCw_odt_dVvM0ZBjrdcgJBEKB7_ITk5V14BY-s4MgjvOvmVQ-ShyfHXE1GpVrg6PLBByl6sLt.citdk_xcBQhDvfIc)

Passcode:       #qStA3Db


















## Week 2 - Appendices
### [String Class]()

The constructors of the `string` class can initialize an object using:

1. a C-style null-terminated string
2. a C-style null-terminated substring
3. another `string` object
4. a substring of another `string` object
5. a sequence of characters

```cpp
string a = "Hello";           // just like assigning a C-style string
string b("Good Bye", 4);      // takes only the FIRST 4 characters → "Good"
string c = a;                 // copies the contents of a → "Hello"
string d(b, 5, 3);            // starts at index 5 of b, takes 3 characters → "Bye"
string e(a.begin(), a.end()); // uses iterators to copy all of a → "Hello"
```





