- 상속 개념
```c++
class CParent
{
public:
	CParent();
	~CParent();
}

class CChild : public CParent 
{
public:
	CChild();
	~CChild();
}
```

클래스를 사용하는 입장에서 CChild 클래스의 객체는 CChild 타입 임과 동시에 CParent 타입이기도 하다.  이것의 의미는 CChild 객체를 통해 CChild의 public 멤버와 메서드 뿐 아니라 CParent의 public 멤버와 메서드로 이용할 수 있다는 말이다.

상속은 단방향으로만 작용한다. 즉, CChild 클래스 입장으로서는 자신이 어떤 클래스를 상속 받았는지 알고 있지만 CParent 클래스 입장에서는 자신을 상속받은 클래스가 무엇인지 알 수 없다는 것이다. => CParent 클래스에서 CChild 클래스에 접근이 불가능하다.

포인터 또는 참조형으로 객체를 참조할 때는 객체의 클래스는 물론, 부모 클래스 중 어떤 클래스 타입으로도 변수를 선언할 수 있다.
CChild 객체를 CParent 타입의 포인터로 참조할 수 있다는 것이다.

다음의 예제는 정상적으로 컴파일된다.
```c++
class CParent
{};

class CChild : public CParent
{};

int main()
{
	CParent* test = new CChild;
	return 0;
}
```
CParent 타입의 포인터 또는 참조형 변수로는 CParent 의 멤버와 메서드만 이용가능하다. 객체 test 는 CChild 의 멤버나 메서드를 사용할 수 없다.

- 상속 방지
c++ 에서는 final 키워드를 통해서 파생 클라스로 확장이 불가능한 클래스를 정의한다.
```c++
class CParent final
{}
```
CParent를 상속 받을 수 없다.

- 메서드 오버라이딩
상속 받으면서 부모 클래스에 정의 되어 있는 메서드의 내용을 바꾸는 것을 말한다.

\ - virtual 속성
메서드 오버라이딩이 동작하려면 부모 클래스에서 해당 메서드가 virtual 로 선언되어 있어야한다. 이는 프로토타입 선언 때만 필요하고 구현부에서는 사용하지 않는다.

\- 메서드 오버라이딩 문법
메서드를 오버라이딩할 때는 부모 클래스의 메서드를 파생 클래스에서 똑같이 선언하고 파생 클래스의 구현부에서 해당 메서드를 새롭게 정의한다.

파생 클래스에서 오버라이딩할 메서드를 선언해 주어야 한다.
```c++
// CParent.h
class CParent
{
private:
	std::string mName;
public:
	virtual void printName();
	std::string getName();
};

// CParent.cpp
void CParent::printName()
{
	std::cout << "Parent Name : " << mName << std::endl;
}

std::string CParent::getName()
{
	return mName;
}

// CChild.h
class CChild : public CParent
{
public:
	virtual void printName() override;   //...1)
};

// CChild.cpp
#include "CChild.h"

void CChild::printName()
{
	std::cout << "Child Name : " << getName() << "junior" << std::endl;
}
```

...1 )  에서는 CParent의 printName() 메서드를 오버라이딩 하는 메서드 선언을 위해 뒤에 override 를 붙여준다.
메서드나 소멸자가 단 한 번이라도 virtual 로 서언되고 나면 파생 클라스에서 오버라이딩할 때 virtual 사용 여부와 관계없이 무조건 virtual 이 적용된다.

포인터나 참조를 통해서 클래스나 그 클래스의 파생 클래스를 가리킬 수 있다. 이때 포인터나 참조가 어떤 타입인지와 상관없이 생성된 클래스(생성자가 호출된 클래스) 를 기준으로 오버라이딩된 메서드를 찾아서 호출한다.
하지만 포인터나 참조가 부모 클래스 타입이면 생성자가 호출된 클래스가 파생 클래스라 하더라도 파생클래스에만 정의된 멤버나 메서드에는 접근이 불가능하다.

