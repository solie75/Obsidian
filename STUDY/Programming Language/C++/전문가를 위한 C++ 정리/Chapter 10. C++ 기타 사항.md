  - 참조형
C++ 에서 참조는 다른 변수에 대한 별명. 
참조에 대한 모든 수정 사항은 참조형 변수가 가리키고 있는 실제 변수에 반영된다.

1. 참조형 변수
\- 참조형 변수는 생성하자마자 초기화 해야 한다.
```c++
int x = 3;
int& xRef = x;
```
위의 선언 이후부터 xRef 는 x 의 또 다른 이름이 된다. 
```c++
xRef = 10;
```
위의 코드의 결과로 x 는 10 이 된다. 

\- 보통 참조형 변수는 항상 생성 시점에 초기화 해야 하지만 클래스 멤버 변수로 선언된 참조형 변수는 클래스 생성자에서 초기화 되어야 하기 때문에 선언과 동시에 초기화 하지 않아도 된다.

\- 숫자와 같이 이름을 가지지 않은 값에 대해서는 참조할 수 없다. 단, const 값은 참조할 수 있다.
const 가 아닌 숫자 값으로 참조형 변수를 초기화 하는 것은 변경되지 않는 상수의 특성과 변경이 가능한 참조형의 특징이 서로 맞지 않아 오류를 일으킨다.

\- 참조형 변수의 참조 대상은 선언할 때 한번 초기화 하면 후에 바꿀 수 없다. 단지 그 값만 바꿀 수 있다.
```c++
int x = 3, y = 4;
int& xRef = x;
int& yRef = y;
xRef = y;   //...1)
xRef = yRef;   // ...2)
```
...1) 변수 x 의 값이 4로 변한다. xRef 가 가리키는 대상이 y로 변하지는 않는다.
...2) 도 마찬가지로 값이 이용될 뿐 참조 대상이 바뀌지는 않는다.

\- 포인터에 대한 참조형 변수, 참조형 변수에 대한 포인터
```c++
int* intPtr;
int*& ptrRef = intPtr;
ptrRef = new int;
*ptrRef = 5;
```
ptrRef 는 int 타입 포인터 변수 intPtr 에 대한 참조이다. 

\- 참조에 대한 참조 또는 참조에 대한 포인터는 선언할 수 없다.

2. 참조형 데이터 멤버
참조형 데이터 멤버의 초기화는 생성자의 초기화 리스트에서 이루어진다.

메서드의 파라미터에서 참조형을 사용할 수 있다. 참조형 파라미터는 메서드 내에서 값을 변경했을 때 원본의 값이 변경된다. 참조형 변수를 상수값으로 초기화할 수 없듯이, 참조형 파라미터또한 상수값 인자를 받을 수 없다.

3. 포인터로부터의 참조
```c++
void swap(int& first, int& second)
{
	int temp = first;
	first = second;
	second = temp;
}

int main()
{
	int x = 5, y = 6;
	int* xp = &x, *yp = &y;
	
	swap(*xp, *yp);
	return 0;
}
```
함수의 인자로 사용할 변수가 포인터 일 때, 역참조를 이용해서 단순히 포인터를 참조형으로 변환시킨다. 

4. 참조로 인한 전달의 장점
\- 큰 객체나 struct 를 전달하려면 실행 시간 오버헤드가 크지만 참조로 전달하면 포인트만 전달하기에 실행 시간 오버헤드가 적다.
\- 모든 객체가 값에 의한 전달을 지원하지 않는다. 또한 지원하더라도 깊은 복제가 제대로 되지 않을 수 있다. 예를 들어 동적으로 할당된 메모리를 멤버로 가진다면 반드시 커스텀 복제 생성자를 통해 깊은 복제가 구현되어야 한다.

=> 따라서 int 나 double 과 같은 인자를 수정할 필요가 없는 기본 데이터 타입은 값에 의한 전달로하고 나머지는 거의 항상 참조에 의한 전달을 사용하는 것이 옳다.

5. 참조형 리턴값
참조형 리턴 타입은 리턴되는 객체가 함수나 메서드 종료 후에도 계속 유요할 때만 사용할 수 있다.


6. 참조와 포인터의 선택 기준
참조가 포인터보다 안전하다.
\- 포인터는 유요하지 않은 데이터를 가리킬 수 있지만 참조는 그럴 수 없다.
\- 포인터는 역참조 연산을 해야할 때가 있지만 참조는 역참조 연산이 없어 역참조 연산오류가 없다.

