1. 복사
```c++
char* copyString(const char* str)
{
	char* result = new char[strlen(str) + 1];
	strcpy(result, str);
	return result;
}
```

strlen() 이 메모리 크기가 아닌 문자 개수만 리턴한다는 것에 주의하자.
문자 개수 외에 NUL 를 위한 메모리 공간이 필요하다. 
즉 strlen() 에 1을 더해야 한다.

2.  연결
```c++
char* appendStrings(const char* str1, const char* str2, const char* str3)
{
    char* result = new char[strlen(str1) + strlen(str2) + strlen(str3) + 1];
    strcpy(result, str1);
    strcat(result, str2);
    strcat(result, str3);
    return result;
}

int main()
{
    char s1[20] = {};
    char s2[20] = {};
    char s3[20] = {};
    std::cin >> s1;
    std::cin >> s2;
    std::cin >> s3;
    std::cout << appendStrings(s1, s2, s3) << std::endl;
    std::cout << std::strlen(appendStrings(s1, s2, s3)) << std::endl;
}

// asdf,zxcv,qwer 를 차례로 입력했을 때 결과
asdf
zxcv
qwer
asdfzxcvqwer
12
```

- C 스타일 문자열에서 sizeof() 값과 strlen()값이 다른 것에 주의!
  C 스타일 문자열이 char[] 타입으로 저장되어 있다면 sizeof() 가 null 문자 '\\0' 을 포함해서 실제 메모리에 점유된 문자열 크기를 알려준다.

```c++
char s4[] = "game";
std::cout << sizeof(s4) << std::endl;
std::cout << std::strlen(s4) << std::endl;

결과는 
5
4
이다.


```

- 문자열 비교 비교
[[cstring#^ed2c96|strcmp, strncmp]]
```c++
char* a = "12";
char b[] = "12";

if(a == b) // 이것은 항상 false 을 반환한다. 문자열 내용 비교가 아닌 주소값 비교가 되기 때문.

if(strcmp(a, b) == 0) // 이와 같은 비교 방법을 사용해야 한다.
```

- 문자열 연산
```c++
int main()
{
	std::string myString = "hello";
	myString += ", there";
	std::string myOtherString = myString;
	if (myString == myOtherString)
	{
		myOtherString[0] = 'H';
	}

	std::cout << myString << std::endl;
	std::cout << myOtherString << std::endl;
}

// 결과
hello, there
Hello, there
```

주의
1. 문자열의 크기 변경와 신규 메모리 할당이 일어나고 있지만 메모리 릭 문제가 발생하지 않는다. 이는 코드 중간 과정에서 생성되는 string 객체가 모두 스택 메모리에서 생성되기 빼문에 메모리의 크기 변경과 할당이 여러번 가능하고 해당 스코프에서 벗어나는 순간 string 의 소멸자가 호출되면서 모든 메모리가 정리되기 때문이다.
2. 호환성 때문에 string 에서 C 스타일의 const 문자열 포인터를 얻기위해서는 c_str() 메서드를 사용한다. 하지만 이렇게 리턴받은 const 포인터는 string 이 메모리를 재할당하거나 string 객체가 삭제되는 순간 무효해 진다.


- std::string 리터럴
보통 문자열 리터럴은 const char*  타입으로 취급된다.  이 대신 std::string 으로 취급하려면 리터럴 뒤에 ' s ' 를 붙인다.
```c++
auto str1 = "hello"; // str1 의 타입은 const char* 이다.
auto str2 = "hello"s; // str2 의 타입은 std::string 이다.
```

- 수치 변환
1. 숫자를 string 으로 변환한다.
string to_string(자료형 변수);

2. 문자열을 숫자로 변환
[[cstring#^2c587a| 문자열->숫자 변환 함수들]]

3. 로우 문자열 리터럴
로우 문자열 리터럴 에서 /t, /n 과 같은 역슬레시 이스케이프 시퀀스 를 일반 문자열 취급한다.
따라서 
```c++
string str = "Hello "World"!"; // 이것은 컴파일 에러를 일으킨다.

string str = "Hello \"World\"!"; // 보통의 문자열에서 따옴표를 표현하기 위해서는 이렇게 이스케이프 시퀀스를 사용해야한다.
```

로우 문자열 리터럴을 이용하여 아래와 같이 이스케이프 시퀀스 없이 사용 가능하다.
```c++
string str = R"(Hello "World"!)"; // 로우 문자열 리터럴은 R"(  로 시작하고  )"  로 끝난다. 
```

```c++
string str = "Line 1
Line 2 with \t"
// 위와 같이 보통의 문자열 리터럴을 여러줄로 쓰면 컴파일 에러를 일으킨다.

string str = R"(Line 1
Line 2 with \t)";
// 이와 같이 로우 문자열 리터럴로 여러 줄로 쓸 수 있다. 이 때  \t 는 그대로 출력된다.
```