- 생성자
1. 클래스가 부모 클래스를 갖는다면 부모 클래스의 디폴트 생성자가 실행된다. (단, 파생 클래스의 생성자 초기화 리스트에서 명시적으로 부모 클래스의 특정 생성자가 호출된다면 해당 생성자가 호출된다.)
2. static 이 아닌 클래스 맴버들이 선언 순서에 따라 생성된다.
3. 클래스의 생성자 바디가 실행된다. 
```c++
class CFirst
{
public:
	CFirst() { std::cout << "first" << std::endl; }
	~CFirst() {}
};

class CSecond : public CFirst
{
private:
	int num;
public:
	CSecond(int i)
		: num(i)
	{
		std::cout << "second : " << num << std::endl;
	}
	CSecond() { std::cout << "second" << std::endl; }
	~CSecond() {}
};

class CThird : public CSecond
{
public:
	CThird()
		: CSecond(100)   //...1 )
	{
		std::cout << "thrid" << std::endl;
	}
	~CThird() {}
};

int main()
{
	CThird t;
	return 0;
}

// 결과
first
second : 100
thrid
```
...1 )  생성자 초기화 리스트에서 부모 클래스 생성자를 직접 지정할 수 있다. 직접 지정하지 않으면 디폴드 생성자를 호출한다. => 디폴트 생성자가 없는 상황에서 지정해 주지 않으면 컴파일 오류를 일으킨다.

```c++
class CFirst
{
public:
	CFirst(int i) { std::cout << "first : " << i << std::endl; }
	CFirst() { std::cout << "first" << std::endl; }
	~CFirst() {}
};

class CSecond : public CFirst
{
private:
	int num;
public:
	CSecond(int i)
		: num(i)
		, CFirst(num)   //...1)
	{
		std::cout << "second : " << num << std::endl;
	}
	CSecond() { std::cout << "second" << std::endl; }
	~CSecond() {}
};

class CThird : public CSecond
{
public:
	CThird()
		: CSecond(100)
	{
		std::cout << "thrid" << std::endl;
	}
	~CThird() {}
};

int main()
{
	CThird t;
	return 0;
}

// 결과
first : -858993460
second : 100
thrid
```
...1 )  과 같이 파생 클래스의 데이터 맴버를 부모 클래스의 생성자  파라미터로 넘기는 것은 제대로 작동하지 않는다. 코드는 컴파일 되지만 멤버의 초기화는 부모 클래스가 초기화된 다음 이기 때문에 실제 어떤 값이 넘겨질지 보증되지 않기 때문이다.

- 소멸자
1. 클래스의 소멸자 바디 호출
2. 클래스의 데이터 멤버를 생성 순서와 반대순으로 삭제
3. 부모 클래스가 있다면 소멸자를 호출
```c++
class CFirst
{
public:
	CFirst(int i) { std::cout << "first : " << i << std::endl; }
	CFirst() { std::cout << "first" << std::endl; }
	virtual ~CFirst() { std::cout << "first delete" << std::endl; }
};

class CSecond : public CFirst
{
private:
	int num;
public:
	CSecond(int i)
		: num(i)
		, CFirst(num)
	{
		std::cout << "second : " << num << std::endl;
	}
	CSecond() { std::cout << "second" << std::endl; }
	virtual ~CSecond() { std::cout << "second delete" << std::endl; }
};

class CThird : public CSecond
{
public:
	CThird()
		: CSecond(100)
	{
		std::cout << "thrid" << std::endl;
	}
	virtual ~CThird() { std::cout << "third delete" << std::endl; }
};

int main()
{
	CThird t;
	std::cout << std::endl;
	return 0;
}

// 결과
first : -858993460
second : 100
thrid

third delete
second delete
first delete
```

*** 생성자와 소멸자 모두 파생 클래스에서 부모 클래스의 virtual 메서드를 오버라이딩하고 있더라도 부모 클래스의 생성자 안에서는 부모 클래스의 메서드가 호출된다.

```c++
class CFirst
{
public:
	CFirst() {}
	~CFirst() {}

	virtual std::string getString() const
	{
		return "first";
	}
};

class CSecond : public CFirst
{
public:
	CSecond() {}
	~CSecond() {}

	std::string getString() const override
	{
		return getString();   //...1)
		return CFirst::getString();   //...2)
	};
};


int main()
{
	CSecond s;
	std::cout << s.getString() << std::endl;
	return 0;
}

// 결과
...1) 의 경우 버그를 일으킨다.
...2) 의 경우 'first' 를 출력한다.
```
c++ 에서는 먼저 현재의 스코프, 그 다음으로 클래스 스코프에서 메서드 이름을 찾는다.