포인터를 꼭 사용해야만 하는 상황
\- 가리키는 대상을 바꾸어야 할 때. 예를 들어 메모리를 동적으로 할당받는다면 그 메모리 영역은 참조형 변수 대신 포인터에 저장해야 한다. 
\- 파라미터에서 포인터로 nullptr 과 같은 디폴트 값을 지정할 수 있지만 참조형 파라미터로는 디폴트 값을 지정할 수 없다.

- 우측값 참조
C++ 에서 좌항은 그 주소를 얻을 수 있거나 변수 이름을 가진 대상이다. 우항은 상수 값이나 임시 객체와 같은 것들이다.
우측값 참조란 우항의 값에 대한 참조로 특히 그 우항이 임시 객체일 때 적용되는 개념이다.
우측값 참조는 임시 객체가 생성되는 코드 구문에서 함수나 메서드, 특히 복제 생성자나 대입 연산자를 컴파일러가 선택할 때 사용된다. 이를 통해 깊은 복제를 하지 않고 포인터 주소만 복제하여 오버헤드를 줄일 수 있다.
*** 임시 객체란 : 실행 도중에 잠깐만 사용되는 객체로, 힙 이외의 공간에 생성된다. 파라미터, 리턴값 등이 있다.

```c++
void incr(int& value)
{
	std::cout << "lvalue" << std::endl;
	++value;
}

void incr(int&& value)
{
	std::cout << "rvalue" << std::endl;
	++value;
}

int main()
{
	int a = 10, b = 20;
	incr(a);
	incr(a + b);
	incr(3);
	incr(std::move(a));   //...1)

	return 0;
}

// 결과
lvalue
rvalue
rvalue
rvalue
```
우측값 참조형 파라미터를 선언할 때 && 를 붙인다. 이 선언으로 임시 객체 타입을 인자로 받을 수 있다.
상수도 우측값 참조형 파라미터를 호출한다.

...1) 일반 변수를 이용해서 우측값 참조형 incr() 함수를 호출하려면 std::move() 함수를 이용하여 좌항 변수를 우측값 참조로 변환한다.

```c++
int& i = 2;   // 오류 : 상수값에 대한 참조
int a = 2, b = 3;
int& j = a + b;   // 오류 : 임시 객체에 대한 참조
```

```c++
int&& i = 2;
int a = 2, b = 3;
int&& j = a + b;
```

- 이동 시맨틱
이동 시맨틱은 이동 생성자와 이동 대입 연산자를 통해 지원된다.
이동 생성자와 이동 대입 연산자는 원본 객체로부터 새로운 객체로 멤버 변수를 복사한 다음 원본 객체의 멤버를 null 값으로 초기화 한다. 멤버 변수에 대해 얕은 복제를 하여 메모리에 대한 소유권을 바꿈으로써 댕글링 포인트나 메모리 릭을 방지한다.

이동 생성자와 이동 대입 연산자는 반드시 noexcept 로 선언되어 컴파일러가 익셉션을 발생시키지 않도록 해야 한다.

```c++
class CTest;

class CTestA
{
private:
	int num1;
	CTest* mTest;
public:
	CTestA(){}
	~CTestA(){}
	CTestA(CTestA&& src) noexcept
	{
		num1 = src.num1;
		mTest = src.mTest;

		// 소유권이 이동되었기 때문에 원본 객체를 리셋한다.
		src.num1 = 0;
		src.mTest = nullptr;
	}
}
```
std::vector 는 동적으로 커지면서 새로운 객체를 담는다. 이러한 동적 확장은 현재보다 더 큰 메모리를 할당 받아 객체를 옮겨 넣는 방식이다. 이동 생성자를 사용하여 오버 헤드를 줄인다.

