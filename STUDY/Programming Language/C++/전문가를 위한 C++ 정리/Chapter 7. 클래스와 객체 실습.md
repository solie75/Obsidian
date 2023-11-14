스프레드시트 프로그램을 구현한다.

```c++
class CSpreadsheetCell
{
private :   //...6)
	double mValue;
public :
	void setValue(double inputValue);
	double getValue() const;   //...2)
};   //...1)
```

1. 클래스 정의는 작업 구문이기 때문에 세미클론( ; )으로 끝나야 한다.
2. 객체의 프로퍼티에 변경이 없는 메서드는 const 로 선언하는 것이 좋다.
3. 각 파라미터의 값은 클래스가 인스턴스화한 객체에 저장된다.
4. 맴버 변수(파라미터) 와는 다르게 메서드의 구현 내용은 모든 객체가 공유한다. 
5. 맴버 간, 메서드 간 또는 맴버와 메서드 간에 이름이 같아서는 안된다.
6. 모든 메서드와 맴버 변수는 public, protected, private 세가지 접근 식별자로 분류한다.
   \- 접근자 지정 이후에 선언되는 모든 메서드와 맴버 변수는 다른 접근자 지정 전까지 해당 접근 식별자를 따른다.
   \- class 의 디폴드 접근 식별자는 private 로 사용자가 접근자 지정을 하지 않으면 자동으로 private 접근 속성이 된다.
   \- public : 제한 없이 접근
   \- private : 메서드나 데이터 멤버를 해당 클래스에서만 접근 가능. 파생 클래스에서의 접근 또한 불가능
   \- protected : 같은 클래스 내 또는 파생클래스에서 접근 가능.
1. <span style="color:#ffd33d"> struct 도 class 처럼 메서드를 가질 수 있다. struct 가 class 와 다른 점은 디폴드 접근 식별자는 public 이라는 것이다. </span>

```c++
struct CSpreadsheetCell
{
	void setValue(double inputValue);
	double getValue() const;
private :
	double mValue;	
}
```

- 선언 순서
  메서드, 맴버 변수, 접근자는 어떤 순서로든 선언할 수 있다.
  사용한 접근자를 반복하여 사용할 수 있다.
  
- 메서드 정의
  클래스 정의에서는 메서드 프로토 타입만 선언 하고 몸체는 따로 구현한다. 이때 클래스 정의가 메서드 구현 앞에 있어야 한다. 보통 클래스 정의를 .h 헤더 파일에서 하고 그 헤더파일을 include 하는 소스파일에서 구현한다.
```c++
// CSpreadsheetCell.cpp

#include "CSpreadsheetCell.h"
void CSpreadsheetCell::setValue(double inputValue)
{
	mValue = inputValue;
}
double CSpreadsheetCell::getValue() const
{
	return mValue;
}
```

1. 클래스 이름 뒤의 :: 를 스코프 지정 연산자라 한다. 이 스코프 지정 연산자가 컴파일러에 setValue() 메서드가 CSpreadsheetCell 클래스에 속한 것임을 알려준다.

- 데이터 맴버 접근
  클래스의 메서드는 그 클래스가 인스턴스화한 특정 객체를 대상으로 실행된다.
  메서드 몸체 안에서는 객체가 해당하는 클래스의 모든 맴버에 접근할 수 있다. 

- 메서드 호출
  메서드 안에서 같은 클래스의 다른 메서드를 호출할 수 있다.
  ```c++
// 클라이언트에서 string 으로 데이터를 지정하면 해당 문자열을 double로 변환하고, double 을 string 으로도 변환할 수 있는 코드를 구현한다. 이때 텍스트가 숫자를 표현하고 있지 않으면 double 값은 0.
// CSpreadsheetCell.h

#include <string>

class CSpreadsheetCell
{
private:
	double mValue;
	std::string mString;

	//...1)
	std::string doubleToString(double inputValue) const;
	double stringToDouble(const std::string& inputString) const;
	
public:
	void setValue(double inputValue);
	double getValue() const;

	void setString(const std::string& inputString);
	const std::string& getString() const;
};
```
1.  double 과 string 간 상호 변환을 위한 private 접근 속성의 편의 메서드 2개

