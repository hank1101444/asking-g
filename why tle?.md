```cpp
#include <iostream>
using namespace std;

int num;
//int ans( int a) {
//	num++;
//	if (a == 1)
//		return num;
//	else {
//		if (a % 2 == 1)
//			ans(3 * a + 1);
//		else
//			ans(a /= 2);
//	}
//}


int main() {

	 int a, b;
	while (cin >> a >> b) {
		int max_ = 0;
		cout << a << ' ' << b << ' ';
		if (a > b) {
			int tmp = a;
			a = b;
			b = tmp;
		}

		for (int i = a; i <= b; i++) {
			num = 1;
			//int tmp = ans(i);
			int tmp = i;
			while (1) {
				if (tmp == 1)
					break;
				if (tmp % 2 == 1) {
					tmp = 3 * tmp + 1;
				}
				else tmp /= 2;
				num++;
			}

			if (num > max_)
				max_ = num;
		}

		cout << max_ << endl;
	}
	return 0;
}
```
