
\*  해당 문서에서 ' !보충 ' 은 이후에 보충이 필요한 내용을 의미한다.

- C++ 프로그램의 Build 과정
1. 전처리(Preprocess) 과정 : 코드 내에 지시된 메타 정보를 인식하여 자동으로 코드를 수정.
	- \#include 의 경우 대상이 되는 헤더파일의 모든 코드를 가져와 현재 소스파일에 \#include 부분에 삽입한다.
	- 헤더파일은 함수들의 함수 선언문이 작성되어 있다. 컴파일러는 현재의 소스파일이 include 된 헤더파일의 소스를 참조하도록한다.
2. 컴파일 작업 : 소스 코드를 기계어로 번역.
3. 링크 작업 : 기계어로 된 여러 오브젝트 파일을 묶어서 하나의 실행 파일로 만든다.

- main 함수
프로그램 실행 때 가장 먼저 호출되는 시작점. 이 함수의 int 타입 리턴 값은 프로그램의 상태를 알려주는 목적으로 사용된다. 
main 함수에 두 개의 매개변수가 사용되는 경우 : [[main()  의 명령 인수]]

- namespace
함수선언 및 함수 구현 을 하나의 namespace 안에 두어 이름이 같은 함수끼리 충돌하는 것을 방지할 수 있다.
호출할 때에는 함수 이름 앞에 namespace와 :: 를 붙인다. 이때 :: 를 '스코프 설정 연산자' 라고 한다.
using 지시자를 이용하여 네임스페이스 사용을 명시적으로 선언하면 그 아래의 코드에서는 namespace :: 를 사용하지 않아도 해당 namespcae 에 속한 이름을 불러다 쓸 수 있다.
이때 특정 namespace 에 속한 항목을 따로 지정할 수 있다. 
using (namespace 이름)::(항목 이름);  -> (항목 이름) 은 그 이후로 스코프 설정 없이 사용할 수 있다.
주의 : using 지시자나 using 선언을 헤더 파일에 넣어서는 안된다. 해당 헤더 파일의 사용자에게서 namespace 에 대한 선택권을 빼앗는 것이 되기 때문이다.

- 변수
변수는 각 종류에 때라 접근 범위가 정해진다. 변수는 선언할 때 초깃값을 설정할 수 있다. 설정하지 않을 경우에는 변수가 할당된 메모리 영역에 들어 있던 값이 된다. 흔히 쓰레기 값이라고 한다. 이는 버그를 유발할 수 있으니 되도록 변수는 초기화를 해주도록 한다.
1. 캐스팅 : 변수는 캐스팅을 통해서 다른 타입으로 변환할 수 있다. 
```c++
float mf = 3.14f; 에 대하여
1-1.  변수 앞에 캐스팅할 타입을 괄호로 명시한다.
	int i1 = (int) mf;
1-2. 타입 생성자 함수를 이용한다.
	int i2 = int(my);
1-3. 캐스팅 연산자를 이용한다.
	int i3 = static_cast<int>(mf);
```

특정 상황에서 변수의 타입 캐스팅이 자동으로 또는 강제로 일어난다. 이런 경우를 '묵시적 캐스팅' 이라 한다.

- 연산자
단항 연산자(unary operator), 이항 연산자(binary operator), 삼항 연산자(ternary operator) 가 있으며 이는 각 연산자가 한번에 대상으로 하는 변수의 개수에 따른다. 
주의 : ++, -- 는 변수 값을 1 증가 시키거나 1감소시키는 단항 연산자로 연산자가 변수 오른쪽에 있으면 '사후 증가' 로 해당 코드라인에서는 증가하기 전의 값을 가지지만 그 라인을 벗어나면 1 증가한다. 반대로 연산자가 변소 왼쪽에 있다면 '사전 증가'로 해당 코드 라인에서 1 증가한 값을 가진다.
(=, +, -, * , / , %, ++, --, +=, -=, *=, /=, %=, &, &=, \\, \\=, <<, >>, <<=, >>=, ^, ^= )
연산자별로 우선순위가 존재하나 괄호를 사용하여 명시적으로 지정해 명확하게 보이도록 한다.

