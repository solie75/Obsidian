1. strcmp, strncmp ^ed2c96

```c++
int strcmp(const char* str1, const char* str2);
// str1 : 비교할 문자열 1
// str2 : 비교할 문자열 2

int strncmp(const char* str1, const char* str2, size_t n);
// str1 : 비교할 문자열 1
// str2 : 비교할 문자열 2
// n : 비교할 문자열 길이

```

위의 예시 중 strncmp 에서 n 은 size_t 이고 size_t 는 unsigned int 이므로 0보다 큰 값이 전달 되어야 한다. 또한 str1, str2 문자열보다 큰 값을 넣게 되면 알아서 문자열 전체 길이를 비교한다.
예를 들어 str1이 문자 5개이고 str2 가 문제 10개로 이루어져 있으면 n에 5 이상의 값 아무것이나 넣어도 str1 이 5 이므로 5까지의 문자만 비교하게 된다.

strcmp 과 strncmp 는 매개변수로 들어온 두 개의 문자열을 비교하여
str1 < str2 인 경우에는 음수 반환
str1 > str2 인 경우에는 양수 반환
str1 == str2 인 경우에는 0을 반환한다.
아스키 코드 값으로 비교하기 때문에 문자를 대소 비교 가능하다.

- 예시 코드
```c++
int main()
{
	char str1[] = "MonkeyDLuffy";
	char str2[] = "MonkeyDLuffy";

	std::cout << strcmp(str1, str2) << std::endl;
	std::cout << strcmp(str1, "MonkeyCLuffy") << std::endl;
	std::cout << strcmp(str1, "MonkeyFLuffy") << std::endl;
}

// 결과
0
1
-1
```

- 예시 코드 2
```c++
int main()
{
	const char* str1 = "MonkeyDLuffy";
	const char* str2 = "MonkeyFLuffy";

	std::cout << strncmp(str1, str2, 5) << std::endl;
	std::cout << strncmp(str1, str2, 7) << std::endl;
	
// 결과
0
-1
}
```

위의 함수들을 c++ string 에서는 operator \==, !=, < 등을 문자열 연산에 대해 오버로딩 하고 있다.

- 문자열을 숫자로 변환 ^2c587a
1. int stoi(const string& \_Str, size_t* \_Idx = nullptr, int \_Base = 10);
2. long stol(const string& \_Str, size_t* \_Idx = nullptr, int \_Base = 10);
3. unsigned long stoul(const string& \_Str, size_t* \_Idx = nullptr, int \_Base = 10);
4. long long stoull(const string& \_Str, size_t* \_Idx = nullptr, int \_Base = 10);
5. float stof(const string& \_Str, size_t* \_Idx = nullptr)
6. double stod(const string& \_Str, size_t* \_Idx = nullptr)
7. long double stold(const string& \_Str, size_t* \_Idx = nullptr)

예시
```c++
int main()
{
	std::string str = "1234";

	int i = std::stoi(str);

	std::cout << i << std::endl;
}

// 결과
1234
```

