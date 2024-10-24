컴파일 타임에 값을 계산할 수 있도록 해주는 키워드.
이를 통해 컴파일 시점에 상수 표현식을 정의하거나 , 컴파일 타임에 계산될 수 있는 함수를 작성할 수 있다.
-> 컴파일러가 해당 값을 미리 계산하여 런타임 성능을 최적화 할 수 있다.

constexpr 로 선언된 변수는 반드시 컴파일 시점에 초기화 되어야 한다.

constexpr 함수 해당 함수는 컴파일 타임에 계산될 수 있는 함수이다.
```c++
constexpr int square(int n)
{
	return n * n;
}

int main() {
	constexpr int result = square(5); // 컴파일 타임에 계산됨
	int runtineResult = square(10); // 런타임에도 사용 가능.
}
```

C++ 11 에서 constexpr 은 단일 return 문만 갖은 단순 함수였다.
C++ 14 에서 반복문, 조건문 등으로 포함할 수 있게 되었다.
C++ 17 에서 if constexpr 문 이 도입되어 컴파일 타임 조건부 코드 실행이 가능해졌다.
```c++
template<typename T>
constexpr T get_value(T x)
{
	if constexpr (std::is_integral<T>::value)
	{
		return X * 2;
	}
	else
	{
		return x;
	}
}
```

### constexpr 와 const 의 차이

- constexpr : 컴파일 타임에 상수로 평가되어야 하는 변수나 함수, 컴파일 시점에 값이 결정된다.
- const : 변수의 값을 변경할 수 없도록 하지만, 값은 런타임에 결정 될 수 있다.\