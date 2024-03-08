c++ 에서 ( ) 를 붙여서 호출할 수 있는 모든 것을 Callable 이라 정의한다.

struct 객체를 함수 처럼 호출하여 사용한다.

```c++
struct S {
	void operator()(int _a, int _b)
	{
		std::cout << "A + B = " << _a + _b << std::endl;
	}
};

int main()
{
	S Temp_Obj;

	Temp_Obj(3, 6);
	// Temp_Obj 는 함수가 아니라 S 클래스의 객체이다. 하지만 마치 함수와 같이 호출하고 있다.
	// 실제로는 Temp_Obj.operate()(3, 6) 을 한 것과 같다.

	return 0;
}

// 결과
A + B = 9
```

람다 함수

```c++
int main()
{
	auto Fuc = [](int a, int b) {
		std::cout << "A + B = " << a + b << std::endl;
	};

	Fuc(3, 6);
}

// 결과
A + B = 9
```

c++ 에서는 이러한 Callable 들을 객체의 형태로 보관 가능한 std::function 이라는 클래스를 제공한다. std::function 의 경우 함수 뿐 아니라 모든 callable 들을 보관할 수 있는 객체이다.

[[STUDY/Programming Language/C++/STD/functional/function|function]]

std::function 예시
```c++
#include <functional>
#include <iostream>
#include <string>

int Func1(const std::string& _s) {
	std::cout << "Func1 호출 " << _s << std::endl;
	return 0;
}

struct Func2 {
	void operator()(char _c) {
		std::cout << "Func2 호출 " << _c << std::endl;
	}
};

int main()
{
	std::function<int(const std::string&)> F1 = Func1;
	std::function<void(char)> F2 = Func2();
	std::function<void()> F3 = []() {std::cout << "Func3 호출" << std::endl; };

	F1("WoW");
	F2('vov');
	F3();

	return 0;
}

// 결과
Func1 호출 WoW
Func2 호출 v
Func3 호출
```

class 의 맴버 함수를 std::function 에 저장하는 경우
```c++
class A 
{
private:
	int num;

public:
	A(int _i) : num(_i) {};
	int PrintNum() { std::cout << "비상수 함수 호출, 숫자 : " << num << std::endl; return num; }

	int PrintConstNum() const { std::cout << "상수 함수 호출, 숫자 : " << num << std::endl; return num; }
};

int main()
{
	A a(5);

	std::function<int(A)> func1 = &A::PrintNum; // ...ㄱ
	int (A::*tempOP)() = &A::PrintNum;

	
	

	std::function<int(const A&)> func2 = &A::PrintConstNum;

	func1(a);
	func2(a);

	return 0;
}
```

...ㄱ) 멤버 함수를 'std::function' 에 바인딩 할 때는 해당 멤버 함수에 대한 함수포인터를 바인딩 대상으로 취해야한다. 함수 포인터를 만들기 위해서는 클래스의 인스턴스가 아니라 클래스 그 자체에 대한 정보가 필요하다. 따라서 'a' 가 아닌 '&A::PrintNum' 을 사용하여 함수 포인터를 생성해야 한다. 또한 std::function 의 입장에서 해당 함수 포인터 형인 'int (A::\*함수이름)()' 을 대입 받기 위해서 인자형으로 'A' 또는 'A&' 를 두어야 한다.

** 레퍼런스를 인자로 받는 함수들의 경우
```c++
// struct S
{
	int data;
	S(int _data) : data(_data) { std::cout << "일반 생성자 호출!" << std::endl; }
	S(const S& _s) : data(_s.data) { std::cout << "복사 생성자 호출!" << std::endl; }

	S(S&& _s) : data(_s.data) {
		std::cout << "이동 생성자 호출!" << std::endl;
	}
};

void AddThree(S& _s1, const S& _s2) { _s1.data = _s2.data + 3; }

int main()
{
	S s1(1), s2(2);

	std::cout << "Before : " << s1.data << std::endl;
	auto AddThree_with_s1 = std::bind(AddThree, s1, std::placeholders::_1);
	AddThree_with_s1(s2);

	std::cout << "After : " << s1.data << std::endl;
}

// 결과
일반 생성자 호출!
일반 생성자 호출!
Before : 1
복사 생성자 호출!
After : 1
```

위에 s1 의 값이 바뀌지 않았음을 알 수 있다.  이는 bind 함수로 인자가 복사 되어서 전달되기 때문이다.
이를 해결하기 위해서 명시적으로 s1 의 레퍼런스를 전달 해주어야 한다.

```c++
auto AddThree_with_s1 = std::bind(AddThree, s1, std::placeholders::_1);
=> auto AddThree_with_s1 = std::bind(AddThree, std::ref(s1), std::placeholders::_1);

// 결과
일반 생성자 호출!
일반 생성자 호출!
Before : 1
After : 5
```

