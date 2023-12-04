해당 chapter 에서는 Spreadsheet 클래스를 점진적으로 구형한다. Spreadsheet 는 SpreadsheetCell 의 2차원 배열을 데이터 맴버로 가진다.

```c++
// CSpreadsheet.h
#pragma once
#include "CSpreadsheetCell.h"

class CSpreadsheet
{
private:
	int mWidth, mHeight;
	CSpreadsheetCell** mCells;
public:
	CSpreadsheet(int inputWidth, int inputHeight);
	~CSpreadsheet();
	void setCellAt(int x, int y, const CSpreadsheetCell& cell);
	CSpreadsheetCell& getCellAt(int x, int y);

private: 
	bool inRange(int val, int upper);
};
```

```c++
// CSpreadsheet.cpp
#include "CSpreadsheet.h"
#include <stdexcept>
CSpreadsheet::CSpreadsheet(int inputWidth, int inputHeight)
    : mWidth(inputWidth)
    , mHeight(inputHeight)
{
    mCells = new CSpreadsheetCell*[mWidth];   //...1)
    for (int i = 0; i < mWidth; i++)
    {
        mCells[i] = new CSpreadsheetCell[mHeight];   //...2)
    }
}

CSpreadsheet::~CSpreadsheet()
{
    for (int i = 0; i < mWidth; i++)
    {
        delete[] mCells[i];
    }
    delete[] mCells;
    mCells = nullptr;
}

void CSpreadsheet::setCellAt(int x, int y, const CSpreadsheetCell& cell)
{
    if (!inRange(x, mWidth) || !inRange(y, mHeight))
    {
        throw std::out_of_range("");
    }
    mCells[x][y] = cell;
}

CSpreadsheetCell& CSpreadsheet::getCellAt(int x, int y)
{
    if (!inRange(x, mWidth) || !inRange(y, mHeight))
    {
        throw std::out_of_range("");
    }
    return mCells[x][y];
}

bool CSpreadsheet::inRange(int val, int upper) // 가리키는 x, y 좌표가 유효한 지에 대한 검사
{
    // 아직 구현되지 않음.
}

```

...1 )   mCell 은 'CSpreadsheetCell 을 가리키는 포인터' 를 가리키는 포인터로 이중포인터이다. mCell 에 'CSpreadsheetCell의 포인트 배열의 주소를 가리키는 포인터'를 대입한다.
...2 )  mCell 은 CSpreadsheetCell* 형 배열의 주소를 가리키고 mCell [i] 로  포인터 배열에 접근한다. 접근한 포인터 배열의 요소에 CSpreadsheetCell 형 배열의 주소를 대입한다.


- 복제와 대입의 관리
컴파일러가 자동으로 생성한 메서드는 데이터 맴버에 대해 재귀적으로 복제 생성자 또는 대입 연산자를 호출한다.
하지만, int, double, 포인터와 같은 기본 데이터 타입에 대해서는 복제 생성자나 대입 연산자 대신 '얕은 복제(비트 단위 복제)'가 일어난다. 이는 포인터가 가리키는 데이터는 빼놓고 피상적으로 그 변숫값, 즉 주솟값만 복제하는 것을 말한다.
얕은 복제는 다음의 경우 문제가 된다.
```c++
void printSpreadsheet(CSpreadsheet S)   //...2)
{

}

int main()
{
	CSpreadsheet s1(4, 3);
	printSpreadsheet(s1);   //...1)
	//...3)
	return 0;
}
```
...1 )  CSpreadsheet 은 포인터 변수 mCells 를 가지고 있다. 얕은 복제는 mCells 의 데이터는 냅두고 포인터만 원본에서 대상으로 복제하고 이 때문에 s 와 s1 은 같은 메모리 공간 (데이터) 을 가리키고 있다.
...2 )  s 가 mCells 데이터를 변경하는 것은 즉 s1 의 데이터를 변경하는 것이다. 즉, printSpreadsheet() 함수가 반환하면 스택 객체인 s 의 소멸자가 호출되면서 s 와 s1 이 동시에 가리키는 메모리 공간을 해제해 버린다.
...3 )  s1 은 더이상 유효하지 않다. 이러한 포인터를 '댕글링 포인터 (dangling pointer)' 라고 한다.