```c++
class CTest
{
public:
	CTest() { std::cout << "CTest constructor " << std::endl; }
	~CTest() { std::cout << "CTest destructor " << std::endl; }
};

class CTestA
{
private:
	int num1;
	int num2;
	CTest* mTest;

public:
	CTestA(CTestA&& src) noexcept // 이동 생성자
	{
		num1 = src.num1;
		num2 = src.num2;
		mTest = src.mTest;
		
		// 소유권이 이동 되었으니 원본 객체를 리셋
		src.num1 = 0;
		src.num2 = 0;
		src.mTest = nullptr;

		std::cout << "CTestA Move constructor " << this <<  std::endl;
	}

	CTestA(int input1, int input2)
	{
		num1 = input1, num2 = input2; 
		std::cout << "CTestA input int 2 constructor " << this << std::endl;
	}
	CTestA()
		: num1(0)
		, num2(0)
		, mTest(nullptr)
	{
		std::cout << "CTestA default constructor " << this << std::endl;
	};
	~CTestA() 
	{
		std::cout << "CTestA defualt destructor " << this << std::endl;
	};

	CTestA& operator=(CTestA& src)
	{
		if (this == &src)
		{
			return *this;
		}

		// 데이터의 얕은 복제
		num1 = src.num1;
		num2 = src.num2;
		mTest = src.mTest;

		std::cout << "CTestA copy operator " << this << std::endl;

		return *this;
	}

	CTestA& operator=(CTestA&& rhs) noexcept // 이동 대입 연산자
	{
		if (this == &rhs)
		{
			return *this;
		}

		// 데이터의 얕은 복제
		num1 = rhs.num1;
		num2 = rhs.num2;
		mTest = rhs.mTest;

		// 소유권이 이동 되었으니 원본 객체를 리셋
		rhs.num1 = 0;
		rhs.num2 = 0;
		rhs.mTest = nullptr;

		std::cout << "CTestA Ref Copu operator " << this << std::endl;

		return *this;
	}
};

int main()
{
	std::vector<CTestA> vec;
	for (int i = 0; i < 3; i++)
	{
		std::cout << "Iteration " << i << std::endl;
		vec.push_back(CTestA(100, 100));
		std::cout << std::endl;
	}

	CTestA A(2, 3);
	A = CTestA(3, 2);   //...1)
	CTestA A2(5, 6);
	A2 = A;

	return 0;
}

// 결과
Iteration 0
CTestA input int 2 constructor 000000A2B9AFF678
CTestA Move constructor 0000028D5BA71A00
CTestA defualt destructor 000000A2B9AFF678

Iteration 1
CTestA input int 2 constructor 000000A2B9AFF678
CTestA Move constructor 0000028D5BA70300
CTestA Move constructor 0000028D5BA702F0
CTestA defualt destructor 0000028D5BA71A00
CTestA defualt destructor 000000A2B9AFF678

Iteration 2
CTestA input int 2 constructor 000000A2B9AFF678
CTestA Move constructor 0000028D5BA6D710
CTestA Move constructor 0000028D5BA6D6F0
CTestA Move constructor 0000028D5BA6D700
CTestA defualt destructor 0000028D5BA702F0
CTestA defualt destructor 0000028D5BA70300
CTestA defualt destructor 000000A2B9AFF678
// vector 에 push_back 될 때마다 vector 의 모든 요소들이 복사된다.

CTestA input int 2 constructor 000000A2B9AFF558
CTestA input int 2 constructor 000000A2B9AFF6A8
CTestA Ref Copu operator 000000A2B9AFF558
CTestA defualt destructor 000000A2B9AFF6A8    // ..1)의 우항의 소멸자
CTestA input int 2 constructor 000000A2B9AFF588   
CTestA copy operator 000000A2B9AFF588
CTestA defualt destructor 000000A2B9AFF588   // A2 의 소멸자
CTestA defualt destructor 000000A2B9AFF558   // A 의 소멸자 
CTestA defualt destructor 0000028D5BA6D6F0   // vec 소멸자
CTestA defualt destructor 0000028D5BA6D700   // vec 소멸자
CTestA defualt destructor 0000028D5BA6D710   // vec 소멸자
```

- const  키워드
\- const 변수, const 파라미터

전역 변수나 클래스 데이터 멤버를 포함한 모든 변수는 const 로 지정될 수 있고 이는 대표적으로  \#define 역할을 한다.
```c++
const double PI = 3.141592653589793238462;
```

함수나 메서드의 파라미터를 const 로 지정하여 함수 바디 내에서 해당 파라미터 값의 변경이 불가능하게 한다.
```c++
void func(const int param)
{
	// 변수 paran 에 대한 변경은 컴파일 에러를 일으킨다.
}
```