- 논리 연산자
(<, <=, >, >=, \==, !=, !, &&, || )

- 열거 타입
각 변수에 int 정수 값을 할당하여 사용한다. 
enum (열거 이름 (예 : PieceType)) { King, Queen, Rook, Pawn};
이때 King 은 0, Queen 은 1, Rook 은 2, Pawn 은 3 이다.
```c++
PieceType myPiece;
myPiece = 0;
이 경우 정해진 열거 목록의 요소의 이름이 아닌 정수 자체를 직접 대입하고 있기 때문에 컴파일러에서 경고나 에러 메시지를 출력할 수 있다.
```
열거 타입의 각 항목에 대해 개별저긍로 특정 정수값을 할당하는 것 또한 가능하다.
enum (열거 이름 (예 : PieceType)) { King = 1, Queen, Rook = 10, Pawn};
열거 의 요소의 정수 값은 그 앞의 요소의 +1 이기 때문에 Queen 은 2, Pawn 은 11 이 된다.
enum 과 열거 이름 사이에 class 를 더하여 enum class (열거 이름) 으로 사용할 수 있으며 이는 enum 보다 엄격한 열거 타입이라 한다.
enum class 를 사용할 때에는 요소 이름 앞에 열거 타입 이름과 스코프 설정 연산자를 붙여 주어야 한다.
```c++
enum eNumber {one, two, three};
eNumber MyNum = one;
enum class eColor {Red, Green, Blue};
eColor MyColor = eColor::Green;
```
enum 은 기본적으로 int 형이지만 enum class 는 요소의 타입을 바꿀 수 있다. (이때 설정되는 요소의 타입은 정수 계열 형식이어야 한다. )
```c++
enum class Myfloat : long
{ ... }
```

주의 : 엄격한 열거 타입
	enum 의 경우 다른 enum 끼리의 연산이 가능하다.
즉, 다음의 코드는 어떤 경고나 에러를 일으키지 않는다.
```c++
enum ePieceType {King, Queen, Rook, Pawn};
enum eTest {test1, test2, test3};
int i = King + test2;
ePieceType MyPiece = Queen;
eTest MyTest = test1;
int j = MyPiece + MyTest;
if(MyPiece > MyTest)
{
	return true;
}
```
하지만 enum class 를 사용하면 위의 경우 연산 부분들에서  비교 연산자가 에러를 일으킨다.
enum class 의 경우 같은 enum class 끼리의 연산자 작용만이 가능하다.

- 구조체
하나 이상의 기본 타입 또는 다른 구조체를 조합하여 새로운 타입을 만드는 것이다.
구조체의 요소는 ' . ' 로 접근 가능하다.
```c++
struct sPlayerData
{
	int HP,
	int MP,
	char* Name,
	float str,
	float dex,
}

sPlayerData MyPlayerData;
MyPlayerData.dex = 7.34f;
```

- 조건문
1. If, else 문
2. switch 문
3. 조건 연산자 (A ? B : C)  A가 true 인 경우 B 를 A가 false 인 경우 C 를 반환한다.

- 배열
같은 데이터 타입의 값들은 일려로 나열한 것. 배열의 크기를 명시적으로 설정해야 한다. 배열의 크기는 상수 여야 한다. 또한 상수 표현식(constant expression) 을 배열 크기 선언에 이용할 수 있다.
```c++
int MyArray[3];
MyArray[0] = 0;
MyArray[1] = 0;
MyArray[2] = 0;
이는 3개의 정수 값을 담는 배열을 선언하고 배열의 항목을 모두 0으로 초기화 한 것이다.
다음은 여러 초기화 방식이다.
1.
int MyArray[3] = {0}; // 배열의 모든 요소가 0으로 초기화 된다.
2.
int MyArray[3] = {2}; // 첫번째 항목(MyArray[0])을 2로 초기화 하고 나머지 항목은 0으로 초기화 한다.
3.
int MyArray[] = {1, 2, 3, 4}; // 초기화 리스트를 사용하여 배열을 초기화. 컴파일러가 크기 4인 배열을 생성한다. MyArray[0] = 1, MyArray[3] = 4 가 된다.
```

