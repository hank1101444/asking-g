```cpp
#include <iostream>
using namespace std;

int main() {
    int* tt = new int[5]();
    for (int i = 0; i < 5;++i) {
        tt[i] += i + 1;
    }
    int tt2[10] = {};
    int** test = &tt;
    for (int i = 0; i < 5; ++i) {
        cout << *test[i];
    }
    return 0;
}
```
