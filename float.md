```cpp
#include <iostream>
using namespace std;
#include <cmath>

int main() {
	double mx, mi;
	mx = 72.74050000;
	mi = 72.74050000;
	double tp = mx + mi;
	printf("mx = %.16f\n", mx);
	// I want to round to the third decimal place
	//mx *= 10000;
	//int tp = fmod(mx, 10);
	//cout << tp;
	//if (tp >= 5) 
	//	mx += 10 - tp;
	//mx /= 10000;
	//printf("mx = %.3f\n", mx);

	return 0;
}
```