1. 다차원 배열
```c++
2차원 배열
char TicTacToeBoard[3][3];
위의 배열은 3x3 표의 형태이며 TicTacToeBoard[0][0]는 왼쪽 위, TicTacToeBoard[1][1]는 중앙, TicTacToeBoard[2][2]는 우측 하단이 된다.
```

2. std::array
\<array> 헤더파일에 정의되어 있다.
std::array 는 C 언어의 배열을 대체하는 안전한(범위를 벗어난 잘못된 메모리 작업의 위험이 없는) 배열 타입이다. std::vector 와는 다르게 std::array 는 크기가 고정이 되어 있어 새로 항목의 추가나 삭제가 불가능하지만 오버헤드가 적다는 장점이 있다. 이에 포인터가 자동으로 잘못 타입캐스팅 되는 것도 피할 수 있고 iterator 를 활용하여 항목을 순회하거나 STL 알고리즘을 이용할 수 있다.
```c++
#include <array>
using namespace std;
int main()
{
	array<int, 3> arr = {9, 6, 3};
	return 0;
}
```

- 반복문
1. while
	break 키워드를 이용하면 루프에서 바로 벗어나 while 블록 이후의 코드를 연속해서 실행하게 한다. 반면 continue 키워드는 루프 내에서 해당 키워드 아래쪽 코드를 실행하지 않고 바로 루프의 처음으로 돌아가서 다시 실행하게 한다. ( 두 키워드 모두 가독성 문제로 권장 되지 않는다.)
2. do/ while
	while 블록의 코드가 처음에 한번 실행 되고 난 뒤 루프 반복 조건을 체크한다. 따라서 처음부터 루프 반복 조건이 거짓이더라도 전체적으로 한번은 while 의 코드가 실행된다. 
3. for
4. 구간 지정 for
	리스트를 순회하며 작업할 때 편리하다. C 언어의 배열, 초기화 리스트, 반복사 iterator 를 리턴하는 begin() 과 end() 함수를 맴버로 가지는 데이터 타입을 대상으로 한다.
```c++
std::array<int, 4> arr = {1, 2, 3, 4};
for (int i : arr) {
	std::cout << i << std::endl;
}
```

- 함수
한수는 선언부와 정의부로 나눌 수 있으며 선언부는 ' } '뒤에 ' ; ' 로 끝내야하지만 정의 부는 ' } ' 로 끝난다.
함수는 반환값의 자료형, 함수 이름, 함수가 받을 매개변수, 함수 본문으로 이루어져 있다.
```c++
// 다음은 두개의 int 형 정수를 받아 더한 값을 반환하는 함수이다.
// 선언부
int AddTwoInt (int A, int B);

// 정의부
int AddTwoInt (int A, int B)
{
	return A + B;
}

// 호출 부분
int FirstInt = 10;
int SecondInt = 3;
int result = AddTwoInt(FirstInt, SecondInt);
//  result 에는 13 이 대입된다.

// 새로운 함수 정의 문법/ 템플릿 함수에서 유용하다. !보충
auto func(int i) -> int
{
	return i + 2;
}

// 자동 함수 리턴 타입.
auto divideNumbers(double A, int B)
{
	if(A > 1.0)
	{
		return A;
	}

	return B;
}
```

