# OOP244:   General Syntax


---


## Week 2 - Foundations:    Dynamic Memory

Foundations: [Dynamic Memory](https://intro2oop.sdds.ca/B-Foundations/dynamic-memory)

### Single Instances: Dynamic Allocation

```cpp
// Syntax
pointer = new Type;

// Example
Student* harry = nullptr;   // a pointer in static memory
harry = new Student;        // points to a Student in dynamic memory
```

### Single Instances: Dynamic Deallocation

```cpp
// Syntax
delete pointer;
pointer = nullptr;

// Example
delete harry;
harry = nullptr;  // good programming style
```

### Multiple Instances (Arrays) - Dynamic Allocation

Syntax
```cpp
pointer = new Type[size];
```

Example
```cpp
int n = 5;                  // the number of students
Student* student = nullptr; // the address of the dynamic array

student = new Student[n];   // allocates dynamic memory
```

### Multiple Instances (Arrays) - Dynamic Deallocation

Syntax
```cpp
delete [] pointer;
pointer = nullptr;
```


---

## Week 8 - Encapsulation:    Classes and Resources

Encapsulation: [Classes and Resources](https://intro2oop.sdds.ca/C-Encapsulation/classes-and-resources)

### Copy Constructor - Declaration

```cpp
// Syntax
Type(const Type&);

// Example
Student(const Student&);
```

### Copy Constructor - Definition

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

### Copy Assignment Operator

Client code use case
```cpp
identifier = identifier
```

### Copy Assignment Operator - Declaration

```cpp
// Syntax
Type& operator=(const Type&);

// Example
Student& operator=(const Student&);
```

### Copy Assignment Operator - Definition

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

### Localization - Private Member Function

**Syntax: Declaration**
```cpp
void init(const Type& source)
```

**Syntax: Definition**
```cpp
// Private Member Function
void Type::init(const Type& source) {
    // shallow copy logic
    // deep copy logic
}

// Copy Constructor
Type::Type(const Type& source) {
    // call private member function
    init(source);
}

// Copy Assignment Operator
Type& Type::operator=(const Type& source) {
    // check for self-assignment
    // clean up
    // call private member function
    // return object
}
```

Example
```cpp
void Student::init(const Student& source) {
    no = source.no;
    ng = source.ng;
    if (source.grade != nullptr) {
        grade = new float[ng];
        for (int i = 0; i < ng; i++)
            grade[i] = source.grade[i];
    }
    else {
        grade = nullptr;
    }
}

Student::Student(const Student& source) {
    init(source);
}

Student& Student::operator=(const Student& source) {
    if (this != &source) {
        delete [] grade;
        init(source);
    }
    return *this;
}
```

### Localization - Direct Call

### Localization - Assigning Temporary Objects

### Copies Prohibited

Syntax
```cpp
    Student(const Student& source) = delete;
    Student& operator=(const Student& source) = delete;
```


---


## Week 9 - Inheritance:    Derived Classes

Inheritance: [Derived Classes](https://intro2oop.sdds.ca/D-Inheritance/derived-classes)

### Derived Class Definition

Syntax
```cpp
class Derived : access Base { };
```

### Access

Syntax
```cpp
class Base {
private:            // Only for the base
protected:          // Only for the family (derived classes)
public:             // For everyone
}

class Derived : public Base {
public:
};
```


---


## Week 9 - Inheritance:    Functions in a Hierarchy

Inheritance: [Functions in a Hierarchy](https://intro2oop.sdds.ca/D-Inheritance/functions-in-a-hierarchy)


### Shadowing Solution

Syntax
```cpp
Base::identifier(arguments)
```

Example
```cpp
// Student.cpp

void Person::set(const char* n) {
    strncpy(name, n, NC);
    name[NC] = '\0';
}

Student::Student() {
    no = 0;
    ng = 0;
}
```

### Shadowing – Exposing an Overloaded Member Function

```cpp
// Syntax - Declaration
using Base::identifier;

// Example
    using Person::display;
```


### Constructors - Passing Arguments to a Base Class Constructor

A call to the base class constructor from a derived class constructor that forwards values takes the form:

Syntax
```cpp
Derived( parameters ) : Base( arguments )
```

Example
```cpp
Student::Student(const char* nm, int sn, const float* g, int ng_) : Person(nm) {
    bool valid = sn > 0 && g != nullptr && ng_ >= 0;
    if (valid)
        for (int i = 0; i < ng_ && valid; i++)
            valid = g[i] >= 0.0f && g[i] <= 100.0f;

    if (valid) {
        no = sn;
        ng = ng_ < NG ? ng_ : NG;
        for (int i = 0; i < ng; i++)
            grade[i] = g[i];
    } else {
        *this = Student();
    }
}
```

### Constructors - Inheriting Base Class Constructors (Optional)

The declaration for inheriting a base class constructor takes the form:

```cpp
// Syntax
using Base::Base;


// Example
class Instructor : public Person {
public:
    using Person::Person;
};
```

### Destructors

### Helper Operators (Optional)


---


## Week 10 - Polymorphism:  Virtual Functions

Polymorphism: [Virtual Functions](https://intro2oop.sdds.ca/E-Polymorphism/virtual-functions)

Syntax
```cpp
class Base {
public:
    virtual type functionName();
}
```


---


## Week 10 - Polymorphism:  Abstract Base Classes

Polymorphism:  [Abstract Base Classes](https://intro2oop.sdds.ca/E-Polymorphism/abstract-base-classes)

### Pure Virtual Function

```cpp
// Syntax: Declaration
virtual Type identifier(parameters) = 0;


// Example
virtual void display(std::ostream&) const = 0;
```


---


## Week 11 - Refinements:   Derived Class with a Resource

Refinements:   [Derived Class with a Resource](https://intro2oop.sdds.ca/F-Refinements/derived-classes-and-resources)

### Constructor and Destructors

Syntax
```cpp
Derived::Derived(parameters) : Base(arguments) {
    // ..
}

Derived::~Derived() {
    // .. (same)
}
```

Syntax - Comparison
```cpp
// Constructor
// Default - calls base class no-argument constructor
Derived::Derived(parameters) {
    // constructor body
}

// Explicit - calls specific base class constructor
Derived::Derived(parameters) : Base(arguments) {
    // constructor body
}

// Destructor
Derived::~Derived() {
    // cleanup derived class resources
}
// Base class destructor is called automatically after
```

Example
```cpp
// Constructor
// Calls Person(const char*)
Student::Student(const char* nm, int sn, const float* g, int ng_) : Person(nm) {
    // Student initialization
}

// Destructor
Student::~Student() {
    delete[] grade;  // Clean up derived class resource
}
// ~Person() is called automatically
```


### Copy Constructor

Syntax
```cpp
// Declaration (same as usual)
Derived(const Derived&);


// Definiton
Derived(const Derived& identifier) : Base(identifier) {
    // ...
}
```

* Steps - Definition:
  1. Copy the base class part of the existing object
    1. Allocate memory for the instance variables of the base class in the order of their declaration
    2. Execute the base class' copy constructor
  2. Copy the derived class part of the existing object
    1. Allocate memory for the instance variables of the derived class in the order of their declaration
    2. Execute the derived class' copy constructor


Example
```cpp
Student::Student(const Student& src) : Person(src) {
    no = src.no;
    ng = src.ng;
    if (src.grade != nullptr && ng > 0) {
        grade = new float[ng];
        for (int i = 0; i < ng; i++)
            grade[i] = src.grade[i];
    }
    else
        grade = nullptr;
}
```


### Copy Assignment Operator

Syntax
```cpp
// Functional Expression Form
Base::operator=(identifier);

// Cast Assignment Form
// Version 1: Cast in one line
(Base&)*this = identifier;

// Version 2: Create reference variable first
Base& base = *this;
base = identifier;
```

#### Using a Private Member Function

* Setup:
  * Private member function created
  * Derived copy constructor calls Base copy constructor

Example: Setup pt. 1 - Header
```cpp
class Student : public Person {
    void init(int, int, const float*);   // ← NEW: Private member function 
public:
    Student& operator=(const Student&);  // ← NEW: Assignment operator added
};
```

Example: Setup pt. 2 - Source code
```cpp
// Private member function
void Student::init(int no_, int ng_, const float* g) {
    no = no_;
    ng = ng_;
    if (g != nullptr && ng > 0) {
        grade = new float[ng_];
        for (int i = 0; i < ng; i++)
            grade[i] = g[i];
    } else {
        grade = nullptr;
    }
}

// Copy Constructor
Student::Student(const Student& src) : Person(src) {    // Calls base copy constructor
    init(src.no, src.ng, src.grade);                    // Calls private helper
}
```

##### Copy Assignment Operator

###### Option 1:   Functional Form

Example
```cpp
Student& Student::operator=(const Student& src) {
    if (this != &src) {
        // Base class assignment
        Person::operator=(src);             // 1 - Functional Expression
        delete [] grade;
        init(src.no, src.ng, src.grade);    // Calls private helper
    }
    return *this;
}
```

###### Option 2:   Cast Assignment Form - Version 1

Example
```cpp
Student& Student::operator=(const Student& src) {
    if (this != &src) {
        // Base class assignment            // 2 - Assignment Expression
        (Person&)*this = src;               // Call base class assignment operator
        delete [] grade;
        init(src.no, src.ng, src.grade);    // Calls private helper
    }
    return *this;
}
```

###### Option 2:   Cast Assignment Form - Version 2

Example
```cpp
Student& Student::operator=(const Student& src) {
    if (this != &src) {
        // Base class assignment        // 2 - Assignment Expression
        Person& person = *this;         // Only copies address
        person = src;                   // Calls base assignment operator
        delete [] grade;
        init(src.no, src.ng, src.grade);    // Calls private helper
    }
    return *this;
}
```


#### Direct Call Copy Constructor

* Setup: 
  * Derived copy constructor calls no-argument base constructor

Example:
```cpp
// Copy constructor
Student::Student(const Student& src) {  // Calls no-argument base constructor NOT base copy constructor
    grade = nullptr;
    *this = src;                        // Calls assignment operator directly
}

// Copy Assignment Operator
Student& Student::operator=(const Student& src) {
    if (this != &src) {
        // Base class assignment
        // 1 - Functional Expression
        Person::operator=(src);         // OR
        // 2 - Assignment Expression
        // (Person&)*this = src;        // Call base class assignment operator
        delete [] grade;
        no = src.no;
        ng = src.ng;
        if (src.ng > 0) {
            grade = new float[ng];
            for (int i = 0; i < ng; i++)
                grade[i] = src.grade[i];
        }
        else
            grade = nullptr;
    }
    return *this;
}
```


---


## Week 12 - Polymorphism:  Templates

Polymorphism:  [Templates](https://intro2oop.sdds.ca/E-Polymorphism/templates)

### Function Template

Syntax - Basic
```cpp
template<Type identifier[, ...]>
```

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



