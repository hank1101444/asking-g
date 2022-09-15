```cpp
#include <iostream>
using namespace std;
int main() {
	long long int tt = 15;
	char* first = &reinterpret_cast<char&>(tt); // 00001111
	int a;
	cout << *first;
	return 0;
}
```
