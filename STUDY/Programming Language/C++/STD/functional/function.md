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