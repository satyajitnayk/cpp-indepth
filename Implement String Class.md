```c++
#include <iostream>
#include <string>
using namespace std;

class String {
private:
  char *res;
  unsigned int len;

public:
  String() {
    res = new char[1]; // Allocate memory for an empty string
    res[0] = '\0';     // Null-terminate the string
    len = 0;
  }

  String(const char *chars) {
    len = strlen(chars);
    // C-Style Strings: The +1 ensures there’s enough space for the null
    // terminator (\0), which is essential for strings in C-style.
    res = new char[len + 1];
    strcpy(res, chars);
  }

  String(const String &str) {
    len = str.len;
    res = new char[len + 1];
    strcpy(res, str.res);
  }

  String &operator=(const String &str) {
    // check if assiging same str to str (avoid self assignment)
    // `this` is a pointer that points to the current instance of the class.
    if (this != &str) {
      // storing in temp pointer in case memory allocation fails
      char *temp = this->res;
      // also current pointer alredy holding some resource
      // we need to clean it before re-assigning new resource to it
      len = str.len;
      res = new char[len + 1];
      strcpy(res, str.res);
      delete[] temp;
    }
    // Dereferencing the this pointer (*this) gives a reference to the
    // current instance of the class, not a pointer.
    return *this;
  }

  unsigned int length() { return len; }

  // To make the << operator work seamlessly with the String class, you need to
  // declare it as a friend function. This allows the operator function to
  // access private or protected members of the class, like res.
  friend ostream &operator<<(ostream &out, const String &str);
  friend istream &operator>>(istream &out, const String &str);

  // destructor to freeup resources
  ~String() {
    if(res) {
      delete[] res;
      res = nullptr;
      len = 0;
    }
  }
};

ostream &operator<<(ostream &out, const String &str) {
  out << str.res;
  return out; // we can do chaining like cout << str1 << str2 << "\n";
}

istream &operator>>(istream &in, const String &str) {
  in >> str.res;
  return in; // we can do chaining like cin >> str1>> str2;
}

int main() {

  String str1;             // default constructor
  String str2 = "hello";   // parameterized constructor
  String str3 = str1;      // copy constructor
  String str4(str1);       // copy constructor
  str3 = str1;             // copy assignment operator
  str4 = "hello";          // copy assignment operator
  int len = str3.length(); //
  cout << str2 << "\n";
  cin >> str1;          // overload >> operator
  cout << str1 << "\n"; // overload << operator

  return 0;
}
```