- 업캐스팅, 다운캐스팅
부모 클래스로 타입 캐스팅하는 것을 업캐스팅 이라고 한다. 객체는 부모 클래스로 업캐스팅 되거나 부모 클래스의 객체에 대입할 수 있다.
만약 다음처럼 객체를 대상으로 캐스팅이나 대입이 일어나면 자식 클래스의 특징이 사라지는 슬라이싱이 발생한다.
```c++
CSecond mySecond
CFirst myFirst = mySecond; // 슬라이싱 발생 
```
하지만 다음과 같이 참조형 변수의 초기화 방식으로 업캐스팅을 한다면 슬라이싱이 발생하지 않는다.
```c++
CSecond mySecond;
CFirst& myFirst = mySecond;
```

파생 클래스로 타입 캐스팅 하는 것을 다운 캐스팅 이라한다. 다운 캐스팅은 컴파일 타입에 검증이 안 되기 때문에 전문 개발자는 아예사용하지 않는다. 꼭 써야 한다면 dynamic_cast<> 를 이용한다. dynamic_cast 는 객체에 저장된 정보를 이용하여 해당 캐스팅이 적합한지 런타입에 검사하여 문제가 있으면 캐스팅을 거부하고 에러를 발생시킨다.
캐스팅 실패가 포인터 변수에 대해 발생하면 포인터 값이 nullptr 이 된다. (참조형 변수에 캐스팅이 실패했다면 std::bad_cast 익셉션이 발생한다.)

- 순수 가상 함수
여러 파생 클라스가 공통으로 필요로 하는 기능을 그 부모 클래스에서 메서드로 구현한다. 이때 해당 메서드가 각 파생 클라스의 데이터 멤버를 가져오거나 변환하는 것이라면 그 부모 클래스에서는데이터 멤버와 관련되지 못하는 상황에서 기능을 선언 및 구현해야 한다. 이때 사용하는 것이 순수 가상 함수이다.
순수 가상 함수는 클래스를 정의할 때 선언만 하고 구현부 정의는 하지 않는 메서드를 말한다. 순수 가상 함수로 선언된 메서드에 대해 컴파일러는 해당 메서드가 선언된 클래스에서 메서드 구현부를 찾지 않는다.  따라서 순수 가상 함수를 가진 클래스는 인스턴스화 할 수 없기 때문에 추상 클래스라고 불린다.
순수 가상 함수는 메서드 선언 뒤에 ' =0 ' 을 붙인다. 
```c++
class CTest
{
private:
	int num = 10;
public:
	CTest(){};
	~CTest(){};
	void printNum() = 0;
}

int main()
{
	CTest t1;   //...1)
	CTest* t2 = nullptr;   //...2)
}
```
...1 )  추상 클래스의 인스턴스를 생성하려 했기 때문에 컴파일 에러를 일으킨다.
...2 )  나중에 파생 클래스를 인스턴스화 하면서 초기화되기 때문에 문제가 없다.

추상 클래스의 파생 클래스는 부모 클래스의 모든 순수 가상 함수를 빠짐없이 구현해야 한다.
하나라도 구현 안한 순수 가상 함수가 남아 있다면 파생 클래스 또한 추상 클래스가 된다.

 - 다중 상속
여러 클래스로부터 하나의 파생 클래스를 정의하는 것
```c++
// 다중 상속 선언
class CChild : public CParent1, public CParent
{
}
```
위의 예제에서 정의된 CChile 클래스로 생성된 객체는 다음의 특성을 갖는다.
1. CChild 객체는 CParent1 과 CParent2 의 모든 public 멤버와 메서드를 지원한다.
2. CChild 객체는 메서드 구현부에서 CParent1 과 CParent2 의 protected 멤버와 메서드에 접근할 수 있다.
3. CChild 객체는 CParent1 또는 CParent2 로 업캐스팅 될 수 있다.
4. 새로운 CChiled 객체를 생성할 때 자동으로 CParent1과 CParent2의 디폴트 생성자를 호출한다. 이때 호출 순서는 상속 목록에 나열된 순서를 따른다.
5. CChiled 객체를 삭제하면 CParent1 과 CParent2 소멸자가 생성자 호출의 역순으로, 즉 상속 목록에 나열된 순서의 반대 순서로 호출된다.

\- CParent1 과 CParent2 에 똑같은 메서드 가 있을 때, 해당 메서드를 호출하지 않는다면 문제가 발생하지 않지만 CChild 에서 해당 메서드를 호출할 경우 CParent1 의 메서드인지 CParent2의 메서드인지 컴파일러가 판단할 수 없어 에러를 일으킨다. -> 이는 스코프 지정 연산자를 활용한다. ( 또는 업스캐일을 활용하는 방법도 있다.)