대입의 경우도 문제가 된다.
```c++
Spreadsheet s1(2, 2), s2(4, 3);
s1 = s2;
```
s1 에 s2 를 대입하면 s1 은 s2 의 메모리를 가리킨다. 이렇게 되면 s1이 가리키던 원래의 메모리 공간은 그대로인 채로 자신을 가리키는 변수가 아무것도 없는 상황이 된다.
이 문제를 피라기 위해서는 대입 대상인 좌변항의 객체가 참조하고 있는 메모리를 우선 반환한 후에 새로 메모리를 준비하여 깊은 복제(포인터 변수값 뿐 아니라 연관된 데이터 까지 재귀적으로 온전하게 복제하는 것)를 해야 한다.

*** 클래스에서 메모리를 동적으로 할당하는 경우가 있다면 항상 복제 생성자와 대입 연산자를 프로그래머가 직접 구현하여 깊은 복제가 일어나도록 해야 한다.

- Spreadsheet 의 복제 생성자 구현
``` c++
// CSpreadsheet.h
class CSpreadsheet
{
	...
public :
	...
	CSpreadsheet(const CSpreadsheetCell* src);
}

// CSpreadsheet.cpp
CSpreadsheet::CSpreadsheet(const CSpreadsheet& src)
    : mWidth(src.mWidth)
    , mHeight(src.mHeight)
    , mCells(nullptr)
{
    mCells = new CSpreadsheetCell * [mWidth];
    for (int i = 0; i < mWidth; i++)
    {
        mCells[i] = new CSpreadsheetCell[mHeight];
    }
    for (int i = 0; i < mWidth; i++)
    {
        for (int j = 0; j < mHeight; j++)
        {
            mCells[i][j] = src.mCells[i][j];
        }
    }
}
```

- Spreadsheet 의 대입 연산자.
```c++
// CSpreadsheet.h
class CSpreadsheet
{
public:
	CSpreadsheet& CSpreadsheet::operator=(const CSpreadsheet* rhs);
}


// CSpreadsheet.cpp
CSpreadsheet& CSpreadsheet::operator=(const CSpreadsheet* rhs)
{
    if (this == rhs)   //...1)
    {
        return *this;
    }
    for (int i = 0; i < mWidth; i++)
    {
        delete[] mCells[i];   //...2)
    }
    delete[] mCells;
    mCells = nullptr;

    mWidth = rhs->mWidth;
    mHeight = rhs->mHeight;
    mCells = new CSpreadsheetCell * [mWidth];
    for (int i = 0; i < mWidth; i++)
    {
        mCells[i] = new CSpreadsheetCell[mHeight];
    }
    for (int i = 0; i < mWidth; i++)
    {
        for (int j = 0; j < mHeight; j++)
        {
            mCells[i][j] = rhs->mCells[i][j];
        }
    }
    return *this;
}
```
...1 )  대입의 대상이 자기 자신인지를 확인한다. 동적 메모리를 대입 하기 전에 대입 대상의 메모리를 먼저 해제하게 되는데 이때 만약 대입의 대상과 원본이 같다면 대입할 때 이미 원본은 가리키는 메모리가 없기 때문이다.
...2 )  대입에 앞서 대입의 대상이 되는 객체의 메모리를 먼저 해제한다.
위와 같이 해제하지 않고 새로운 메모리를 할당하면 메모리 릭이 발생한다.

*** 따라서 클래스가 동적 할당 메모리를 가진다면 소멸자, 복제 생성자, 대입 연산자를 꼭 구현해야 한다.

- 복제 생성자와 대입 연산자의 공통부분을 편리하게 한다.
```c++
위의 
mWidth = rhs->mWidth;
    mHeight = rhs->mHeight;
    mCells = new CSpreadsheetCell * [mWidth];
    for (int i = 0; i < mWidth; i++)
    {
        mCells[i] = new CSpreadsheetCell[mHeight];
    }
    for (int i = 0; i < mWidth; i++)
    {
        for (int j = 0; j < mHeight; j++)
        {
            mCells[i][j] = rhs->mCells[i][j];
        }
    }
이 부분이 복제 생성자와 대입 연산자에 공통되므로 다른 함수로 빼내어 관리한다.

```


## 클래스와 여러 데이터 맴버

- static 데이터 멤버

