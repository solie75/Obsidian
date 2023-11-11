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
3. 각 파라미터의 값의 저장은 클래스가 인스턴스화한 객체이다.
4. 맴버 변수(파라미터) 와는 다르게 메서드의 구현 내용은 모든 객체가 공유한다. 
5. 맴버 간, 메서드 간 또는 맴버와 메서드 간에 이름이 같아서는 안된다.
6. 모든 메서드와 맴버 변수는 public, protected, private 세가지 접근 식별자로 분류한다.
   \- 접근자 지정 이후에 선언되는 모든 메서드와 맴버 변수는 다른 접근자 지정 전까지 해당 접근 식별자를 따른다.
   \- class 의 디폴드 접근 식별자는 private 로 사용자가 접근자 지정을 하지 않으면 자동으로 private 접근 속성이 된다.
   \- public : 제한 없이 접근
   \- private : 메서드나 데이터 멤버를 해당 클래스에서만 접근 가능. 파생 클래스에서의 접근 또한 불가능
   \- protected : 같은 클래스 내 또는 파생클래스에서 접근 가능.
1. struct 도 class 처럼 메서드를 가질 수 있다. struct 가 class 와 다른 점은 디폴드 접근 식별자는 public 이라는 것이다.

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
  1. ' . ' 은 도트 연산자로 객체를 대상으로 메서드를 호출할 때 이용한다.\

\- 힙에 객체를 생성하는 경우
new 연산자를 이용해서 동적으로 객체를 생성한다.


```c++
#include <iostream>
#include <sstream>
#include <string>

#include "CSpreadsheetCell.h"

int main()
{
	CSpreadsheetCell* myCell = new CSpreadsheetCell;
	myCell->setValue(3.7);
	std::
}
```

```c++
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

//CSpreadsheetCell::CSpreadsheetCell()
//{
//}
//
//CSpreadsheetCell::~CSpreadsheetCell()
//{
//}

void printCell(const CSpreadsheetCell& inputCell)
{
    std::cout << inputCell.getString() << std::endl;
}

void CSpreadsheetCell::setValue(double inputValue)
{
    mValue = inputValue;
    mString = doubleToString(mValue);
    printCell(*this);
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


```c++
#pragma once
#include <string>

class CSpreadsheetCell
{
private:
	double mValue;
	std::string mString;

	std::string doubleToString(double inputValue) const;
	double stringToDouble(const std::string& inputString) const;
	
public:
	//CSpreadsheetCell();
	//~CSpreadsheetCell();

	void setValue(double inputValue);
	double getValue() const;

	void setString(const std::string& inputString);
	const std::string& getString() const;
};

```