인자로 주어진 실수를 내림하여 그보다 작거나 같은 가장 큰 정수를 반환하는 함수

```c++
#include <iostream>
#include <cmath>
int main() { 
double x = 3.6; double result = floor(x); // 3.0 
std::cout << "Floor of " << x << " is: " << result << std::endl; 
return 0; 
}
```

`3.6`을 `3.0`으로 내림한 값을 출력