- auto 키워드
1. 특정 변수의 타입을 컴파일 타임에 자동적으로 연역한다.
```c++
auto X = 123; // 이때 X 는 int 형 변수가 된다.
// 반환값이 여러 타입일 수 있는 함수의 반환값을 대입 받는 변수에 대해 auto 를 사용하여 컴파일 단계에서 변수의 자료형을 지정하도록 한다.
```
2. 위의 새로운 함수 정의 문법
3. 위의 자동 함수 리턴 타입
4. 제네릭 람다 표현식 !보충 (17장에서 설명)

- decltype 키워드
표현식을 인자로 받아서 그 표현식의 결과 타입이 무엇인지 알아낸다.
```c++
int X = 123;
decltype(X) y = 456;
// 컴파일러는 X 가 int 라는 사실로부터 Y 타입을 int 로 추정한다. 
```

auto 를 사용하면 표현식의 타입이 자동으로 연역된다. 이때 const 한정자가 붙어 있다면 const 속성을 없애버린다. 이와 같은 효과가 decltype 에는 없는데 이를 decltype(auto) 로 해결한다.

```c++
using namespace std;

const string sMessage = "Test";

const string& foo()
{
    return sMessage;
}

int main()
{
    auto f1 = foo();
    
    //const auto f1 = foo();

    f1 = "Fake";

    cout << "f1 은 : " << f1 <<  endl;
    cout << "sMessage 는 : " << sMessage << endl;
    
    return 0;
}

위의 코드 결과는 다음과 같다.
f1 은 : Fake
sMessage 는 : Test
f1 에 const 속성이 사라진 것을 확인 할 수 있다.
이에
auto f1 = foo(); 를 const auto& f1 = foo() 로 바꾸면 f1 = "Fake"; 에서 오류가 생긴다.
```

위의 const auto& 와 같은 효과를 내기 위해 다음을 사용한다.
```c++
decltype(foo()) f2 = foo();
// 이러면 const 속성이 유지된다.
이 코드의 경우 f2 는 const string& 타입이 되지만 foo()를 중복해서 표기해 지저분하다.
이를 다음의 코드로 해결한다.

decltype(auto) f3 = foo();
이렇게 하면 f3의 타입이 foo() 의 반환형과 동일한 const string& 이 된다.
```


- 포인터와 동적 메모리
동적 메모리는 컴파일 타임에 크기를 정할 수 없는 데이터를 이용할 수 있게 한다.

1. 스택 메모리 과 힙 메모리
스택은 함수가 호출될 때마다 스택 프레임이 생성되고 이전 스택 프레임의 위에 쌓인다고 생각하자. 맨 위의 스택 프레임은 프로그램 상태를 나타내는 것으로 보통 현재 실행 중인 함수에 관련된 것이다. 스택 프레임은 각 함수 간 메모리 공간을 격리시키는 중요한 역할을 한다. 함수가 반환하면 해당 스택 프레임의 로컬 변수는 해당 스택 프레임과 사라지기 때문에 메모리를 차지 하지 않는다. 즉, 프로그래머가 로컬 변수에 할당된 메모리를 직접 해제할 필요가 없다.
힙은 현재 실행 중인 함수나 스택 프레임과는 완전히 독립된 메모리의 영역이다. 함수의 호출과 리턴에 상관 없이 항상 존재해야 하는 변수라면 힙에 할당한다. 이 힙에 할당된 메모리는 프로그래머가 직접 해제해야 한다. (이 번거로움을 해결하기 위해 스마트 포인터를 활용한다.)

2. 포인터
특정 메모리 영역을 가리키는 변수를 뜻한다. 포인터 변수는 생성하고 바로 초기화 하지 않을 시에 임의 메모리 영역을 지정하고 있다. 이 포인터를 모르고 사용하면 엄한 메모리 영역을 조작하는 것이기에 포인터는 생성과 동시에 포기화 해주는 것이 좋다. (당장의 지정할 메모리 영역이 없다면 nullptr 로 초기화)

   1. 메모리를 할당할 때에는 new 연산자를 사용한다.