데이터 맴버를 static 으로 선언하여 객체가 아닌 class 에 종속되도록 한다. 이는 해당 class 로 선언된 모든 객체가 하나의 static 변수에 접근하는 것을 의미한다. 전역변수와는 다르게 특정 클래스에 종속된다.
static 데이터 맴버는 별도의 소스 파일(주로 메서드의 구현부가 있는 파일) 안에서 다시 한번 선언해야 하고 초기화도 이때 해야 한다.

```c++
// CTest.h
Class CTest
{
public :
	static int mStaticInt;   //...2)
}

// CTest.h
int CTest::mStaticInt = 20;   //...1)

// main
int main()
{
	std::cout << CTest::mStaticInt << std::endl;   //...3)
}

// 결과
20
```

...1 )  static 멤버 변수 선언 부분.  "int CTest::mStaticInt; " 였다면 결과는 0 이다. 즉, 선언만 하고 초기화를 하지 않으면 0으로 자동으로 초기화 된다.
...2 )  되도록 이면 private 로 static 멤버 변수를 선언하고 게터와 세터를 사용하여 접근하는 것이 좋다.  private 로 선언했다면 ...3 ) 에서 엑세스 할 수 없다는 오류가 뜬다.

- 클래스 메서드 내에서의 static 데이터 멤버 접근
메서드 안에서는 static 데이터 멤버를 일반 데이터 멤버와 같은 방식으로 이용할 수 있다.
```c++
// CSpreadsheet.h
class CSpreadsheet
{
public:
	...
	int getIn() const;
private:
	...
	static int sCounter;
	int mId;
}

// CSpreadsheet.cpp

CSpreadsheet::CSpreadsheet(int inputWidth, int inputHeight)
    : mWidth(inputWidth)
    , mHeight(inputHeight)
{
    mId = sCounter++;   //...1) 생성자
    ...
}

CSpreadsheet::CSpreadsheet(const CSpreadsheet& src)
    : mWidth(src.mWidth)
    , mHeight(src.mHeight)
    , mCells(nullptr)
{
    mId = sCounter++;   //...2) 생성자

   ...
}
```

- 클래스 메서드 밖에서의 static 데이터 멤버 접근
public 으로 선언한다면 클래스 바깥에서도 접근 가능. 하지만 public 으로 하지 않고 private 으로 선언한 다음 게터와 세터로 접근하도록 한다. 이때 게터와 세터 메서드를 구현하는데 static 메서드를 사용해야 한다.

- const 데이터 멤버
클래스의 데이터 멤버를 const 선언하면 생성 시점에서 초깃값을 부여한 뒤로 더는 값을 변경할 수 없게 된다. 이처럼 객체 수준에서 상수값을 보유하는 것은 대부분 메모리 낭비이기에 static const 맴버를 이용해서 개체 간 상수 값을 공유하도록 한다.

```c++
// CSpreadsheet의 최대값을 지정한다.
class CSpreadsheet
{
public :
	static const int kMaxHeight = 100;
	static const int kMaxWidth = 100;
...
}
```

```c++
// CSpreadsheet 생성자에서 최대값을 넘지 않도록 한다.
CSpreadsheet::CSpreadsheet(int inputWidth, int inputHeight)
    : mWidth(inputWidth < kMaxWidth ? inputWidth : kMaxWidth)
    , mHeight(inputHeight < kMaxHeight ? inputHeight : kMaxHeight)
{
    ...
}
```

- 참조형 데이터 멤버
spread sheet 프로그램의 작동을 위해 Spreadsheet 과 SpreadsheetCell를  모두 관리하는 CSpreadsheetApplication 클래스를 생성한다. 이때 CSpreadsheet 과 CSpreadsheetCell 은 CSpreadsheetApplication을 CSpreadsheetApplication 은 CSpreadsheet과 CSpreadsheetCell 을 참조해야한다. 즉, 클래스끼리 서로 참조를 해야하는 상황이 생길 수 있다는 것이다. 이와 같은 경우를 전방선언 ('포워드 선언') 으로 해결한다. 교차 참조되는 클래스의 헤더 파일 중 어느 한쪽에 상대편 클래스를 전방 선언하면 컴파일러가 나중에 해당 정의를 찾아서 타입 매칭을 한다.

