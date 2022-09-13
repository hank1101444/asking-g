```cpp
#include <iostream>
using namespace std;
#include <cmath>

int main() {
    // 278.78
    // double to int will get wrong answer
    double d;
    int a;
    // d = 278.78
    // d = 278.127
    //cin >> d;
    a = d * 100;
    cout << a << endl;      //a will be like 27877

    cout << d * 100;        //d will be like 27888
	return 0;
}
```