\- const 포인터
```c++
int* ip;
ip = new int[10];
ip[4] = 5;
```
위의 모드에 대해서 const 를 적용하는 두 가지 방버은 다음과 같다.

1. 포인터가 가리키는 데이터가 변경되는 것을 막는 방법
```c++
const int* ip;  // 또는 int const* ip;
ip = new int[10];
ip[4] = 5; // 컴파일 안됨!
```
-> ip 가 가리키는 데이터는 수정할수 없게 된다.

2. 가리키는 데이터가 아닌 포인터 ip 자체에 대한 변경을 막는 방법
```c++
int* const ip = nullptr; // 또는 int* const ip = new int[10]
ip = new int[10]; // 컴파일 안됨
ip[4] = 5; // 오류, null 포인터에 대한 역참조
```
-> ip 자체에 대한 변경을 불가능하다. 단 선언과 동시에 초기화해야 컴파이된다.

3. 포인터 자체 뿐만 아니라 가리키는 데이터 까지 const 로 지정.
```c++
int const* const ip = nullptr;   //...1)
// 또는
const int* const ip = nullptr;
```

*** const 키워드는 기본적으로 자신의 왼쪽에 오는 대상에 적용된다. 
const int* 와 같이 const 가 맨 앞에 오는 예외 상황을 제외하면
...1 )  에서 첫번째 const 는 int 에 적용되어 포인터가 가리키는 값이 변경이 불가능하게 하고, 두번째 const 는 * 에 적용하여 포인터 자체가 변경이 불가능 하게 된다.

\- const 참조
참조형 변수는 한 번 초기화 되면 변경할 수 없기 때문에 이미 const 의 성질을 띄고 있다. 따라서 참조형 변수를 const 로 제한하는 것이 허용되지 않는다.
```c++
int z;
const int& zRef = z;  // 또는 int const& zRef
zRef = 4; // const 값에 대한 변경 시도로 컴파일 에러가 발생한다.
```
-> zRef 를 const 로 선언하더라고 변수 z 에는 아무런 영향이 없다. (z 값의 변경이 가능하다.)
-> 오버헤드 등을 줄이기 위해 파라미터에 참조형 을 사용하는 경우 원본의 변경을 막기위해 const 참조로 파라미터 선언이 이루어진다.

\- const 메서드
메서드를 const 로 선언하면 그 함수 안에서는 클래스 멤버 데이터 중 mutable 로 선언된 것을 제외하고는 그 값을 변경할 수 없다.

\- constexpr 키워드

다음과 같이 상수 표현이 요구되는 곳에 상수가 아닌 구문을 사용하면 C++ 에서는 오류로 처리된다.
```c++
const int getArraySize() {return 32;}
int main()
{
	int myArray[getArraySize()]; // C++ 에서 허용되지 않는다.
	return 0;
}
```

constexpr 키워드를 이용하면 다음과 같다.
```c++
constexpr int getArraySize() {return 32;}
int main()
{
	int myArray[getArraySize()]; // 허용됨
	return 0;
}
```

constexpr 을 다음처럼 상수 표현식의 구성 요소로도 사용가능하다.
```c++
int myArray[getArraySize() + 1]; // 허용됨
```

constexpr 을 이용하면 컴파일러가 최적화를 훨씬 효과적으로 할 수 있다.

가만 함수를 constexpr 로 선언하는데에는 다음의 제약 사항들이 지켜져야 한다.

1. 상수 표현식을 컴파일러가 컴파일 타임에 계산 완료할 수 있어야 한다.(해당 표현식이 런타임에서만 부가 휴과를 일으켜서는 안된다.)
2. 함수 본체의 리턴문은 1개여야 한며 리터널 타입이어햐 한다.
3. goto 문이나 try/cath 블록이 포함되어서는 안되며 익셉션을 일으켜서도 안된다. 단, 다른 constexpr 함수를 호출하는 것은 허용된다.
4. 인자는 모두 리터럴 타입이어야 한다.
5. dynamic_cast 와 new, delete 가 혀용되지 않는다.

constexpr 생성자를 이용하면 자체적으로 정의한 타입으로도 상수 표현 변수를 만들수 있다. 이때
1. 모든 생성자 인자는 리터럴 타입이어야 한다.
2. 생성자 바디는 constexpr 바디가 같은 요구사항을 똑같이 만족해야 한다.
3. 모든 데이터 멤버는 상수 표현식으로 초기화 되어야 한다.