```c++
// CSpreadsheet.h
Class CSpreadsheetApplication; // 전방 선언
class CSpreadsheet
{
public:
	Spreadsheet(int inWidth, int inHeight, SpreadsheetApplication& theApp);
	...
private:
	...
	CSpreadsheetApplication& mTheApp;	
}

// CSpreadsheet.cpp
CSpreadsheet::CSpreadsheet(int inputWidth, int inputHeight, CSpreadsheetApplication& theApp)
    : mWidth(inputWidth < kMaxWidth ? inputWidth : kMaxWidth)
    , mHeight(inputHeight < kMaxHeight ? inputHeight : kMaxHeight)
    , mTheApp(theApp)
{
    mId = sCounter++;
    mCells = new CSpreadsheetCell * [mWidth];
    for (int i = 0; i < mWidth; i++)
    {
        mCells[i] = new CSpreadsheetCell[mHeight];
    }
}

CSpreadsheet::CSpreadsheet(const CSpreadsheet& src)
    : mWidth(src.mWidth)
    , mHeight(src.mHeight)
    , mCells(nullptr)
    , mTheApp(src.mTheApp)
{
    mId = sCounter++;

    mCells = new CSpreadsheetCell * [mWidth];
    for (int i = 0; i < mWidth; i++)
    {
        mCells[i] = new CSpreadsheetCell[mHeight];
    }
    for (int i = 0; i < mWidth; i++)
    {
        for (int j = 0; j < mHeight; j++)
        {
            mCells[i][j] = src.mCells[i][j];
        }
    }
}
```

참조형 변수는 생성과 동시에 다른 객체를 참조하도록 초기화되어야 하므로 객체가 생성되고 나서 나중에 세팅할 수 없다. 따라서 생성자 초기화 리스트에서 참조형 변수를 초기화 하며 이는 복사 생성자에서도 마찬가지이다.

- const 참조형 데이터 멤버
참조형 데이터가 const 객체를 참조하기 위해서는 해당 변수를 const 로 선언하면 된다.
```c++
class CSpreadsheet
{
private :
	const CSpreadsheetApplicaion& mTheApp;

public: CSpreadsheet(int inputWidth, int inputHeight, const CSpreadsheetApplication.theApp)
}
```

const 참조현 변수로 참조된 CSpreadsheetApplication 데이터 멤버는 CSpreadsheetApplication 객체에 대해 const 메서드만 호출할 수 있다.

## 메서드 종류

- static
메서드가 특정 객체에 종속되는 부분 없이 모든 객체에 대해 공통적으로 적용되어야 할 때에 사용된다. 
```c++
class SpreadsheetCell
{
	...
	private:
		static std::string doubleToString(double val);
		static double stringToDouble(const std::string& str);
	...
}
```

static 멤버 변수와 같이 클래스 만으로도 접근이 가능하다.
객체와 독립적으로 객체의 생성과 상관이 없다. 객체가 생성되어야 지만 할당되는 기본 맴버 변수를 사용할 수 없다. 하지만 객체 생성과 상관없는 static 멤버 변수는 사용이 가능하다.
객체 생성과 상관이 없으므로 당연히 this 포인터 또한 사용할 수 없다.

 - const
```c++
// CSpreadsheetCell.h
class CSpreadsheetCell
{
	public:
	...
	double getValue() const;
	const std::string& getString() const;
}

// CSpreadsheetCell.cpp
double CSpreadsheetCell::getValue() const   //...1)
{
    return mValue;
}

const std::string& CSpreadsheetCell::getString() const   //...1)
{
    return mString;
}
```
메서드를 const로 선언하는 것은 메서드가 객체의 데이터 값을 바꾸지 않는다는 뜻이다.
const 메서드 내에서 접근하는 모든 데이터 멤버를 const 로 취급하는 것으로 구현한다.
...1 )  const 제한자는 메서드 프로토타입 선언에 포함되기 때문에 구현부에서도 똑같이 적용해야 한다.
static 메서드는 객체에 연계되지 않으므로 const 선언이 무의미하다. 따라서 static 메서드에는 const 제한자를 적용할 수 없다.

```c++
int main()
{
	CSpreadsheetCell myCell(5);
	std::cout << myCell.getValue() << std::endl;

	myCell.setString("6");
	const CSpreadsheetCell& anotherCell = myCell;

	std::cout << anotherCell.getValue() << std::endl;
	anotherCell.setString("6");   //...1)

	return 0;
}
```