```c++
// CSpreadsheetCell.cpp
#include "CSpreadsheetCell.h"
#include <iostream>
#include <sstream>

std::string CSpreadsheetCell::doubleToString(double inputValue) const
{
    std::ostringstream ostr;
    ostr << inputValue;
    return ostr.str();
}

double CSpreadsheetCell::stringToDouble(const std::string& inputString) const
{
    double temp;
    std::istringstream istr(inputString);
    istr >> temp;
    if (istr.fail() || !istr.eof())
    {
        return 0;
    }
    return temp;
}

void CSpreadsheetCell::setValue(double inputValue)
{
    mValue = inputValue;
    mString = doubleToString(mValue);
}

double CSpreadsheetCell::getValue() const
{
    return mValue;
}

void CSpreadsheetCell::setString(const std::string& inputString)
{
    mString = inputString;
    mValue = stringToDouble(mString);
}

const std::string& CSpreadsheetCell::getString() const
{
    return mString;
}
```

참조 : [[sstream]]

- this 포인터
  모든 일반 메서드는 자신이 호출된 객체에 접근할 수 있는 '숨겨져 있는' 파라미터를 넘겨받는다. 이것이  this 포인터이다. 이 포인터를 통해 데이터 맴버에 접근하거나 메서드를 호출하고 다른 메서드의 파라미터로 전달할 수도있다.
  \- this 포인터를 다른 함수나 다른 클래스의 메서드를 호출할 때 파라미터로 활용하는 예시
  ```c++
  // CSpreadsheetCell.cpp 에서

// 독립 함수
void printCell(const CSpreadsheetCell& inputCell)
{
	std::cout << inputCell.getString() << std::endl;
}

void CSpreadsheetCell::setValue(double inputValue)
{
    mValue = inputValue;
    mString = doubleToString(mValue);
    printCell(*this); //...1)
}
```
1. this 포인터를 CSpreadsheetCell 의 참조에 대응하는 파라미터로 넘겨줄 수 있다.

- 객체의 이용
  class 는 그 자신을 자료형 삼는 변수를 선언함으로써 class '객체' 를 생성하게 된다.
  객체 생성 방법은 스택 또는 힙을 사용할 수 있다. 두 경우 모두 배열로 저장할 수 있다.
\-  스택에 객체를 생성하는 경우
```c++
int main()
{
	CSpreadsheetCell myCell, anotherCell;

	myCell.setValue(6);   //...1)
	anotherCell.setString("3.2");

	std::cout << "Cell 1 : " << myCell.getValue() << std::endl;
	std::cout << "Cell 2 : " << anotherCell.getValue() << std::endl;
}
```
  1. ' . ' 은 도트 연산자로 객체를 대상으로 메서드를 호출할 때 이용한다.

\- 힙에 객체를 생성하는 경우
new 연산자를 이용해서 동적으로 객체를 생성한다.
```c++
int main()
{
	auto pSmartCell = make_unique<SpreadsheetCell>(4);

	CSpreadsheetCell* pMyCell = new CSpreadsheetCell(5);
	CSpreadsheetCell* pAnotherCell = nullptr;   //...1)
	pAnotherCell = new CSpreadsheetCell(4);   //...2_)

	delete pAyCell;
	pMyCell = nullptr;
	delete pAnotherCell;
	pAnotherCell = nullptr;
}
```
1. <span style="color:#ffd33d"> 스택 객체와 달리 객체의 포인터 변수를 선언할 때에는 바로 생성자를 호출하지 않아도 된다. 그리고 필수는 아니지만 nullptr 로 초기화 해 두는 것이 바람직하다. </span>
2. new 연산자를 이용해서 실제로 객체를 생성하는 시점에 생성자 파라미터를 클래스명에 전달하면서 생성자를 호출한다.

*** new 연산자에서 ( ) 와 [ ] 의 차이 : [[new 와 new[]의 차이]]

- 디폴트 생성자
  파라미터가 없는 생성자를 디폴트 생성자라 한다.
  객체 저장에 std::vector 와 같은 STL 컨테이너를 이용하려면 디폴트 생성자를 반드시 만들어야한다.

