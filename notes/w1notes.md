# OOP345 Notes

## Week 1 Notes

```cpp
// a-sep11
#include <iostream>
using namesapce std;
auto main() -> int {
    cout << "Welcome to oop345" << endl;
    cout << "App dev using C++" << endl;
    return 0;
}
```

### Review of OOP244

**Core Concepts of OOP244**
 * Encapsulation    - putting the data and behaviour together
 * Polymorphism     - doing the same thing in different ways  
 * Inheritance      - reusing the design to build new classes

* Encapsulation couples data with logic
  * Weak encapsulation data is accessible

 * Method       - is a member function; can be private or public
 * Attribute    - is a member variables

Example of Inheritance
```cpp
// inheritance.cpp
#include <iostream>
using namespace std;

class Animal {
public:
    void speak() const { cout << "Some sound\n"; }
};

class Dog : public Animal {
public:
    void speak() const { cout << "Woof!\n"; }
};

int main() {
    Dog d;
    d.speak();      // Woof!
}
```







@@@



```cpp
// scope_example.cpp
#include <iostream>
using namespace std;

int g = 10;     // global scope, external linkage

int main() {
    int x = 5;  // local scope
    cout << g + x << endl;
}
```

@@@


shadowed_scope.cpp great for walkthroughs, terrible for debugging

```cpp

```


