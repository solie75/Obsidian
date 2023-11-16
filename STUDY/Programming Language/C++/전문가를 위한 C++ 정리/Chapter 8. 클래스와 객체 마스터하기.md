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

