값을 특정 범위 내에 가둬놓고 싶을 때 사용하는 함수.

예시
```c++
#include <algorithm>

int func(int x){
	return std::clamp(x, 1, 10);
}

int main () {
	int x = 5;
	x = func(x++); // 6
	x = 10;
	x = func(x++); // 10;
	return 0;
}
```

### reference

https://en.cppreference.com/w/cpp/algorithm/clamp