\- 또한 CParent1 과 CParent2 가 공통된 부모 클래스( ex CGrandParent)를 갖을 때, CChild 에서 CGrandParent 의 메서드를 호출하지 못한다. CParent1 의 부모 클래스에서 호출할지 CParent2 의 부모 클래스에서 호출 해야할지 컴파일러가 판단할 수 없기 때문이다. 그래서 이런 경우 CGrandParent 를 모든 메서드가 순수 가상 함수로 이루어진 추상 클래스로 만드는 것이 최선이다.

- 메서드의 특성 변경

1. 리턴 타입 변경
원본 메서드의 리턴 타입이 어떤 클래스에 대한 포인터나 참조형이라면 오버라이딩하는 메서드의 리턴 타입을 원본 리턴 타입 클래스의 파생 클래스에 한하여 바꿀 수 있다.
이런 타입을 공변 리턴 타입 이라 한다. (covariant return type)

이는 어떤 클래스 계층이 완전 별개의 다른 클래스 계층에 작업상 연관된 경우에 편리하게 이용된다.

```c++
class CTestA
{};

class CTestB : public CTestA
{
public:
	CTestA* getBaseClassType()
	{ return new CTestA; }
};
```
CTestB 를 CTestA 의 포인터로 리턴할 때 자동으로 업캐스팅 된다.
이때 getBaseClassType 의 리턴값이 void* 와 같은 전혀 관계없는 타입으로 바꿀 수 없다.

2. 파라미터 변경 불가
부모 클래스의 virtual 메서드를 파생 클래스에서 같은 함수명에 같은 리턴 타입으로 선언하더라도 파라미터가 다르면 오버로딩 되지 않는다. 이는 새로운 메서드를 선언하는 것이라 컴파일에는 문제가 없지만 오버로딩하고자 하는 의도에 맞지 않다.

3. override 키워드
```c++
class CTestA
{
public:
	CTestA() {};
	virtual void someMethod(int i) { std::cout << "A" << std::endl; }   //...1)
};

class CTestB : public CTestA
{
public:
	CTestB() {};
	virtual void someMethod(double i) { std::cout << "B" << std::endl; }  //...2)
	
};

int main()
{
	CTestB testB;
	CTestA& testA = testB;
	testA.someMethod(3.4);
	return 0;
}

// 결과
A
```
위의 코드는 ...2 ) 를 호출하고 싶었지만 ...1) 이 호출되었다. 이는 리턴 타입과 메서드 이름이 같더라도 매개변수가 다르다면 오버로딩이 되지 않기 때문이다. 따라서 이러한 실수를 막기 위해서 ...2) 에 override 키워드를 사용한다.
override 키워드를 사용하면 해당 메서드가 override 가 될 수 없는 메서드 일 때 컴파일 오류를 일으킨다.

```c++
class CTestA
{
public:
	CTestA() {};
	virtual void someMethod(int i) { std::cout << "A" << std::endl; }   //...1)
};

class CTestB : public CTestA
{
public:
	CTestB() {};
	virtual void someMethod(double i) override { std::cout << "B" << std::endl; }  //...2)
	
};

```
위의 코드는 ...2) 에서 ' override 로 선언된 멤버 함수는 기본 클래스 멤버를 재정의하지 않습니다' 하는 오류를 일으킨다.
이는 파생 클래스에서의 override 의도에 맞지 않는 새 메서드 생성을 방지하고 또한 오버로딩 관계에 있는 부모 클래스의 메서드가 변경사항이 있을 때, 해당 메서드를 오버로딩 한 파생 클래스 들의 메서드들을 체크하는 데에 도움이 된다.

- 생성자의 상속
```c++
class CTestA
{
public:
	CTestA() { std::cout << "A : default constructor" << std::endl; }
	CTestA(const std::string& str) { std::cout << "A : constructor with string" << std::endl; }
};

class CTestB : public CTestA
{
public:
	CTestB(int i) { std::cout << "B : constructor with int" << std::endl; }
	
};

int main()
{
	CTestA testA("testA");   //...1)
	CTestB testB1(1);   //...2)
	CTestB testB2("testB");   //...3)  오류!
	return 0;
}
```
...1) 과 ...2) 는 정상적으로 컴파일 되지만 ...3) 에서 오류 ('인수 목록이 일치하는 생성사 "CTestB::CTestB"의 인스턴스가 없습니다.' ) 가 발생한다.