```c++
CSpreadsheetCell myCell;
myCell.setValue(6);

CSpreadsheetCell anotherCell();   //...(컴파일은 된다) ...1)
anotherCell.setValue(3);   //...(컴파일 되지 않는다.)
```
 스택 기반의 객체에 디폴트 생성자를 호출할 대에는 다른 생성자와 달리 함수 호출 형태를 쓰면 안되다. 
 1. 디폴트 생성자를 함수 호출 형태로 사용하게 되면  함수명이 anotherCell 이고 공백 파라미터에 리턴 타입이 CSpreadsheetCell 인 함수로 인식하기 때문이다.

- 컴파일러에 의해 생성되는 디폴트 생성자
  사용자가 디폴트 생성자를 코딩하지 않으면 컴파일러가 자동으로 디폴트 생성자를 만들어준다.
  컴파일러에 의해  생성된 디폴트 생성자는 클래스 안에 속한 맴버들에 대해서도 디폴트 생성자를 호출한다.
  <span style="color:#ffd33d"> 사용자가 직접 생성자를 코딩하는 순간 생성자의 형태와는 관계없이 컴파일러의 디폴트 자동 생성이 일어나지 않는다. </span>

- 명시적으로 삭제된 생성자
  컴파일러가 자동으로 생성하는 디폴트 생성자 까지 포함해서 생성자가 전혀 정의 되지 않는 클래스를 만들 수 있다.
  ```c++
  class MyClass
  {
	  public:
		  MyClass() = delete;
  }
```

- 생성자 초기화 리스트
```c++
CSpreadsheetCell::CSpreadsheetCell()
	: mValue()
{}
```
c++ 에서 객체를 생성할 때는 생성자를 호출하기 전에 모든 데이터 맴버가 먼저 생성되어 메모리에 할당된 생태여야 한다. 이때 객체 타입인 데이터 맴버는 생성자가 호출되고 기본타입은 그 데이터가 할당된 메모리에 남아 있는 임의의 값을 가진다. -> <span style="color:#ffd33d"> 생성자 초기화 리스트는 데이터 맴버의 초기화가 객체 갱성 시점에 즉시 일어날 수 있도록 해준다. </span>
생성자 초기화 리스트는 이러한 과정에서의 맴버에 대한 생성자 호출과 기본 타입 데이터 초깃값을 선택할 수 있게 한다. -> <span style="color:#ffd33d"> 데이터 맴버 클래스가 디폴트 생성자를 제공하지 않는다면 반드시 생성자 초기화 리스트를 이용해서 그 맴버 클래스의 명시적인 생성자를 호출해주어야 한다 </span>.

\- 위의 내용을 예시를 들면 다음과 같다.
```c++ 
// main 함수와 아래 세 개의 클라스가 있다고 가정할 때 (세 클라스 모두 선언부분만 코딩되어 있음)
class CTestOne
{
private :
	int mNum;
public :
	CTestOne();
	~CTestOne();

	void SetValue(int inputNum);
}

class CTestTwo
{
private :
	int mNum;
public :
	CTestTwo(int initNum);
	~CTestTwo();

	void SetValue(int inputNum);
}

class CSpreadsheetCell
{
private :
	double mValue;

	CTestOne mTest1;   //...3)
	CTestTwo mTest2(100);   //...2)
	
public:
	void SetValue(double inputValue);
}

int main()
{
	CSpreadsheetCell mCell;   //...1)
	return 0;
}
```
1. 2가 3과 같이 인자를 받지 않는 'CTestTwo mTest2' 형태 였다면  '기본 생성자를 참조할 수 없습니다. 삭제된 함수입니다.' 오류가 발생한다. 이는 CTestTwo 클래스는 디폴트 생성자가 없어 1의 생성자 리스트에서 CTestTwo 의 디폴트 생성자를 못찾기 때문이다.

\- 다음의 특정 데이터 타입은 생성자의 몸체가 아닌 생성자 초기화 리스트로만 초기화 해야한다.
	1. const 데이터 맴버
	   const 변수는 생성된 이후에 값을 대입할 수 없다. 이 변수에 대한 초깃값 할당은 반드시 변수 생성 시점에 해야 한다.
	2. 참조형 데이터 맴버
	3. 디폴트 생성자가 없는 객체 타입 맴버
	4. 디폴트 생성자가 없는 베이스 클래스

\- <span style="color:#ffd33d"> 각 맴버를 초기화 할 때 초기화 리스트에 등장하는 순서로 초기화 하는 것이 아니라 클래스 정의에서 그 맴버들이 등장하는 순서로 초기화 한다. </span>