```c++
int* mIntegerPointer = new int;

```
   2. 역참조
	포인터를 역참조하여 변수값에 접근할 수 있다.
```c++
*mIntegerPointer = 8;
// 포인터인 mIntegerPointer 에 8 이라는 값을 대입하는 것이 아니라 mIntegerPointer가 가리키는 메모리의 값이 바뀌는 것이다.
```

   3. 메모리 해제
	사용이 끝난 메모리를 delete 연산자를 이용하여 할당을 해제(메모리 반환)한다. 할당 해제된 메모리에 접근하는 것을 막기 위해 해제 직후 nullptr 로 초기화 한다.
   
   4. 기존의 변수로부터 포인터 얻기
	기존의 변수 앞에 주소 참조 연산자 ' & ' 를 붙여서 해당 변수의 주소(포인터) 를 받는다.
```c++
int a = 10;
int* aPointer = &a;
```

   5. 포인터로 구조체 참조
	값에 접근하기 위해 추가적으로 역참조 ' * ' 를 포인터에 붙이고, 역참조된 변수에 ' . ' 을 붙여서 구조체 내 각 요소를 선택한다.
```c++
struct sEmployeeData
{
    char* name;
    int age;
    long salary;
};

int main()
{
    sEmployeeData mEmployee1;

    sEmployeeData* pEmployee = &mEmployee1;

    std::cout << (*pEmployee).age << std::endl;

    return 0;
}
```

위 예시를구조체 역참조 연산자 ' -> ' 로 간단하게 할 수 있다.
```c++
(*pEmployee).age
를
pEmployee->age
로 대신 사용할 수 있다.
``` 


   6. 매개변수 전달 방식
	- 값에 의한 전달 (passing by value)
	  함수를 호출할 때 함수의 매개변수에 변수의 값이 아닌 변수의 복사 값을 전달하는 것. 따라서 호출된 함수 내에서 매개변수의 값을 변경 하더라도 원래의 변수의 값은 변하지 않는다.
	- 참조에 의한 전달 (passing by reference)
	  함수 호출의 매개변수 전달에 값이 아닌 변수의 주소 값을 전달한다. 호출된 함수는 전달된 주소 값을 역참조하여 메모리의 값을 변경할 수 있다. 

   7. 동적 할당 배열
      힙 메모리에서 new[] 연산자를 이용하여 동적으로 배열을 할당할 수 있다. 
```c++
int arraySize = 4;
int* mVariableArray = new int[arraySize];
// int 변수를 arraySize개 만큼 저장할 수 있는 힙 메모리를 확보아여 그 주솟값을 포인터 변수에 대입해준다. 포인터 변수 자체는 스택에 위치하고 동적으로 할당된 배열 데이터는 힙에 위치하는 것이다.
```
위의 동적으로 할당된 배열을 사용하지 않는다면 delete[] 연산자로 힙 메모리에서 해제해야한다.
```c++
delete[] mVariableArray;
```

\* null 포인터 상수
NULL 은 포인터가 아니라 정수 0과 완전히 같기 때문에 nullptr 을 사용한다. 
```c++
void func(char* str) { std::cout << "char* version" << std::endl; }

void func(int i) { std::cout << "int version" << std::endl; }

int main()
{
    func(???);

    return 0;
}
??? 가 NULL 인 경우 "int version" 가 출력되고
??? 가 nullptr 인 경우 "char* version"가 출력된다.
```

   8. 스마트 포인터
객체에 유효한 스코프가 더 이상 없을 때 (함수의 반환 등으로 스코프를 벗어날 때) 자동으로 메모리를 해제한다. [[STUDY/Programming Language/C++/기타 개념/Smart Pointer|Smart Pointer]]
	- std::unique_ptr
	- std::shared_ptr
	- std::weak_ptr
위의 세 스마트 포인터는 모두 <\memory> 헤더 파일에 정의되어 있다.

   9. 참조형 매개변수
      함수의 매개변수의 정의부에서 각 매개변수 이름에 ' & ' 를 붙인다.