CTestB 객체 생성에 String 을 인자로 하는 생성자를 이용하고 싶다면 다음과 같이 using 키워드를 사용한다.
```c++
class CTestB : public CTestA
{
public:
	using CTestA::CTestA;
	CTestB(int i) { std::cout << "B : constructor with int" << std::endl; }
	
};

// 결과
A : constructor with string
A : default constructor
B : constructor with int
A : constructor with string
```
위와 같이 CTestB 를 선언하면 CTestB에 CTestA 의 생성자가 추가되어 ...3) 이 CTestA 로 부터 상속 받은 string 타입 생성자를 호출해 정상으로 컴파일된다.

\- using 키워드를 사용할 때 부모 클래스의 생성자를 모두 상속 받아야 한다. 또한 다중 상속 받은 경우 부모 클래스 중에 같은 파라미터 목록의 생성자를 가진 경우가 있다면 생성자를 상속 받을 수 없다. 어느 부모 클래스의 생성자를 호출해야 할 지 알 수 없기 때문이다. 이런 경우 파생 클라스 에서 명시적으로 오버라이딩 하여 해결한다.

- Static 메서드의 override
 C++ 에서 Static 메서드는 오버로딩 할 수 없다. 우선 메서드가 static 이면서 동시에 virtual 일 수 없다. 파생클래스에서 부모 클래스의 static 메서드와 같은 이름의 메서드를 정의하면 그 두 개의 메서드는 서로 다른 메서드일 뿐이다.

```c++
class CTestA
{
public:
	static void printString() { std::cout << "CTestA" << std::endl; }
	virtual void printStringSub() { std::cout << "CTestA : Sub" << std::endl; }
};

class CTestB : public CTestA
{
public:
	static void printString() { std::cout << "CTestB" << std::endl; }
	void printStringSub() override { std::cout << "CTestB : Sub" << std::endl; }
};

int main()
{
	CTestB testB;
	CTestA& testA = testB;

	testA.printString();
	testB.printString();

	testA.printStringSub();
	testB.printStringSub();
	return 0;
}

// 결과
CTestA
CTestB
CTestB : Sub
CTestB : Sub
```

- 오버로딩 하지 않은 부모 클래스의 메서드를 호출하는 방법
```c++
class CTestA
{
public:
	virtual void print() { std::cout << "TestA" << std::endl; }
	virtual void print(int i) { std::cout << "TestA : " << i << std::endl; }
};

class CTestB : public CTestA
{
public:
	virtual void print() override { std::cout << "TestB" << std::endl; }
};
```

위의 코드에 대하여
```c++
int main()
{
	CTestB testB;
	testB.print(2);   //...1)

	return 0;
}
```
...1 ) 에서 '함수 호출 인수가 너무 많습니다' 오류가 뜬다.

```c++
int main()
{
	CTestB testB;
	CTestA* testPtr = &testB;
	testPtr->print(5);

	return 0;
}

// 결과
TestA : 5
```
포인터나 참조형을 통해 부모 클래스로 접근하면 정상적으로 컴파일 된다.
-> 명시적으로 해당 클래스 타입으로 접근될 때만 부모 클래스의 메서드들이 감춰지고, 간단하게 부모 클래스로 타입 캐스팅만 하면 다시 이용할 수 있게 된다.

오버라이딩 구현을 일부에만 적용하고 나머지는 부모 클래스 것을 그대로 사용하고 싶다면 using 키워드를 이용한다.
```c++
class CTestA
{
public:
	virtual void print() { std::cout << "TestA" << std::endl; }
	virtual void print(int i) { std::cout << "TestA : " << i << std::endl; }
};

class CTestB : public CTestA
{
public:
	using CTestA::print;
	virtual void print() override { std::cout << "TestB" << std::endl; }
};

int main()
{
	CTestB testB;
	testB.print(2);

	return 0;
}

// 결과
TestA : 2
```

using 키워드는 선언된 이름만 같으면 모두 사용하도록 허락하기 때문에 조심히 사용해야 한다.
*** 찾기 어려운 버그를 만들지 않으려면 메서드를 오버라이딩할 때 서로 다른 파라미터로 정의된 해당 메서드의 모든 버전을 오버라이딩하거나 using 키워드를 이용해서 명시적으로 베이스 클래스의 메서드 이용을 선언하는 것이 바람직하다.