...1 )  에서 '객체에 멤버 함수 "CSpreadsheetCell::setString" 과(와) 호환되지 않는 형식 한정자가 있습니다. 개체 형식 : const CSpreadsheetCell ' 오류가 뜬다.
const 객체에 대해서는 const 객체만 호출 가능하다. 가능하다면 객체를 변경하지 않는 모든 메서드에 const 제한자를 적용하여 const 객체에서도 호출할 수 있게 하는 것이 바람직하다.

- mutable
mutable 로 선언된 멤버 변수는 const 함수에서도 값을 변경할 수 있다.
```c++
// CSpreadsheetCell.h
class CSpreadsheetCell
{
private:
	mutable std::string mString;
	...
public:
	...
}

// CSpreadsheetCell.cpp
const std::string& CSpreadsheetCell::getString() const
{
    std::string str;
    mString = str;   //...1)
    return mString;
}
```
...1 )  에서 메서드가 const 이기 때문에 원래는 맴버 변수의 값의 변경이 불가능 하지만 mString 맴버 변수가 mutable 선언되었기 때문에 값의 변경이 가능하다.

- 메서드 오버로딩
이름이 같은 메서드 또는 함수를 파라미터만 달리하여 정의하는 것을 말한다.
```c++
class CTest()
{
private:
	mNum;
public:
	printNum(int input);
	printNum(int input1, std::string input2);
}

CTest::printNum(int input)
{
	return input;
}

CTest::printNum(int input1, std::string input2)
{
	std::cout << input2 << std::endl;
	return input1;
}
```
컴파일러가 printNum() 메서드의 호출을 만나면 인자의 데이터 타입과 개수를 보고 적합한 메서드로 매핑 한다. 이러한 매핑 메커니즘을 오버로드 지정 이라고 한다.
리턴타입만 다른 메서드 혹은 함수에 대해서는 오버로딩을 허용하지 않는다.
const 제한자에 기반한 오버로딩도 가능하다. 예로 두 개의 이름도 같고 파라미터도 같은 메서드 중 어느 한쪽이 const 라면, 그 메서드를 호출한 객체의 타입이 const 인지 아닌지 에 맞춰서 컴파일러가 호출할 메서드를 지정할 수 있다.

```c++
class CTest()
{
public:
	void testFnc(int i);
}

int main()
{
	CTest test;
	test.testFnc(3);
	test.testFnc(1.1);   //...1)
}
```
... 1 )  에서 1.1 을 1 로 타입 캐스팅하여 testFnc(int i) 를 호출한다. double 자료형을 입력했을 때 testFnc(int i) 가 호출되지 않게 하기 위해서는 다음을 추가한다.
```c++
class CTest()
{
public:
	void testFnc(int i);
	void testFnc(double d) = delete;   //...1)
}
```
...1) 과 같이 선언하면 double 타입 인자로 testFnc() 메서드가 호출될 때 컴파일러가 int 로 타입캐스팅을 하는 대신 에러 메시지를 출력한다.

- 디폴트 파라미터
함수나 메서드의 프로토 타입을 선언 할 때 파라미터에 디폴트 값을 지정할 수 있다. 사용자가 함수나 메서드에 인자를 전달하면 디폴트값이 아닌 전달한 값이 적용 된다.
디폴트 파라미터는 가장 마지막(가장 오른쪽)의 파라미터부터 시작해서 건너뜀 없이 연속으로 적용할 수 있다.
```c++
class CTest
{
public:
	void testFnc(int numI = 10, double numD, float numF = 0.3f)   //...1)
	{}
};
```
...1 )  디폴트 파라미터가 오른쪽 부터 건너뜀 없이 적용되어야 하는데 double numD 에는 디폴트 파라미터가 적용되지 않았는데 int numI = 10 은 디폴트 파라미터가 적용되어 있다. 이때 int numI = 10 에 '기본 인수가 매개 변수 목록의 끝에 없습니다.' 오류가 뜬다.