```c++
void AddOne(int i)
{
	i++; // 매개변수가 복사값이기 때문에 원래의 변수의 값은 변하지 않는다.
}

void AddOne(int& i)
{
	i++; // 본래의 변수의 값이 1 증가한다.
}
```

```c++
int main()
{
    int Num = 10;

    AddOne(Num);

    cout << Num << endl;

    return 0;
}

// AddOne 함수가 void AddOne(int i) 일 경우 10 이 출력되고,
// AddOne 함수가 void AddOne(int& i) 일 경우 11 이 출력된다.

여기에서 
void AddOne(int i) 의 경우 AddOne() 에 상수를 넣어도 문제가 없지만.
void AddOne(int& i) 일 경우 AddOne() 에 상수를 넣는다면 
'비 const 참조에 대한 초기 값은 lvalue 여야 합니다' 라는 오류가 생긴다. -> !보충 10장의 우측값 참조
```

- 문자열

1. 문자의 배열 이용
2. string 타입 이용
3. 프로그래머가 c++ 의 타입정의 기능을 이용해서 직접 정의 -> !보충 2장

- 예외 처리
대부분 프로그래밍 언어에서 함수의 리턴값을 nullptr 이나 특별한 에러코드에 매핑하는 방법을 사용하지만 익셉션은 더 나은 예회 처리 방법을 제공한다. [[try, throw, catch]]

- const
1. const 상수
   컴파일러에 의해 변수값이 바뀌지 않도록 보증된다. -> const로 선언된 파라미터를 함수 안에서 변경하려 하면 컴파일 에러가 발생한다.
   
   - 파라미터 보호를 위한 const
     파라미터를 통해서 원래의 값을 변경하는 것을 막는다.

   - 참조형 const
     참조형은 기본적으로 메모리 영역에 접근하여 값을 변경하고자 함인데 const 와는 모순되어 보인다. 하지만 참조형 파라미터 변수는 성능에 대한 관점에서 불필요한 복사를 막기위해 사용 될 대가 있는데 이 경우 const 가 의미있는 역할을 한다.  파라미터의 값 복제 오버헤드를 피하고 변조도 막는 효과를 보는 것이다. -> 참조형 const 변수는 객체를 이용할 때 더 중요하다. 객체는 정수 기본 타입에 비해 볼륨이 크고 복제 작업 자체가 의도치 않는 작용을 일으키기 쉽기 때문이다.


- 간단한 프로그램 만들기 사원 관리

