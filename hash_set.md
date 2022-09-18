 ```cpp
 #include <iostream>
using namespace std;
#include <set>
#include <unordered_set>
#include <string>
#include <map>
int main() {
	map <int, int> m;
	unordered_set <int> s;
	for (int i = 0; i < 65535; ++i) {
		s.insert(i);
		if (!m.count(s.bucket(i))) {
			m[s.bucket(i)] = 1;
		}
		else {
			//cout << "got it" << endl;
			++m[i];
		}
	}

	for (auto it = m.begin(); it != m.end(); ++it) {
		cout << it->first << "->" << it->second << endl;
	}


	/*unordered_set <int> s2{ 10,6,7,2 };
	for (auto it = s2.begin(); it != s2.end(); ++it) {
		cout << *it << "->" << s2.bucket(*it) << ' ';
	}*/

	return 0;
}
 ```