- inline
메서드나 함수를 별도의 분리된 코드 블록으로 호출하는 대신, 호출 지점에 바로 메서드나 함수의 바디를 옮겨놓아 호출 오버헤드를 줄이는 방법을 인라이닝(inlining)이라하고 인라이닝되는 메서드 또는 함수를 inline 메서드, inline 함수 라 한다.
인라이닝은 \#define 매크로를 이용해서 코드를 삽입하는 방법보다 훨신 안전하다.
```c++
// CSpreadsheetCell.cpp
inline double CSpreadsheetCell::getValue() const
{
    return mValue;
}

inline const std::string& CSpreadsheetCell::getString() const
{
    return mString;
}
```
위의 예제는 getValue() 와 getString() 메서드를 inline 화 한것이다.
컴파일러가 위의 두 메서드의 호출을 만날 때마다 메서드 호출 코드 대신 메서드 바디를 해당 위치에 삽입한다.
이를 위해서는 해당 메서드와 함수의 바디가 inline 할 소스 파일에서 보여야 한다.
따라서 inline 함수는 헤더파일에 해당 프로토 타입과 함께 정의부가 들어간다. 예를 들어 클래스 메서드라면 클래스 정의가 있듣 .h 파일에 정의되어야 한다.

```c++
// CSpreadsheetCell.h
class CSpreadsheetCell
{
public:
	double getValue() const { return mValue; };
	const std::string& getString() const { return mString; }
}
```
위의 예제는 클래스 정의 안에 구현부를 정의한 것으로 인라이닝이 적용된다. 
디버거를 이용해서 라인 단위로 디버깅을 할 때 만약 함수가 인라이닝 되어 있다면 인라이닝 된 함수로 점프한다.

- friend 속성
friend 설정을 이용하면 다른 클래스 또는 다른 클래스의 메서드에서 private나 protected 로 선언된 멤버와 메서드에 접근할 수 있게 열어줄 수 있다.
```c++
class CTestOne
{
private:
	int mNum;
public:
	friend class CTestTwo;
};

class CTestTwo
{
private:
	CTestOne t1;
public:
	int getNum() { return t1.mNum; }
};
```

class 전체가 아니라 하나의 메서드 혹은 함수에도 friend 선언이 가능하다.
```c++
class CTestOne;

class CTestTwo
{
public:
    void printNum(CTestOne& t);
};

void CTestTwo::printNum(CTestOne& t)
{
    std::cout << t.num << std::endl;
}

class CTestOne
{
private:
    int num;
public:
    friend void CTestTwo::printNum(CTestOne&);
};
```

- 연산자 오버로딩
```c++
// CSpreadsheetCell.h
class CSpreadsheetCell
{
public:
	CSpreadsheetCell add(const CSpreadsheetCell& cell) const;
	CSpreadsheetCell operator+ (const CSpreadsheetCell& cell) const;
}

// CSpreadsheetCell.cpp
CSpreadsheetCell CSpreadsheetCell::add(const CSpreadsheetCell& cell) const
{
    CSpreadsheetCell newCell;
    newCell.setValue(mValue + cell.mValue);
    return newCell;
}

CSpreadsheetCell CSpreadsheetCell::operator+(const CSpreadsheetCell& cell) const
{
    CSpreadsheetCell newCell;
    newCell.setValue(mValue + cell.mValue);
    return newCell;
}

// main
int main()
{
	CSpreadsheetCell cellOne(4), cellTwo(9);

	CSpreadsheetCell cellThree = cellOne.add(cellTwo);   //...1)
	CSpreadsheetCell cellFour = cellOne + cellTwo;   //...2)

	return 0;
}
```

1과 2 모두 결과가 같다. 상황에 따라 알맞은 방식으로 사용한다.

- 묵시적인 타입 캐스팅
```c++
int main()
{
	CSpreadsheetCell cellOne(4), cellTwo(9);

	CSpreadsheetCell cellThree = cellOne.add(cellTwo);
	CSpreadsheetCell cellFour = cellOne + cellTwo;
	CSpreadsheetCell cellFive = cellOne + 4;   //...1)
	CSpreadsheetCell cellsix = cellOne + 5.6;   //...2)
	std::string str = "game";
	CSpreadsheetCell cellsix = cellOne + str;
	return 0;
}
```

위의 예제는 오류없이 컴파일 된다. 이는 operator+ 를 컴파일러가 처리할 때 적합한 타입을 찾을 뿐아니라 적합한 타입으로 변환할 방법도 찾기 때문이다. 
만약 생성자 중에 부적합한 타입을 인자로 받는 것이 있다면 그 생성자를 이용해서 적합한 타입의 임시 객체를 자동으로 생성한다.

