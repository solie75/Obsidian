# Syntax
```c++
sort ( 배열 시작 주소, 정렬할 마지막 인덱스, 정렬 함수 )
또는
sort ( 벡터 시작 iterator, 벡터 종료 iterator, 정렬 함수)
```

1. array 오름 차순 기본 정렬 ( 배열의 시작 주소와 끝 주소를 넣는다. )

```c++
#include <iostream>
#include <algoritm>

int main()
{
	int a[] = {1, 3, 4, 2, 7, 5};

	for(int i : a)
	{
		std::cout << i << " ";
	}
	std::cout << std::endl;
	std::sort(a, a+6);

	for(int i : a) std::cout << i << " ";
}

---

결과
1 3 4 2 7 5
1 2 3 4 5 7
```

2. 일반 함수를 사용하여 array 내림 차순 기본 정렬

```c++
#include <iostream>
#include <algorithm>

using namespace std;

bool compare(int a, int b){
	return a < b;
}

int main()
{
	int a[] = {1, 3, 4, 2, 7, 5};

	for(int i : a) cout << i << ' ';
	cout << endl;

	sort(a, a + sizeof(a)/sizeof(*a), compare); // 내림차순으로 정렬
	for(int i:a) cout << i << " ";
}
```

3. 람다 함수를 이용한 내림차순 정렬

```c++
#include <iostream>
#include <algorithm>

using namespace std;

int main()
{
	int a[] = {1, 3, 4, 2, 7, 5};

	for(int i : a)
	{
		cout << i << " ";
	}
	cout << endl;

	sort(a, a+ sizeof(a)/sizeof(a*), [](int a, int b){return a > b;});

	for(int i : a)
	{
		cout << i << " ";
	}
}

---
결과
1 3 4 2 7 5
7 5 4 3 2 1
```

4. vector 정렬

내림차순
```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

int main()
{
	vector<int> v{1, 3, 4, 2, 7, 5}

	for(int i : v)
	{
		cout << i << " ";
	}
	cout << endl;

	sort(v.begin(), v.end(), [](int a, int b){return a > b});

	for(int i : v) cout << i << ' ';
}
----
1 3 4 2 7 5
7 5 4 3 2 1
```

```c++
sort(v.begin(), v.end(), greater<int>());
```

5. 사용자 규칙으로 정렬
```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

int main()
{
	vector<int> v{1, 3, 4, 2, 7, 5, 13};
	for(int i: v) cout << i << ' ';

	sort(v.begin(), v.end(), [](int a, int b){
		if(a/10 > b/10) // 왼쪽 항의 십의 자리 수가 더 크면 
		{
			return true; 먼저 정렬
		}
		else
		{
			return a < b; //  그 외는 오름차순으로 정렬
		}
	})

	for(int i : v)
	{
		cout << i << ' ';
	}
}
---
1 3 4 2 7 5 13
13 1 2 3 4 5 7
```