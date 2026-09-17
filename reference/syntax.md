# OOP345 - General Notes:   Syntax

## Week 1 - Introduction:   C++ Building Blocks

> **Reminder: Declarations vs. Definitions**
> Declarations are the introduction
> Definitions allocate memory (for variables) or provide a body (for classes/functions)


### External Linkage

```cpp
extern int share_me;    // declaration
int share_me = 0;       // definition
```

### Internal Linkage

```cpp
static int local = 2;
```

### Type Definition

Syntax
```cpp
typedef compoundType Synonym;
```

Example
```cpp
typedef const int constInt; // defines the const int type as a constInt
constInt myConstant;        // myConstant is a const int
```




## Week 1 - Introduction:   Compilation and Execution


### Interface with the Operating System

```cpp
int main();                       // no command-line arguments
int main(int argc, char *argv[]); // two command-line arguments
```


### Compile-time Evaluations:   Static Assertions

Syntax
```cpp
static_assert(bool condition, const char* message);
```

Example
```cpp
constexpr int SIZE = 10;

static_assert(SIZE > 0, "SIZE must be positive");
```

---


## Week 2 - Types:      Fundamental Types

+ *Range Specifiers* - `signed` vs. `unsigned`
+ *`void` Type*
+ *Type Inference* - `auto`
+ *Type Alignment* - `alignof()`


## Week 2 - Types:      Pointers, References and Arrays

### Pointer Types
#### Null Address Constant

Example: 
```cpp
int* ptr = nullptr;
int x = *ptr;               // runtime error
*ptr = 10;                  // runtime error
```

#### Synonym Pointer Type

Syntax
```cpp
typedef type* typeName_ptr;
typeName_ptr ptrName;
```

Example
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

#### Generic Pointer Type

```cpp
void* p; // generic pointer type
```



## Week 2 - Types:      Classes and Scoped Enumerations

### Reminder from OOP244
Encapsulation: [Classes and Resources](https://intro2oop.sdds.ca/C-Encapsulation/classes-and-resources)

#### Copy Constructor - Declaration
```cpp
// Syntax
Type(const Type&);

// Example
Student(const Student&);
```

#### Copy Constructor - Definition
##### Long version - Outdated
**Steps**
 1. Perform a shallow copy on all non-resource instance variables
 2. Allocate memory for each new resource
 3. Copy data from the source resource to the newly created resource

**Example**
```cpp
Student::Student(const Student& src) {
    // shallow copy
    no = src.no;
    ng = src.ng;

    // allocate dynamic memory for grades
    if (src.grade != nullptr) {
        grade = new float[ng];
        // copy data from the source resource
        // to the newly allocated resource
        for (int i = 0; i < ng; i++)
            grade[i] = src.grade[i];
    }
    else {
        grade = nullptr;
    }
}
```

##### Short Version - Uses Copy Assignment Operator
```cpp
Student::Student(const Student& src) {
    grade = nullptr;  // Set pointers to null if not initialized to null, otherwise omit this line
    *this = src;      // Use copy assignment operator to copy resources
}
```

#### Copy Assignment Operator
Client code use case
```cpp
identifier = identifier
```

#### Copy Assignment Operator - Declaration
```cpp
// Syntax
Type& operator=(const Type&);

// Example
Student& operator=(const Student&);
```

#### Copy Assignment Operator - Definition

**Steps**
 1. Check for self-assignment
 2. Deallocate any previously allocated memory for the resource associated with the current object
 3. Shallow copy the non-resource instance variables to destination variables
 4. Allocate a new memory for the resource associated with the current object
 5. Copy resource data from the source object to the newly allocated memory of the current object

**Example**
```cpp
Student& Student::operator=(const Student& source)
{
    // 1. check for self-assignment (and NOTHING else)
    if (this != &source)
    {
        // 2. clean up (deallocate previously allocated dynamic memory)
        delete[] grade;

        // 3. shallow copy (copy non-resource variables)
        no = source.no;
        ng = source.ng;

        // 4. deep copy (copy the resource)
        if (source.grade != nullptr) {
            // 4.1 allocate new dynamic memory, if needed
            grade = new float[ng];
            // 4.2 copy the resource data
            for (int i = 0; i < ng; i++)
                grade[i] = source.grade[i];
        }
        else {
            grade = nullptr;
        }
    }
    return *this;
}
```





#### Move-Constructor 
```cpp
// Declaration Syntax
class-name(class-name&&);

// Declaration Example
Array(Array&& src)

// Definition Example
Array(Array&& src) { 
    *this = std::move(src); 
}
```

#### Move-Assignment Operator
```cpp
// Declaration Syntax
class-name& operator=(class-name&&);

// Declaration Example
Array& operator=(Array&& src)
```

* Steps for Move Assignment Operator
  1. Check for self-assignment
  2. Clean up the resource used by the current instance
  3. Shallow copy
  4. Move the resource from parameter into current instance
     1. Copy address to current object
     2. Set parameter to nullptr
        * The parameter doesn't have the resource anymore

```cpp
class Array
{
    int* a = nullptr;
    unsigned n = 0u;
    int dummy = 0;
public:
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
}
```



## Week 3 - Class Relationships:    Class Templates

### Function Template

Syntax - Basic
```cpp
template<Type identifier[, ...]>
```
Where:
 * `Type` can be 
   * `typename` or `class`
   * `int`, `long`, `short`, `char`
 * `identifier` is whatever placeholder for argument

Syntax - Alternatives
```cpp
template <typename T>                   // OR 
// template <class T>

// ... template body follows here
    T value; // value is of type T
```

Example
```cpp
// Template for swap
// swap.h

template<typename T>
void swap(T& a, T& b) {
    T c;
    c = a;
    a = b;
    b = c;
}
```

```cpp
template < template-parameter-list > // template header
return-type function-name( ... )
{
    // template body for a family of functions
    // ...
}

template < template-parameter-list > // template header
class-key Class-name
{
    // template body for a family of classes
    // ...
};

template < template-parameter-list > // template header
type variable_name; // template body for a family of variables
```

### Class Template

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


