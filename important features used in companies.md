> Features supported from c++17 onwards

### structured bindings

```c++
#include <iostream>
using namespace std;

struct Person {
  int age;
  string address;
  long long mobileno;
};

int main() {
  Person p{34, "bangalore", 123456789};
  auto [age, addr, mob] = p;
  cout << age << " " << addr << " " << mob << "\n";
  return 0;
}
```

### Future and Async

```c++
#include <future>
#include <iostream>
using namespace std;

int sum(int a, int b) {
  cout << "adding two numbers\n";
  this_thread::sleep_for(chrono::milliseconds(2000));
  return a + b;
}
int main() {
  future<int> ft = async(sum, 8, 9);
  cout << ft.get() << "\n";
  return 0;
}
```

### File systems(portable on all OS)

```c++
#include <filesystem>
#include <iostream>
using namespace std;
namespace fs = filesystem;

int main() {
  fs::create_directory("docs");
  fs::copy_file("src.txt", "docs/dst.txt",
                fs::copy_options::overwrite_existing);

  // iterate through directories
  for (auto &dir : fs::directory_iterator("docs")) {
    cout << dir << " ";
  }
  cout << "\n";

  fs::remove_all("docs");
  return 0;
}
```
