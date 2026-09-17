# OOP345 - General Notes:   Dictionary


## Week 1 - Introduction

Topics: Introduction
 * Overview
 * C++ Building Blocks
 * Compilation and Execution


### [Overview](https://advoop.sdds.ca/A-Introduction/overview)

#### Object-Oriented Paradigm
*Encapsulation*
*Inheritance*
*Polymorphism*

#### Building Blocks
+ *Object*
  + An object occupies a region of memory, holds a value and may have a name, but does not necessarily have a name.
+ *Variable*
  + A variable is a named object. It occupies a region of memory, holds a value and has a name.
+ *Value*
  + The value that an object holds is the contents of the memory region allocated to the object and defines its current state.

##### Types
+ *Fundamental types*
  + correspond directly to the hardware facilities
+ *Built-in Types*
  + reflect the capabilities of the hardware facilities directly and efficiently
+ *User-defined Types*
  + *Concrete Types*
    + their representation is part of their definition and is known
  + *Abstract types*
    + their representation is not part of their definition and is unknown

##### Declarations:  Scope
+ *Local scope*
  + the name has been declared within a function or code block
+ *Class scope*
  + the name has been declared as a member of a class
+ *Namespace scope*
  + the name has been declared as a member of a named block
+ *Global scope*
  + the name has not been declared in any one of the above scopes

##### Declarations:  Linkage
+ *Linkage*
  + A name has a linkage if it can refer to an identical name declared in another scope.
+ *External Linkage*
  + connected across different scopes in different modules 
+ *Internal Linkage*
  + connected across different scopes within the same module 
+ *Non-Existent Linkage*
  + not connected to any entity outside its own scope 

#### Compilers
##### Compiling, Linking and Executing
+ *Compile-time*
  + during the compiler's translation of a module's source code into binary code
+ *Link-time*
  + during the linker's assembly of the binary code components for the modules that constitute an application, or
+ *Run-time*
  + during the user's execution of the binary executable.

###### Statically and Dynamically Allocated
In these notes, the terms *statically* and *dynamically* distinguish what can be determined at compile-time from what needs to be determined at run-time.

+ *Statically*
  + The term *statically* refers to anything that the compiler itself can determine for the module being translated without any link-time or run-time information. As programmers, we seek to translate and type-check as much of the source code as possible at compile-time. We refer to this as static type-checking and refer to a language that performs type-checking at compile-time as a statically typed language.
+ *Dynamically*
  + The term *dynamically* refers to anything that is determined during execution. As programmers, we refer to type-checking of dynamically allocated memory as dynamic type-checking and refer to a language that performs type-checking at run-time as a dynamically typed language.

###### Memory Distinctions
+ *Code segment*
  + stores the program instructions
+ *Data segments*
  + store data that survives the lifetime of the program
+ *Stack segment*
  + stores local data that is statically allocated
+ *Heap segment*
  + stores local data that is dynamically allocated

---

### [C++ Building Blocks](https://advoop.sdds.ca/A-Introduction/cpp-building-blocks)

#### Declarations:  Definitions
+ *Declaration*
  + Declarations are the introduction
+ *Definition*
  + Definitions allocate memory (for variables) or provide a body (for classes/functions)

#### Declarations:  Linkage
+ *External linkage*
+ *Internal Linkage*

#### Declarations:  Type
+ *cv Qualifiers*
  + none
    + the type is modifiable and not subject to any side-effects
  + `const`
    + the type is unmodifiable and not subject to any side-effects
  + `volatile`
    + the type is modifiable and subject to some side-effects
  + `const volatile`
    + the type is unmodifiable and subject to some side-effects

##### Type Definition
+ *Type definition*
+ `typedef`
  + keyword identifies a synonym for a specified type.
  + Syntax:     `typedef compoundType Synonym;`
  + e.g.        `typedef const int constInt;`

#### Lifetime in Memory
##### Variables and Objects
+ *Subobject*
+ *Complete object*
  + A complete object is an object that is not part of any other objec

##### Storage Duration
+ *Storage Duration*
  + The storage duration of a variable or object defines its lifetime within a program
+ *Automatic*
  + lasts from its declaration to the end of its scope - no keyword
+ *Static*
  + lasts the entire lifetime of the program - use keyword `static`
+ *Dynamic*
  + created using the keyword `new` - last until deallocated using `delete`
+ *Thread*
  + lasts the lifetime of the thread - use keyword `thread_local`


---