```c++
class CRect
{
private:
	int mWidth;
	int mHeight;
public:
	constexpr CRect(int width, int height)
		: mWidth(width)
		, mHeight(height)
	{

	}
	constexpr int getArea() const { return mWidth * mHeight; }
};

int main()
{  
	constexpr CRect r(8, 2);   //...1)
	int myArray[r.getArea()];   //...2)
	return 0;
}
```
...1 )  에서 constexpr 을 선언하지 않으면 ...2) 의 r 에서 ' 식에 상수 값이 있어야 합니다. 런타임 스토리지에 엑세스 하려고 합니다. ' 오류를 일으킨다. 

- static 키워드
static 데이터 멤버는 다른 멤버와 달리 객체 별로 따로 존재하지 않는다. 대신 클래스 수준에서 모든 객체에 공통으로 속하는 단 하나의 데이터만 존재한다.

\- static 링킹
C++ 소스 파일이 컴파일 될 때 소스별로 독립적인 오브젝트 파일이 생성되고 이 오브젝트 파일들을 하나로 연결함으러써 실행 바이너리가 만들어진다.
이때 함수나 변수의 이름을 기준으로 오브젝트 파일에서 일어나는 함수 호출이나 변수 참조가 서로 연결되는데 같은 소스 안에서 연결되는 것을 내부 링킹(static 링킹), 외부에서 연결되는 것을 외부 링킹 이라 한다. 
기본적으로 함수나 전역 변수는 외부 링킹이 적용된다. 

\- static 을 내부 링킹으로 강제하는 방법

```c++
// test.cpp
#include <iostream>
void f();
void f() { std::cout << 'f\n'; }

// main.cpp
void f();   //...1)
int main()
{
	f();
	return 0;
}

// 결과
26122
```
위의 코드는 문제 없이 컴파일 된다. main() 에서 호출하는 함수  f() 는 외부 링크를 통해 다른 파일에서 찾아지는 것이다.
...1) 이 static void f() 이면 'void f (void)' 정적 함수를 선언했으나 정의하지 않았습니다. 컴파일 오류를 일으킨다.

```c++
// test.cpp
#include <iostream>
static void f();
void f() { std::cout << 'f\n'; }

// main.cpp
void f();
int main()
{
	f();
	return 0;
}
```
위의 코드는 컴파일은 되지만 링킹 단계에서 오류를 일으킨다. 링크 단계에서 f () 가 내부 링크만 허용하고 있기 때문에 외부 파일인 main.cpp 에서는 f( )를 찾을 수 없다.
![[Pasted image 20231127154543.png]]

\- 무명 네임스페이스
어떤 함수를 static 으로 선언하는 대신 이름이 없는 네임스페이스로 감싸면 내부 링킹이 적용된다.
```c++
// 위의 코드에서 test.cpp 를 무명 네임 스테이스로 바꾸면 다음과 같다.

#include <iostream>
namespace {
	void f();
	void f() { std::cout << 'f\n'; }
}
```
같은 소스 파일의 무명 네임스페이스 블록 안에 있는 코드는 서로 이용할 수 있지만, 외부에 있는 소스 파일에서는 접근할 수 없다.

- extern 키워드
extern 키워드는 static 키워드와 반대의 기능을 가진다. 즉, 명시적으로 외부 링킹할 대상을 선언한다.
변수의 경우 extern으로 지정하면 컴파일러가 코드를 정의가 아니라 선언으로 취급한다. 따라서 컴파일러가 메모리를 할당하지 않는다. 할당하려면 다음과 같이 변수 선언을 한번 더 해야 한다.
```c++
extern int x;
int x = 3;
```
두번 선언하는 대신 extern 선언과 동시에 값을 초기화 하면 컴파일러가 정의로 취급한다.
```c++
extern int x = 3;
```

\- 로컬 이외의 변수들의 초기화 순서
모든 선젹 변수와 static 클래스 데이터 멤버는 main() 함수가 실행도기 전에 초기화가 이루어진다.
로컬 변수와 달리 여로 소스파일에 나뉘어 선언된 전역 변수에 대해서는 초기화 순서가 특별히 보증되지 않는다.
소멸 순서 또한 정의되어 있지 않다.

- 타입과 캐스팅