- 복제 생성자
  객체의 복제본을 만들 때 사용된다.
  프로그래머가 명시적으로 복사 생성자를 만들지 않으면 컴파일러가 자동으로 만든다.
  <span style="color:#ffd33d"> 컴파일러가 자동으로 생선하는 복제 생성자는 기본 타입 맴버는 그대로 복제하고 객체 타입 맴버는 그 객체의 복제 생성자를 호출한다. </span>
  ```c++
  // CSpreadsheetCell 의 복사 생성자 정의
class CSpreadsheetCell
{
	public :
	CSpreadsheetCell(const CSpreadsheetCell& src);
}
```
복제 생성자는 파라미터로 받는 원본 객체에 대해 상수 참조 하므로 const 와 & 가 붙어 있다.
생성자 안에서는 원본 객체가 가진 모든 맴버 필드에 대해 복제 작업을 해야한다.
```c++
// 복제 생성자 구현 예시
CSpreadsheetCell::SpreadsheetCell(const CSpreadsheetCell& src)
	: mValue(src.mValue), mString(src.mString)
{}
```

함수 호출에 파라미터를 넘기는 것은 파라미터의 대상이 되는 값의 복제 값이다. 따라서 <span style="color:#ffd33d"> 클래스 객체를 함수나 메서드의 인자로 전달하면 컴파일러에 의해 복제 생성사가 호출되어 새로운 객체가 만들어진다. </span>
<span style="color:#ffd33d"> 함수나 메서드에서 객체를 리턴할 때도 호출된다. 이때는 컴파일러가 임시 객체를 만들어서 복제 생성자를 호출한다. </span>

파라미터를 전달할 때 const 키워드를 사용하면 복제 작업 없이 참조로 전달되어 오버헤드를 줄일 수 있다.

복사 생성자도 디폴트 생성자와 같이 default 와 delete 키워드를 이용해서 컴파일러의 자동 생성을 명시적으로 지정하거나 방지할 수 있다.

```c++
CSpreadsheetCell(const CSpreadsheetCell& src) = default;
CSpreadsheetCell(const CSpreadsheetCell& src) = delete;
```

- 균일한 초기화
```c++
class CTest
{
public:
	CTest() {std::cout < "Test!!" << std::ensl;}
}

int main()
{
	CTest mTestOne();   //...1)
	CTest mTestTwo{};   //...2)
	return 0;
}
```
\- 1의 경우 컴파일 에러를 일으킨다. CTest 의 생성자를 호출하는 것이 아니라 반환값이 CTest 이고 파라미터가 없는 함수 mTestOne() 을 선언한 것이기 때문이다.
\- 위의 문제를 해결하기 위해 ( ) 대신 { } 를 사용하는 것으로 이를 <span style="color:#ffd33d"> 균일한 초기화 라고 한다. </span>
\- ( ) 를 이용한 생성과 { } 를 이용한 생성은 일부 암시적 타입 변환들을 불허한다는 차이가 있다.
```c++
class CTest
{
public:
	CTest(int x) {std::cout < "input Number : " << x << std::ensl;}
}

int main()
{
	CTest mTestOne(2.5);   //...1)
	CTest mTestTwo{2.5};   //...2)
	return 0;
}
```
\- 1 의 경우 2.5 를 임시적 변환하여 x 에 2 를 전달하지만 2의 경우 명시적 변환을 하지 못해 double 을 int 형으로 변환할 수 없다는 컴파일 오류를 일으킨다. -> { } 를 사용하면 원하지 않는 타입 캐스팅을 방지할 수 있다.
\- 또한 { } 를 사용하여 함수 리턴 시에 굳이 생성하는 객체의 타입을 다시 명시하지 않아도 된다.
```c++
class CTest
{
public:
	A(int x, double y) {std::cout << "A 생성자 호출" << std::endl; }
};

A func()
{
	return {1, 2, 3};   // 이는 A(1, 2, 3) 과 동일하다.
}

int main() { func(); }
```

- Initializer_list

모든 STL 을 지원한다. 처음 STL 객체를 생성할 때 push_back() 등의 별도 함수 없이 요소들을 한번에 추가한다.
```c++
int arr[] = {1, 2, 3, 4};
```