### [Compilation and Execution](https://advoop.sdds.ca/A-Introduction/compilation-and-execution)

#### Compilation Process
+ *Pre-Processing Stage*
  + the pre-processor creates a separate translation unit from the original source files for each module by inserting the header files into the implementation file and replacing or expanding any macros (lines starting with #)
+ *Compilation Stage*
  + the compiler creates a separate binary file from each translation unit
+ *Linking Stage*
  + the linker creates a single relocatable file from the binary files for all translation units and the binary files for any referenced libraries.

#### Compile-Time Evaluations
+ *Constant Expressions* - `constexpr`
  + keyword declares a run-time constant and can be evaluated at compile time
+ *Static Assertions* - `static_assert()`
  + Checks that run during compile time instead of runtime.
  + If the condition is false, compilation fails and the compiler prints custom message.
  + Condition must be a constant expression.
  + Syntax:     `static_assert(bool condition, const char* message);`

## Week 2 - Types

### [Fundamental Types](https://advoop.sdds.ca/B-Types/fundamental-types)

#### Integer Types

+ standard integers; either
  + `signed` standard integers, or
  + `unsigned` standard integers
+ booleans
+ character types

+ *Range Specifiers*
  + `signed`
    + negative and positive values
  + `unsigned`
    + no negative values
+ *Standard Signed Integers*
  + `signed char`
  + `short int`
  + `int`
  + `long int`
  + `long long int`
+ *Character Types*
  + `char`
  + `signed char`
  + `unsigned char`
  + `wchar_t`
  + `char16_t`
  + `char32_t`

#### Floating-Point Types
+ `float`
  + a single-precision floating-point number
+ `double`
  + a double-precision floating-point number
+ `long double`
  + a double-precision floating-point number, possibly with extra precision

#### `void` Type, Type Inference, Type Alignment
+ *`void` Type*
  + an incomplete type. A type is incomplete if it is missing some information. 
  + C++ does not allow the creation of objects of type void.
  + Use for the return type of functions that do not return a value and for generic pointers.
+ *Type Inference* - `auto`
  + compiler can infer the type of an object from a previously declared object
  + keyword `auto` specifies type inference.
  + Note that `auto` cannot appear in the top-level declaration of an array type.
+ *Type Alignment* - `alignof()`
  + Types can be aligned in memory at different spacings
  + `alignof()` operator returns the alignment requirement of its argument type


---

### [Pointers, References and Arrays](https://advoop.sdds.ca/B-Types/pointers-references-and-arrays)

#### Pointer Types
+ *Null Address*
  + Null address cannot be dereferenced
  + `nullptr` refers to the constant that stores this address
  + dereferencing a pointer that holds value `nullptr` causes run-time error
+ *Wild pointer*
  + a pointer that isn't initialized to a valid address.
  + Good practice to initialize to `nullptr`
+ *Synonym Pointer Type*
  + Simplifies repeated definitions of a pointer type
  + Does not need repeated `*` before identifiers
+ *Generic Pointer Type* - `void* p`
  + Can hold address of an object without holding the objects type information
  + Converting any pointer type into generic type (and vice versa) does not lose its address
  + Syntax:     `void* p;`

#### References
+ *Reference*
  + An **alias** for an existing object
+ *lvalue reference* - denoted by `&`
  + declaration of an lvalue reference identifies an accessible region of memory
+ *rvalue reference* - denoted by `&&`
  + declaration of an rvalue reference identifies:
    + an object near the end of its lifetime
    + a temporary object or subobject
    + a value not associated with an object

Standard Library
+ `std::ref()`
  + returns an *lvalue* reference to its argument
+ `std::move()`
  + returns an *rvalue* reference to its argument
+ Note: these prototypes are declared in <utility> header

#### Array Types
+ *range-based `for`* 
  + an iteration construct specifically designed for use with collections
  + collection type must carry information about its size or a mechanism to detect when the boundary has been reached
    + statically-allocated array types carry such information, but pointers don't
  + can infer the type of each element in the array from the array declaration itself

### [Classes and Scoped Enumerations](https://advoop.sdds.ca/B-Types/classes-and-scoped-enumerations)

+ *User-Defined Types*
  + types that we construct from fundamental, built-in and possibly other user-defined types
  + Divided into two groups
    + *Class types*
      + represent data that has its own dedicated logic
    + *Enumeration types*
      + represent sets of discrete values using symbolic names, which makes the source code more readable and less error-prone.