\- typedef
typedef 를 이용하여 이미 정의된 타입에 또 다른 이름을 부여할 수 있다.
```c++
typedef int* IntPtr;
```
-> int* 타입에 대해 IntPtr 이라는 새로운 이름을 부여한 것이다.

\- 함수 포인터에 대한 typedef
virtual 메서드가 있기에 함수포인터를 이용할 일이 별로 없지만 동적 연결 라이브러리(dll) 에 정의된 함수의 포인터를 얻는 경우에 사용된다.

예를 들어 dll 에 함수 MyFunc() 이 있고 해당 라이브러리를 로딩하여 함수 MyFunc()을 호출해야한다고 할때.
윈도우에서는 LoadLibrary() 함수로 커널 API 를 호출하여 런타임에 라이브러리를 로딩할 수 있다 가정하자.
```c++
HMODULE lib = ::LoadLibrary(_T("library name"));
```
위의 함수의 호출결과를 라이브러리 핸들이라 한다. 로딩에 실패한 경우에는 NULL 값이 리턴된다.

MyFunc() 함수의 프로토 타입이 다음과 같다고 가정하자
```c++
int __stdcall MyFunc(bool b, int n, const char* p);
```
여기에서 __stdcall 은 마이크로소프트에 종속적인 지시자로 파라미터가 함수에 어떻게 전달되고 메모리에 정리되는지를 규정한다.

typedef 를 이용해서 위 함수의 프로토 타입에 대한 포인터를 정의해보면
```c++
typedef int (__stdcall *MyFuncProc)(bool b, int n, const char* p);
```
-> 새로 정의된 이름은 MyFuncProc 이다.

라이브러리를 로딩하고 함수 포인터에 대한 typedef 를 완료하면 다음과 같이 함수 포인터 주소를 라이브러리로부터 얻을 수 있다.
```c++
MyFuncProc myProc = ::GetProcAddress(lib, "MyFunc");

// 호출 예시
myProc(true, 3, "Play Game!!");
```


- 타입 에일리어스
```c++
typedef int MyInt;

// 위의 typedef 예시를 type aliases (타입 에일리어스)로 바꿔서 표현하면
using MyInt = int;
```

다른 예를 들어보자
```c++
typedef int (*FuncType)(char, double);

// 위의 typedef 를 type aliases 로 바꾸면 다음과 같다.
using FuncType = int(*)(char, double);
```

\- 캐스팅

1. const_cast
const 변수의 상수 속성을 없애고자 할 때 사용한다.
원래 const 로 지정한 이유에 반하는 캐스팅으로 사용을 지양해야 한다.
```c++
// 예시
extern void Method(char* str);
void f(const char* str)
{
	Method(const_cast<char*>(str));
}
```

2. static_cast
논리적으로 변환 가능한 타입을 변환한다. 
```c++
static<new-type>(expression)
```
실수와 정수, 열거형과 정수형, 실수와 실수 사이의 변환 등을 허용한다.
포인터 타입을 다른 것으로 변환하는 것을 허용하지 않는다. (컴파일 에러)
그러나 상속 관계에 있는 포인터 끼리 변환이 가능하다.
컴파일 때에 형변환에 대한 타입 오류를 잡아준다. (런타임 타입 검사는 하지 않는다.)

런타임 타임 검사가 수행되는 안전한 캐스팅을 하려면 dynamic_cast 를 사용한다.

```c++
class CTestA
{
public:
	CTestA() {}
	virtual ~CTestA() {}
};

class CTestB : public CTestA
{
public:
	CTestB() {}
	virtual ~CTestB(){}
};

int main()
{  
	CTestA* a;
	CTestB* b = new CTestB();
	a = b;
	b = static_cast<CTestB*>(a);
	CTestA A;
	CTestB B;
	CTestA& ARef = B;
	CTestB& BRef = static_cast<CTestB&>(ARef);
	return 0;
}
```

. 일반 변수 타입간의 static_cast
```c++
int main()
{
	double d = 13.24;
	float f = 10.12f;

	double tmp_double;
	int tmp_int;
	float tmp_float;

	tmp_int = static_cast<int>(d);
	std::cout << tmp_int << std::endl;
	
	tmp_float = static_cast<float>(d);
	std::cout << tmp_float << std::endl;

	tmp_double = static_cast<double>(f);
	std::cout << tmp_double << std::endl;

	return 0;
}

// 결과
13
13.24
10.12
```

