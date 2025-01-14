### **Virtual Constructor**

C++ **does not support virtual constructors** directly because constructors are responsible for creating objects, and virtual functions rely on the object being constructed. Since the object is not fully initialized during construction, the concept of a "virtual constructor" is not feasible in C++.

### **Virtual Destructor**

A **virtual destructor** ensures proper cleanup of derived class objects when deleted through a base class pointer. Without a virtual destructor, only the base class destructor would be called, leading to resource leaks.

#### **Example: Virtual Destructor**

```cpp
#include <iostream>
using namespace std;

class Base {
public:
    Base() { cout << "Base Constructor\n"; }
    virtual ~Base() { cout << "Base Destructor\n"; } // Virtual destructor
};

class Derived : public Base {
public:
    Derived() { cout << "Derived Constructor\n"; }
    ~Derived() { cout << "Derived Destructor\n"; }
};

int main() {
    Base* obj = new Derived();
    delete obj; // Properly calls Derived's destructor, then Base's destructor
    return 0;
}
```

### **Output**:

```
Base Constructor
Derived Constructor
Derived Destructor
Base Destructor
```

### **Explanation**:

- When `delete obj` is called, the `Derived` destructor is called first, followed by the `Base` destructor.
- Without the virtual destructor in the `Base` class, only the `Base` destructor would be called, causing a leak of resources allocated in `Derived`.