아래와 같이 중괄호를 활용하여 위의 배열 정의와 같은 효과를 줄 수 있다.
```c++
class CTest()
{
public:
	CTest(std::initialize_list<int> list)
	{
		for(auto iter = list.begin(); iter != list.end(); ++iter)
		{
			std::cout << *iter << std::endl;
		}
	}
}

int main()
{
	CTest mtest = {1, 2, 3, 4};

	return 0;
}

// 결과
1
2
3
4
```

initializer_list 는 우리가 { } 를 이용해서 생성자를 호출할 때, 클래스의 생성자들 중에 initializer_list 를 인자로 받는 생성자가 있다면 전달된다. (  ' ( ) 소괄호를 사용하여 생성자를 호출하면 inirialize_list 가 생성되지 않는다.')

```c++
template <typename T>
void print_vec(const std::vector<T>& vec)
{
	std::cout << "[";
	for (const auto& e : vec)
	{
		std::cout << e << " ";
	}
	std::cout << "]" << std::endl;
}

template <typename K, typename V>
void print_map(const std::map<K, V>& m)
{
	for (const auto& kv : m)
	{
		std::cout << kv.first << " : " << kv.second << std::endl;
	}
}

int main()
{
	std::vector<int> v = { 1, 2, 3, 4, 5 };
	print_vec(v);

	std::cout << "-----------" << std::endl;
	std::map<std::string, int> m = { {"abc", 1}, {"mic", 3}, {"test", 5}, {"c++", 2}, {"lol", 6} };
	print_map(m);
}

// 결과
[1 2 3 4 5 ]
-----------
abc : 1
c++ : 2
lol : 6
mic : 3
test : 5
```

- initializer_list 의 사용시 주의점

```c++
int main()
{
	std::vector<int> vOne(10);   //...1)
	std::vector<int> vTwo{ 10 };   //...2)

	std::cout << vOne.size() << std::endl;
	std::cout << vTwo.size() << std::endl;
	return 0;
}

// 결과
10
1
```
\- 2 의 경우 그냥 원소 1개 짜리 initializer_list 라고 생각해 10을 보관하고 있는 벡터를 생성한다.
\- 1 의 경우 10개의 요소를 가진 벡터를 생성한다.

- initializer_list 와 auto
{ } 를 이용해서 생성할 때 타입으로 auto 을 지정한다면 initializer_list 객체가 생성된다.
```c++
auto list = {1, 2, 3};
```
위의 코드에서 list 는 initializer_list<\int> 가 된다.

C++17 이후 다음의 형식을 따른다.

1. auto x = {arg1, arg2 ...} 형태의 경우 arg1 과 arg2 와 ... 가 모두 같은 타입이라면 x 는 std::initializer_list<\T> 로 정의된다.
2. auto x { arg1, arg2, ...} 형태의 경우 인자가 1개 라면 해당 인자의 타입으로 추론되고, 여러개일 경우 오류를 일으킨다.

```c++
	auto a = { 1 };   // 1 의 경우로 std::initializer_list<int>
	auto b{ 1 };   // 2 의 경우로 int
	auto c = { 1, 2 };   // 1의 경우로 std::initializer_list<int>
	auto d{ 1, 2 }; // 2의 예시이지만 여러개 이므로 컴파일 오류를 일으킨다.
```

유니폼 초기화와 auto 를 같이 사용할 때 문자열의 경우
```c++
auto list = {"a", "b", "cc"};
```
위의 코드에서 list 는 initializer_list<std::string> 이 아닌 initializer_list<const char*> 이 된다.
해당 내용에 대해 c++ 14 이후에서 추가된 리터럴 연산자를 통해서 해결이 가능하다.
```c++
using namespace std::literals; // 문자열 리터럴 연산자를 사용하기 위해 추가해줘야한다.
auto list = {"a"s, "b"s, "c"s}; // 이와 같이 하면initializer_list<std::string> 으로 추론된다.
```

- 생성자 위임
생성자 안에서 해당 클래스의 다른 생성자를 호출하는 것을 말한다.
생성자 바디에서 다른 생성자를 부를 수 없고 오직 생성자 초기화 리스트를 이용해야 한다.
또한 생성자 초기화 리스트에서 유일한 항목이어야 한다.
이때 생성자 위임 끼리 재귀적으로 꼬리 무는 것을 조심해야 한다.
```c++
class MyClass
{
public:
	MyClass() {}
	MyClass(int num) { mNum = num; }
	MyClass(std::string str)
		: MyClass(3)
		, mNumD(1.5)   // ...1)
	{ mStr = str; }


private:
	int mNum;
	double mNumD;
	std::string mStr;
};
```

