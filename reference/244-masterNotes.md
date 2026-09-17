# OOP244 - MASTER NOTES


---

## Week 4: Encapsulation

### [Construction and Destruction](https://intro2oop.sdds.ca/C-Encapsulation/construction-and-destruction)

#### Constructor - Understanding Order

 * Construction
   * Compiler assembles object in the following order:
     1. Allocates memory for each instance variable in the order listed in the class definition
     2. Executes the logic, if any, within the constructor's definition
 * Member Function Calls
   * Normal member function is called after constructor
 * Multiple Objects
   * Multiple objects defined in a single declaration are created in order specified by the declaration


#### Destructor - Understanding Order

 * Destruction
   * Object destruction proceeds in the following order:
     1. Execute the logic of the object's destructor
     2. Deallocate memory for each instance variable in opposite order to that listed in the class definition
 * Member Function Calls
   * Destructor starts executing only after every normal member function has completed its execution.
 * Multiple Objects
   * Compiler destroys sets of objects in opposite order to that of their creation.

---

## Week 5

### Fardad Lecture Notes
[Lecture link](https://www.youtube.com/watch?v=HiDHcPTKtIs)

 * NEVER USE `using` INSIDE A HEADER FILE. MANUALLY ADD IN THE STD::. IT IS HIDDEN LOGIC.
   * [Source](https://youtu.be/HiDHcPTKtIs?si=AZMNpnHNO9CnsElP&t=2230)

---


## Week 8

### Fardad Lecture Notes
[Lecture Link](https://www.youtube.com/watch?v=rpLY8j8QR4I)
[Topic: Classes and Resources](https://intro2oop.sdds.ca/C-Encapsulation/classes-and-resources)

 * Example: Hotel room and resizing
   * If you want to book a hotel room, and the current one is too small, you book another one and move there
   * You would not ask the hotel to break down the walls, and increase the size of the original room
   * This is the same for resizing data resources
   * If you want to change a resource to have 1mill bytes to 1mill + 1 bytes, you would need 2mill + 1 bytes of storage.

#### Safe Empty State

 * Initialize things as empty right off the bat by using `{}`

Example:
```cpp
class String {
    char* m_data{};         // This becomes null
    size_t m_size{};        // This becomes 0
};
```
https://youtu.be/rpLY8j8QR4I?si=8gzTYUaZByielwyw&t=1211


```cpp
void String::setEmpty() {
    delete[] m_data;
    m_size = 0;
    m_data = new char[1] {};            // This means that the string has 1 character which is null
}
```
https://youtu.be/rpLY8j8QR4I?si=Q1WzsSrHoJiM5c1y&t=1513

#### 

**Copy Constructor**
One argument constructor, where the argument is an object of the same class

**Copy Assignment Operator**




### Reading Notes




@@@ Continuation from spill


### Rule of 3

For an object that has data that is outside of its scope, the Rule of 3 must be implemented


---


## Week 9 - Inheritance

**Topics:** 
 * Inheritance
   * Derived Classes
   * Functions in a Hierarchy

### [Fardad Lecture Notes - Nov 07 cont.](https://www.youtube.com/watch?v=6AVqIv1tCT8)

Review of Classes and Resources

Prototypes
 * Forward declaration is a prototype for a class
 * `extern` is a prototype for a variable
   * For a variable to become accessible in other files, you need to use extern as a prototype of the variable

Utils.h
```cpp
class Utils {
    // Utility functions
};
extern Utils ut;        // Externs
```

Utils.cpp
```cpp
#include "Utils.h"
namespace name {
    Utils ut;           // Instantiates Utils class to ut
}
```

### [Fardad Lecture Notes - Nov 11](https://www.youtube.com/watch?v=bONAxNaIX44)

#### Derived Classes

Syntax
```cpp
class Derived : access Base {

    // ...

};
```

Example
```cpp
class Base {
private:
    // Only for the base
protected:
    // Only for the family (derived classes)
public:
    // For everyone
    Baser() {
        // ..
    }
}

class Derived : public Base {
public:
    // ..
};
```


##### Member Initialization List

**YOU CANNOT CALL A CONSTRUCTOR**

 * If you are constructing a derived class, and you want to customize the base, you must have the constructor in the initialization area at least.

> **Personal Research from MS2**:
> A member initializer list in C++ is a syntax used within a constructor definition to initialize a class's non-static data members and base classes. 
> It appears after the constructor's parameter list, separated by a colon, and consists of a comma-separated list of initializers for each member or base class.


Example:
```cpp
Cat::Cat(const char* name, int numOfLives)
   : Animal(name), m_numOfLives{9} 
{
    // Animal(name);            // DO NOT DO THIS
}
```

 * Base is responsible for validation
 * Initialization vs. Setting
   * Initialization will have the value upon creation
   * Setting will have garbage values, then will be set.
 * Overloading vs. Overriding
   * Overload - signature is different
   * Override - signature is identical


* The reference type used will determine whether an object calls base class or derived class functions

Example:
```cpp
Animal A("Coco");
Cat B("Fluffy");
Animal& AB = B;

AB.act();               // Forgets being a cat
```


---


## Week 10 - Polymorphism

Topics:
 * Polymorphism
   * Virtual Functions
   * Abstract Base Classes


### [Fardad Lecture Notes - Nov 11 cont.](https://youtu.be/bONAxNaIX44?si=HLJWowvvrYfCVCGW&t=2360)

> The madman was able to finish Week 9 and 10 in a single lecture period... sugoi...


How can you guarantee that no matter how you point to an object, the most recent method is called? INTRODUCING....

#### Virtual Functions BABY

 * DON'T MAKE POINTERS TO A BASE CLASS WITHOUT VIRTUAL FUNCTIONS ITS GONNA CREATE MEMORY LEAK
   * The destructor is just gonna delete the base class and not the derived class


 * When you make a destructor virtual, you guarantee that no matter how the object is pointed at the moment of destructor, all destructors are called


* When creating base classes, look at methods. Will the method ever need to be improved in future generations of classes?
* Virtuality will continue on from the base class it came from
  * A grandparents virtual functions will carry on to parent and child
  * A parents virtual functions will carry on to child
  * TRANSITIVE
* Some methods should always stay the same. Those ones won't be virtual.


### [Fardad Lecture Notes - Nov 11 cont.](https://youtu.be/bONAxNaIX44?si=YNeTvEfk5wVuDGdg&t=3398)

#### Abstract Base Classes

**Abstract Base Classes** - 
Any base class that is capable of doing something, but you don't know WHAT it does
 * It's a design, a requirement
 * Contains a series of methods that are hollow
   * You want the methods to be there but not say how they work
 * This class becomes a design
   * It cannot be instantiated until it is inherrited into another class that implements all those hollow methods
   * Otherwise they remain abstract



##### Pure Interface vs. Abstract Base Class

Pure Interface:
 * Contains/inherits only pure virtual function(s)
   * No implementations
 * Cannot be instantiated as a class
 * No data members


Abstract Base Class:
 * Same as above, but has at least one data member
 * Still has no implementations


---


## Week 11 - Refinements: Derived Classes and Resources


### [Fardad Lecture Notes - Mar 20](https://youtu.be/nZQ5lUsYAII?si=9Mf82k6Jjb1r5M57&t=1940)

**Note:**
 * Assignment at the moment of creation is a call to a one argument constructor

Example:
```cpp
// All of these are the same thing
Class object = "name";
Class object("name");
Class object {"name"};
```

#### Copy vs. Assignment

Example:
```cpp
Cat cat1 = "Fluffy";
Cat cat2 = cat1;            // This is copy constructor
Cat cat3 = "Coco";
cat3 = cat1;                // This is copy assignment operator
```

Remember:
 * Copy constructor is for creation of a new object
 * Copy assignment operator is for already existing objects;
   * Data already exists, it needs to be deleted, then assigned new data


#### Copy Assignment Operator in the Rule of 3

Copy Assignment Operator does not need to be implemented as part of the Rule of 3 when:
 * A base class has properly implemented the Rule of 3
 * A derived class may or may not add static resources
   * As long as no resources are dynamically allocated
 * The derived class is being copied to another object
* This scenario implies that the base class Copy Assignment Operator will be used, therefore a new one does not need to be created.



#### Constructors in Inheritance:

Notes:
 * Derived class objects automatically call base class no-argument constructors
   * To call a different base class constructor, use the *member initializer list*

Syntax Comparison + Example
```cpp
// Default - calls base class no-argument constructor
Derived::Derived(parameters) {
    // constructor body
}

// Explicit - calls specific base class constructor
Derived::Derived(parameters) : Base(arguments) {
    // constructor body
}

// Example:
// Calls Person(const char*)
Student::Student(const char* nm, int sn, const float* g, int ng_) : Person(nm) {
    // Student initialization
}
```


#### Destructors in Inheritance:

Notes:
 * Automatically call base class destructors

Syntax + Example
```cpp
Derived::~Derived() {
    // cleanup derived class resources
}
// Base class destructor is called automatically after


// Example
Student::~Student() {
    delete[] grade;  // Clean up derived class resource
}
// ~Person() is called automatically
```


#### Copy Constructor

Notes:
 * When a derived class wants to call a base class constructor, by default it calls its no-argument constructor
   * This leaves base class resources either uninitialized or in a default state
 * To override, the derived class copy constructor needs to explicitly call 

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


#### Copy Assignment Operator

* Notes:
  * Default copy assignment operator for derived class WILL call base class copy assignment operator
    * Custom copy assignment operator for derived class WILL NOT.
  * For custom, we must explicitly call base class copy assignment operator
    * To be done in derived class copy assignment operator definition
  * 2 ways:
    * Functional Form
    * Cast Assignment Form (2 versions)

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

##### Using a Private Member Function

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


##### Direct Call Copy Constructor

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

### [Fardad Lecture Notes - Mar 25](https://www.youtube.com/watch?v=g_tOvmbAk3E)

#### Function Template

Syntax
```cpp
template<Type identifier[, ...]>
```

Where:
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
    * 


### [Reading Notes - Templates](https://intro2oop.sdds.ca/E-Polymorphism/templates)

#### Class Template

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


### [Fardad Lecture Notes - Mar 22 '24](https://www.youtube.com/watch?v=AA5DMK45Ylw)


#### Function Templates for Special Cases (OOP345)

Overloading vs. Specialization

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

### [Reading Notes - Templates](https://intro2oop.sdds.ca/E-Polymorphism/templates)

#### Constrained Casts

[Reading Link](https://intro2oop.sdds.ca/E-Polymorphism/templates#constrained-casts)

Constrained Casts:
* Improve type safety
* 4 Types:
  * `static_cast<Type>(expression)`
  * `reinterpret_cast<Type>(expression)`
  * `const_cast<Type>(expression)`
  * `dynamic_cast<Type>(expression)`


##### static_cast<Type>(expression)
   * Good for related types
     * e.g. `int` to `double`
   * Most common


##### reinterpret_cast<Type>(expression)
   * Unrelated types
     * e.g. `int` to pointer
   * Don't use this it's kinda whack


##### const_cast<Type>(expression)
   * Removes const when passing const variable to function that doesn't accept const
   * Use only when you're sure function doesn't modify value


##### dynamic_cast<Type>(expression)
   * For polymorphism
   * Use when casting within an inheritance hierarchy and unsure if cast is valid
   * Returns `nullptr` when cast fails, so you can check if successful
   * STELLA DEFINITION:
     * This is like a boolean checker to see which class an object comes from
     * If it's coming from the class we want, it'll return a pointer to that type
     * If it doesn't we get a nullptr.
   * Overlord's version of my definition:
     * "Try to treat this object as type X - if it works, give me a pointer to use it as X, if not, give me nullptr"

Syntax + Example
```cpp
// Syntax-ish
Derived* d = dynamic_cast<Derived*>(b);
if (d != nullptr) {
    // It IS a Derived - AND you now have a pointer to use it as one
    d->derivedOnlyFunction();
} else {
    // It is NOT a Derived
}

// Example
Animal* animals[3];
animals[0] = new Cat;
animals[1] = new Dog;
animals[2] = new Cat;

for (int i = 0; i < 3; i++) {
    Cat* c = dynamic_cast<Cat*>(animals[i]);
    if (c != nullptr) {
        c->purr();  // Only cats can purr!
    }
}
```


## Week 13 - Polymorphism:  Overview of Polymorphism

### [Fardad Lecture Notes - Apr 2](https://www.youtube.com/watch?v=POWrWANJLwc)


#### Categories of Polymorphism
* Ad-Hoc Polymorphism   -   Fake polymorphism
  * Coercion
  * Overloading
* Universal Polymorphism    -   Real polymorphism
  * Inclusion
  * Parametric


#### Ad-Hoc Polymorphism

 * Fake polymorphism
   * When you look at it closer, it's not REALLY polymorphism
   * Just LOOKS like polymorphism

#### Ad-Hoc Polymorphism - Coercion

* Coercion
  * Example:
    int + double
    upcasts integer to double, does addition, returns double
  * Essentially casting
    * Upcasts to a different type
  * Two variations:
    * Narrow argument type (narrowing coercion)
    * Widen argument type (promotion)

Coercion Example:
```cpp
void display(int a) {
    cout << "(" << a << ')' << endl;
}

int main( ) {
    display(10);        // outputs (10)
    display(12.6);      // narrowing    ->  outputs (12)
    display('A');       // promotion    ->  outputs (65)
}
```

#### Ad-Hoc Polymorphism - Overloading

* Overloading
  * It's not really doing the SAME THING different ways
  * Overloading works by having different function signatures
    * Only the function name is the same, but the parameter types are still different
    * Therefore it's essentially two different names

#### Universal Polymorphism

* This is the REAL DEAL
* TRUE POLYMORPHISM
* Everything is identical

#### Universal Polymorphism - Inclusion

Inclusion (Virtuality)
* Anything done through *virtuality* is **Inclusion Polymorphism**
  * When you have a print in a base class that's virtual
    * Then you have a print in the derived class that's virtual
    * When you create a pointer to a base class of a derived class, and you say print
      * It looks like a print from the base class being called
      * But automatically, it switches to the latest version
        * Which would be the derived class
      * Therefore print is done in different ways with identical signatures
    * HENCE, ~POLYMORPHIC~


#### Universal Polymorphism - Parametric

Parametric (Templates):
 * The ULTIMATE FORM of polymorphism
   * You don't have a function, you don't have a class
     * You just tell the compiler how to create it for you based on needs
     * These are called **TEMPLATES**
   * Pure polymorphism.


 