풀어서 설명하면
...1 ) 의 ' 4 ' 는 CSpreadsheetCell 임시객체(4) 가 된다. 즉, cellFive 는 cellOne 과 임시객체(4) 에 대한 operator+ 연산의 반환값이 대입된다.
...2 ) 의 ' 5.6 ' 은 CSpreadsheetCell 임시객체(5.6) 이 된다. 하지만 이때 인자를 int 또는 string 으로 하는 생성자와 디폴트 생성자만 있을 뿐 double 에 대한 생성자는 존재하지 않는다. 따라서 컴파일러는 5.6 에 묵시적인 타입 캐스팅을 적용하여 int 형 ' 5 ' 로 변환 하고 CSpreadsheetCell(int inputInt) 생성자를 호출한다.

이러한 암묵적인 변환을 막고 싶다면 해당 생성자에 explicit 키워드를 붙인다.
' explicit CSpreadsheetCell(int inputInt);  ' 와 같이 생성자를 생성하면 위의 예제 코드에서 ...1 ) 의 + 와 ...2 ) 의 + 에 오류가 생긴다.
explicit 키워드는 클래스 정의에서만 사용이 가능하고 파라미터가 하나뿐인 생성자에서만 의미가 있다.

묵시적 생성자가 실행되어 임시 객체가 생성되었다가 사라지는 것은 비효율 적으로 explicit 로 금지하고 operator+ 메서드를 의도에 맞게 오버로딩하는 것이 옳다.

- 전역함수 operator+
```c++
	CSpreadsheetCell cellFive = cellOne + 4;
	CSpreadsheetCell cellSix = cellOne + 5.6;
	CSpreadsheetCell cellSeven = 4 + cellOne;   //...1)
	CSpreadsheetCell cellEight = 5.6 + cellOne;   //...2)
```

묵시적인 변환은 CSpreadsheetCell 객체에 int 와 double 을 더할 수 있게 해주었지만 ...1) 과 ...2) 의 경우 오류를 일으킨다. 즉, 교환법칙이 성립하지 않는다.
이는 operator+ 를 CSpreadsheetCell 의 메서드로서 오버로딩하면 묵시적 타입 변환의 대상이 우항에 있는 객체만을 대상으로 하기 때문이다. 이는 다음의 코드와 같이 operator+ 를 전역 함수로 정의하여 특정 객체에 종속되지 않게 하면 위의 오류가 해결된다.

```c++
class CSpreadsheetCell
{
private:
	double mValue;
	...
public:
	...
	friend CSpreadsheetCell operator+(const CSpreadsheetCell& lhs, const CSpreadsheetCell& rhs);   //...1)
}

CSpreadsheetCell operator+ (const CSpreadsheetCell& lhs, const CSpreadsheetCell& rhs)
{
	CSpreadsheetCell newCell;
	newCell.setValue(lhs.mValue + rhs.mValue);
	return newCell;
}
```
...1 )  operator+ 에서 private 인 mValue 에 접근하기 위해 operator+ 를 CSpreadsheetCell 에서 friend 선언한다.

=> operator -, \*, /   또란 구현해 보자
operatoir/ 를 구현할 대 0으로 나누지 않도록 조심하자

=> C++ 에서는 연산자의 우선순위를 바꿀 수 없다. 프로그래머는 각 연산자의 실제 구현 부분만 커스터마이즈 할 수 있다. 또한 C++ 은 새로운 연산자 기호를 만드는 것을 허용하지 않는다.

이 방법과 같이 축약형 산술 연산자, 비교 연산자 를 오버로딩 할 수 있다.
=> 비교 연산자 오버로딩에서 부동 소수점 변수 간에 동일 값인지에 대한 테스트는 오버헤드가 크다. 이 때에는 ' 엡실론 값 비교를 한다.'

- pimpl 를 구현할 것인가
pimpl 은 컴파일 시간을 줄이는 데 가장 의의를 두는데 현대 c++ 의 컴파일러는 이미 컴파일 시간이 많이 줄어들었기 때문에 사용에 의문점이 있다. 20 버전의 서적을 보고 판단할 것.