1. 여기에서 '위임하는 생성자는 다른 mem-initializer 를 포함할 수 없습니다' 라는 오류가 뜬다.

- 객체의 소멸
객체의 소멸은 다음 의 두 단계를 거친다.
\1. 객체의 소멸자 호출
\2. 할당받은 메모리의 반환 

소멸자에서는 메모리가 반환되기 전에 해야할 작업을 수행한다. 이는 내부 맴버를 위해 할당받은 메모리의 반환, 열어놓은 파일 핸들을 닫는 증의 작업을 진행한다.
소멸자는 생성자 처럼 사용자가 정의하지 않으면 컴파일러가 자동으로 생성한다. 자동으로 생성된 소멸자는 클래스 내 맴버들을 돌아가며 재귀적으로 소멸자를 호출하여 객체가 삭제될 수 있도록 한다.
스택에 생성된 객체와 그 객체의 데이터 맴버는 선언(또는 생성) 순서의 역순으로 삭제된다.

- 객체의 대입
```c++
CSpreadsheetCell myCell(5), anotherCell;
anotherCell = myCell;
```
객체를 다른 객체에 대입할 수 있다.
위의 경우 복제가 일어난다고 생각할 수 있지만 C++ 에서는 복제 생성자만 객체를 복제할 수 있고, 복제 생성자는 사후 대입이 아니니 객체 생성을 위해서만 호출 될 수 있다. 따라서 위의 코드는 복제가 아니라 대입이다.
c++ 에서는 객체 간 대입을 지원하는 대입 연산자라는 별도의 방법을 제공한다.
대입 연산자는 = 연산자를 각 클래스에서 오버로딩하여 구현한다. 위의 예제에서 anotherCell 은 myCell 을 인자로 하여 대입 연산자가 호출 된 것이다.
(이동 대입 연산자와 구별하여 복제 대입 연산자 라고 부르기도 한다. 연산자의 좌변과 우변 객체 모두 연산 실행 후에도 유효하기 때문이다. 이동 대입 연산자의 경우 연산 수행 후 우변의 객체가 성능 최적화 목적으로 소멸한다.)
c++ 언어 차원에서 객체간 대입 연산자를 자동으로 만들며 이것의 기본 동작은 복제 동작과 거의 같다.
객체의 데이터 맴버에 대해 재귀적으로 원본 객체에서 대상 객체로 대입 연산을 수행한다.

\- 대입 연산자 선언
```c++
Class CSpreadsheetCell
{
public:
	...
	CSpreadsheetCell& operator=(const SpreadsheetCell& rhs);
	...
}
```
복제 생성자와 달리 대입 연산자는 CSpreadsheetCell 의 참조 객체를 리턴한다.
이는 대입 연산의 중첩을 가능하게 한다.
```c++
myCell = anotherCell = aThirdCell;
```
1. anotherCell 의 대입 연산자가 aThirdCell 를 우변 인자로 하여 호출된다.
2. myCell 의 대입 연산자는 위의 1단계에서 반환되는 반환값을 우변 항목 인자로 취한다.

- 리턴값으로서의 객체
객체를 리턴할 때 복제가 일어날지 대입이 일어날지 정확히 알 수 없는 때가 있다.
```c++
string CSpreadsheetCell::getString() const
{
	return mString;
}
```
위의 메서드를 아래와 같이 이용할 때
```c++
CSpreadsheetCell myCell(5);
string s1;
s1 = myCell.getString();
```
1. getString() 이 mString 을 반환
2. 컴파일러는 string 의 임시 객체를 생성하면서 string 의 복제 생성자 호출
3. s1 에 리턴 값을 대입할 때 생성된 임시 객체를 우측 인자로 하는 s1 의 대입 연산자 호출
4. 대입연산자의 실행이 끝난 후에 임시 객체 소멸

```c++
CSpreadsheetCell myCell(5);
string s2 = myCell.getString();
```
1. s2는 대입 연산자가 아니라 복사 생성자에 의해 생성된다. 