```c++
// ConsoleApplication.cpp
// 직원 데이터 베이스 프로그램 개발
#include <iostream>
#include <memory>
#include <stdexcept>

#include "CEmployee.h"
#include "CDatabase.h"

enum class eDisplayMenuList
{
    Quit,
    Hire,
    Fire,
    Promote,
    DisplayAll,
    DisplayCurrent,
    DisplayFormer,
    End,
};

eDisplayMenuList displayMenu();
void doHire(CDatabase& db);
void doFire(CDatabase& db);
void doPromote(CDatabase& db);
void doDemote(CDatabase& db);



int main()
{
    CDatabase employeeDB;
    bool done = false;
    while (!done)
    {
        eDisplayMenuList selection = displayMenu();
        switch (selection)
        {
        case eDisplayMenuList::Hire:
            doHire(employeeDB);
            break;
        case eDisplayMenuList::Fire:
            doFire(employeeDB);
            break;
        case eDisplayMenuList::Promote:
            doPromote(employeeDB);
            break;
        case eDisplayMenuList::DisplayAll:
            employeeDB.displayAll();
            break;
        case eDisplayMenuList::DisplayCurrent:
            employeeDB.displayCurrent();
            break;
        case eDisplayMenuList::DisplayFormer:
            employeeDB.displayFormer();
            break;
        case eDisplayMenuList::Quit:
            done = true;
            break;
        }
    }

    return 0;
}

eDisplayMenuList displayMenu()
{
    eDisplayMenuList selectedMenu;
    cout << endl;
    cout << "Employee Database" << endl;
    cout << "-----------------" << endl;
    cout << "1) Hire a new employee" << endl;
    cout << "2) Fire an employee" << endl;
    cout << "3) Promote an employee" << endl;
    cout << "4) List all employee" << endl;
    cout << "5) List all current employwws" << endl;
    cout << "6) List all former employees" << endl;
    cout << "0) Quit" << endl;
    cout << endl;
    cout << "---> ";
    int inputNum;
    cin >> inputNum;
    selectedMenu = (eDisplayMenuList)inputNum;
    return selectedMenu;
}

void doHire(CDatabase& db)
{
    string firstName;
    string lastName;
    cout << "First name? ";
    cin >> firstName;
    cout << "Last name? ";
    cin >> lastName;
    try
    {
        db.addEmployee(firstName, lastName);
    }
    catch (const std::exception& exception)
    {
        cerr << "Unable to add new employee: " << exception.what() << endl;
    }
}

void doFire(CDatabase& db)
{
    int employeeNumber;
    cout << "Employee number? ";
    cin >> employeeNumber;
    try
    {
        CEmployee& emp = db.getEmployee(employeeNumber);
        emp.fire();
        cout << "Employee " << employeeNumber << " terminated. " << endl;
    }
    catch (const std::exception& exception)
    {
        cerr << "Unable to terminate employee: " << exception.what() << endl;
    }
}

void doPromote(CDatabase& db)
{
    int employeeNumber;
    int raiseAmount;
    cout << "Employee number? ";
    cin >> employeeNumber;
    cout << "How much of a raise? ";
    cin >> raiseAmount;

    try
    {
        CEmployee& emp = db.getEmployee(employeeNumber);
        emp.promote(raiseAmount);
    }
    catch (const std::exception& exception)
    {
        cerr << "Unable to promote employee: " << exception.what() << endl;
    }
}

void doDemote(CDatabase& db)
{
    int employeeNumber;
    int demeritAmount;
    cout << "Employee number? ";
    cin >> employeeNumber;
    cout << "How much of a demerit? ";
    cin >> demeritAmount;

    try
    {
        CEmployee& emp = db.getEmployee(employeeNumber);
        emp.demote(demeritAmount);
    }
    catch (const std::exception& exception)
    {
        cerr << "Unable to demote employee: " << exception.what() << endl;
    }
}

```

```c++
// CEmployee.h
#pragma once
#include <string>
#include <iostream>

using namespace std;

const int kDefaultStartingSalary = 30000;

class CEmployee
{
private:
	std::string mFirstName;
	std::string mLastName;
	int mEmployeeNum;
	int mSalary;
	bool mHired;

public:
	CEmployee();
	~CEmployee();
	void promote(int raiseAmount = 1000);
	void demote(int demeritAmount = 1000);
	void hire(); // 종업원 채용 및 재채용
	void fire(); // 종원원 해고
	void display() const; //  콘솔에 종업원 정보 출력;

	void setFirstName(const std::string& firstName);
	const std::string& getFirstName() const;

	void setLastName(const std::string& lastName);
	const std::string& getLastName() const;

	void setEmployeeNumber(int employeeNumber);
	int getEmployeeNumber() const;

	void setSalary(int newSalary);
	int getSalary() const;

	bool  getIsHired() const;
};

// CEmployee.cpp
#include "CEmployee.h"

CEmployee::CEmployee()
	: mFirstName("")
	, mLastName("")
	, mEmployeeNum(-1)
	, mSalary(kDefaultStartingSalary)
	, mHired(false)
{
}

CEmployee::~CEmployee()
{
}

void CEmployee::promote(int raiseAmount)
{
	setSalary(getSalary() + raiseAmount);
}

void CEmployee::demote(int demeritAmount)
{
	setSalary(getSalary() - demeritAmount);
}

void CEmployee::hire()
{
	mHired = true;
}

void CEmployee::fire()
{
	mHired = false;
}

void CEmployee::display() const
{
	cout << "Employee: " << getLastName() << getFirstName() << endl;
	cout << "--------------------------------" << endl;
	cout << (mHired ? "Current Employee" : "Former Employee") << endl;
	cout << "Employee Number: " << getEmployeeNumber() << endl;
	cout << "Salary: $" << getSalary() << endl;
	cout << endl;
}

void CEmployee::setFirstName(const std::string& firstName)
{
	mFirstName = firstName;
}

const std::string& CEmployee::getFirstName() const
{
	return mFirstName;
}

void CEmployee::setLastName(const std::string& lastName)
{
	mLastName = lastName;
}

const std::string& CEmployee::getLastName() const
{
	return mLastName;
}

void CEmployee::setEmployeeNumber(int employeeNumber)
{
	mEmployeeNum = employeeNumber;
}

int CEmployee::getEmployeeNumber() const
{
	return mEmployeeNum;
}

void CEmployee::setSalary(int newSalary)
{
	mSalary = newSalary;
}

int CEmployee::getSalary() const
{
	return mSalary;
}

bool CEmployee::getIsHired() const
{
	return mHired;
}

```

