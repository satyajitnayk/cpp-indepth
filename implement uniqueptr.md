```c++
#include <iostream>
using namespace std;

template <typename T> class uniqueptr {
private:
  T *res;

public:
  uniqueptr(T *a = nullptr) : res(a) { cout << "constructor\n"; }
  // compiler will throw error if try to use it
  uniqueptr(const uniqueptr<T> &ptr) = delete;
  uniqueptr &operator=(const uniqueptr<T> &ptr) = delete;

  // move copy operator
  uniqueptr(uniqueptr<T> &&ptr) { // `&&` for RVALUE
    res = ptr.res;
    ptr.res = nullptr;
  }

  // move assignment constructor
  uniqueptr &operator=(uniqueptr<T> &&ptr) { // `&&` for RVALUE
    if (this != &ptr) {                      // avoid case ptrx = ptrx
      if (res) {
        delete res;
      }
      res = ptr.res;
      ptr.res = nullptr;
    }
    return *this;
  }

  T *operator->() { return res; }

  T &operator*() { return *res; }

  T *get() { return res; }

  void reset(T *newres = nullptr) {
    if (res) {
      delete res;
    }
    res = newres;
  }

  ~uniqueptr() {
    if (res) {
      delete res;
      res = nullptr;
    }
  }
};

int main() {
  uniqueptr<int> ptr1(new int(2));
  // uniqueptr<int> ptr2(ptr1);  // compilation error
  // uniqueptr<int> ptr3 = ptr1; // compilation error (copy constructor)
  uniqueptr<int> ptr4(new int(500));
  // ptr4 = ptr3; // compilation error (copy assignment operator)

  uniqueptr<int> ptr5 = move(ptr1); // ownership moved from ptr1 to ptr5
  // ptr4 = move(ptr1);

  // ptr1->func(); // operator overload
  cout << *ptr4;
  ptr4.get();
  ptr4.reset(new int(30)); //
 
  // destructor: to freeup the resources

  return 0;
}
```