- private 또는 protected 로 선언된 부모클래스의 메서드를 override
private 또는 protected 는 접근 권한에 관한 것일 뿐 오버로딩과는 관계가 없다. 즉, private 로 선언된 부모 클래스의 메서드를 파생 클래스에서 호출할 수 없지만 오버라이딩을 할 수 있다.

```c++
class CTestA
{
private:
	int mNum;

public:
	virtual int getNum()
	{
		return getNumOne() * getNumTwo();
	}
	virtual void setNum(int inputNum) { mNum = inputNum; }
	virtual int getNumOne() { return mNum; }

private:
	virtual int getNumTwo() { return 20; }
};

class CTestB : public CTestA
{
private:
	virtual int getNumTwo() { return 30; }   //...1)
};

int main()
{
	CTestB test;
	test.setNum(5);   //...2)
	std::cout << "Num : " << test.getNum() << std::endl;

	return 0;
}

// 결과
Num : 150
```
... 1 )  public 메서드는 그대로 둔 채 private 메서드를 오버라이딩 하여 행동 방식이 다른 새로운 클래스를 만든다.
...2 )  부모 클래스에 정의된 getNum() 메서드는 자동으로 오버라이딩된 private getNumTwo() 메서드를 호출한다.

- 부모 클래스의 메서드를 오버로드할 때 다른 디폴트 인자값을 갖는 방법
파생 클래스와 부모 클래스는 각자 서로 다른 디폴트 인자값을 가질 수 있다. 어느 디폴트 인자값이 이용되는지의 기준은 호출할 때 어느 클래스 타입을 이용했는냐에 따른다.

```c++
class CTestA
{
public:
	virtual void print(int i = 2)
	{
		std::cout << "TestA : " << i << std::endl;
	}
};

class CTestB : public CTestA
{
public:
	virtual void print(int i = 9)
	{
		std::cout << "TestB : " << i << std::endl;
	}
};

int main()
{
	CTestA testA;
	CTestB testB;

	CTestA& testARefToB = testB;

	testA.print();
	testB.print();
	testARefToB.print();   //... 1)

	return 0;
}

// 결과
TestA : 2
TestB : 9
TestB : 2   // ...1)
```
...1 )  CTestB 객체를 CTestA 타입 포인터 또는 참조로 접근하여 print() 를 호출하면 print() 의 바디는 CTestB 의 것이 실행되지만 디폴트 인자는 CTestA 에서 정의한 2가 적용된다.

- 파생 클래스에서의 복제 생성자, 대입 연산자
파생 클래스가 포인터 같은 특별한 데이터가 없을 때는 복제 생성자와 대입연산자를 굳이 구현할 필요 없다. 파생클래스 에서 구현하지 않으면 컴파일러가 디폴트 복제 생성자나 대입연산자를 만들어준다. 이때 부모 클래스 멤버에 대해서는 베이스 클래스의 복제 생성자나 대입 연산자가 작용된다.

반면, 복제 생성자를 파생 클래스에 명시적으로 정의할 때는 반드시 부모의 복제 생성자를 호출해주어야 한다. 부모의 복제 생성자를 호출하지 않을 경우 부모 클래스의 데이터에 대해서는 디폴트 생성자가 사용된다.

```c++
class CTestA
{
private:
	int numA = 100;
public:
	CTestA() { std::cout << "CTestA constructor" << std::endl; }

	CTestA(const CTestA& input) 
		: numA(input.numA)
	{
		std::cout << "CTestA copy constructor" << std::endl; 
	}

	virtual void print(int i = 2)
	{
		std::cout << "TestA : " << i << std::endl;
	}
};

class CTestB : public CTestA
{
private:
	int numB = 200;
public:
	CTestB() { std::cout << "CTestB constructor" << std::endl; }
	CTestB(const CTestB& input)
		: numB(input.numB)   //...1)
	{
		std::cout << "CTestB copy constructor" << std::endl;
	}
	virtual void print(int i = 9)
	{
		std::cout << "TestB : " << i << std::endl;
	}
	void setNum(int i) { numB = i; }
	void printNum() { std::cout << numB << std::endl; }
};

int main()
{
	CTestB testB;
	testB.setNum(300);
	CTestB testB2(testB);   //...2)
	testB2.printNum();

	return 0;
}

// 결과
CTestA constructor
CTestB constructor
CTestA constructor
CTestB copy constructor
300
```
...1 )  에서 부모 클래스의 복제 생성자를 호출하지 않았기 때문에  ..2 )  에서 CTestA 의 디폴트 생성자를 호출한다.
따라서 위의 코드의 일부분을 다음으로 변경한다.
```c++
...
class CTestB : public CTestA
{
private:
	int numB = 200;
public:
	CTestB() { std::cout << "CTestB constructor" << std::endl; }
	CTestB(const CTestB& input)
		: CTestA(input)
		, numB(input.numB)
	{
		std::cout << "CTestB copy constructor" << std::endl;
	}
...

// 결과
CTestA constructor
CTestB constructor
CTestA copy constructor
CTestB copy constructor
300
```