```c++
// CDataBase.h
#pragma once
#include <iostream>
#include <vector>
#include "CEmployee.h"

// 새로운 종업원에 대해 종업원 번호를 자동으로 부여할 수 있어야 한다.
// 이를 위한 시작 번호를 정의하는 상수 변수를 선언한다.
const int kFirstEmployeeNumber = 1000;

class CDatabase
{
private:
	std::vector<CEmployee> mEmployees;
	int mNextEmployeeNumber;

public:
	CDatabase();
	~CDatabase();

	CEmployee& addEmployee(const std::string& firstName, const std::string& lastName);
	CEmployee& getEmployee(int employeeNumber);
	CEmployee& getEmployee(const std::string& firstName, const std::string& lastName);

	void displayAll() const;
	void displayCurrent() const;
	void displayFormer() const;
};

// CDatabase.cpp
#include "CDatabase.h"

CDatabase::CDatabase()
	: mNextEmployeeNumber(kFirstEmployeeNumber)
{
}

CDatabase::~CDatabase()
{
}

CEmployee& CDatabase::addEmployee(const std::string& firstName, const std::string& lastName)
{
	CEmployee theEmployee;
	theEmployee.setFirstName(firstName);
	theEmployee.setLastName(lastName);
	theEmployee.setEmployeeNumber(mNextEmployeeNumber++);
	theEmployee.hire();
	mEmployees.push_back(theEmployee);
	return mEmployees[mEmployees.size() - 1];
}

CEmployee& CDatabase::getEmployee(int employeeNumber)
{
	for (CEmployee& employee : mEmployees)
	{
		if (employee.getEmployeeNumber() == employeeNumber)
		{
			return employee;
		}
	}
	throw runtime_error("No employee found.");
}

CEmployee& CDatabase::getEmployee(const std::string& firstName, const std::string& lastName)
{
	for (CEmployee& employee : mEmployees)
	{
		if (employee.getFirstName() == firstName && employee.getLastName() == lastName)
		{
			return employee;
		}
	}
	throw runtime_error("No employee found.");
}

void CDatabase::displayAll() const
{
	for (const CEmployee& employee : mEmployees)
	{
		employee.display();
	}
}

void CDatabase::displayCurrent() const
{
	for (const CEmployee& employee : mEmployees)
	{
		if (employee.getIsHired())
		{
			employee.display();
		}
	}
}

void CDatabase::displayFormer() const
{
	for (const CEmployee& employee : mEmployees)
	{
		if (!employee.getIsHired())
		{
			employee.display();
		}
	}
}
```