- virtual 심화

\- 오버로딩 되지 않은 경우,  클래스 참조형 메서드 호출
```c++
class CTestA
{
public:
	 void test() { std::cout << "A" << std::endl; }
};

class CTestB : public CTestA
{
public:
	 void test()  { std::cout << "B" << std::endl; }
};

int main()
{
	CTestB testB;
	CTestA& ref = testB;
	ref.test();

	return 0;
}

// 결과
A
```

```c++
class CTestA
{
public:
	virtual void test() { std::cout << "A" << std::endl; }
};

class CTestB : public CTestA
{
public:
	virtual void test() override { std::cout << "B" << std::endl; }
};

int main()
{
	CTestB testB;
	CTestA& ref = testB;
	CTestA testA = testB;

	ref.test();
	testA.test();

	return 0;
}

// 결과
B
A
```

- virtual의 구현 원리
\- 컴파일러가 클래스 정의 코드를 컴파일하면 그 클래스의 모든 데이터 멤버와 메서드가 들어 있는 바이너리 객체가 만들어진다.
-> virtual 이 아닌 메서드의 경우, 메서드 호출 시 제어권의 전달이 컴파일 타임에 맞춰 직접 하드 코딩된다.
-> virtual 로 선언된 메서드의 경우 vtable 이라 불리는 특별한 메모리 영역을 찾아본다.
-> 하나 이상의 virtual 메서드를 가지는 클래스는 자신만의 vtable 을 가지며 이 vtable 을 통해 virtual 메서드의 구현부를 가리키는 포인터를 가리킨다.
-> 어떤 객체의 메서드가 호출되면 그 객체에 딸린 vtable 에서 해당 메서드의 오버라이딩된 포인터를 찾아서 객체의 실제 타입에 맞추어 올바른 버전의 메서드가 실행되도록 한다.

- RTTI 와 dynamic_cast 및 typeid 연산자

- public 이외의 상속
부모 클래스와의 관계를 protected 로 선언하면 부모 클래스의 모든 public 메서드와 멤버가 파생 클래스에서 protected 로 취급된다. 이는 private 도 마찬가지 이다.

- 버츄얼 베이스 클래스

```c++
class CAnimal
{
public:
	virtual void eat() = 0;
	virtual void sleep() { std::cout << "zzz" << std::endl; }
};

class CDog : public virtual CAnimal   //...1)
{
public:
	virtual void bark() { std::cout << "Woof!" << std::endl; }
	virtual void eat() override { std::cout << "The dog has eaten" << std::endl; }
};

class CBird : public virtual CAnimal   //...2)
{
public:
	virtual void chirp() { std::cout << "Chirp" << std::endl; }
	virtual void eat() override { std::cout << "The bird has eaten" << std::endl; }
};

class CDogBird : public CDog, public CBird
{
public:
	virtual void eat() override { CDog::eat(); }
};

int main()
{
	CDogBird kimera;
	kimera.sleep();   //...3)
	return 0;
}
```
...1) 또는 ...2) 둘 중 하나라도 부모 클래스를 virtual 로 선언되지 않았다면 ...3) 에서 'CDogBird::sleep() 이 모호합니다.' 라는 오류를 일으킨다.

중복되는 부모 클래스를 버츄얼로 상속 받으면 모호성이 발생하지 않는다.
CAnimal 을 virtual 로 상속 받을 경우 공통 부모에 대해서는 하나의 객체만 생성도기 때문에 sleep() 이 실행될 때 어느 쪽 객체를 이용해야 할지 선택할 필요가 없어진다.
