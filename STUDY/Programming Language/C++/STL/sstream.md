- 문자열 스트림
  문자열에 여러가지 자료형이 들어왔을 때 용도에 맞게 파싱하기에 유용하다.

- stringstream
  입출력 스트림 : 입력 스트림과 출력 스트립을 모두 할 수 있다.
  
- istringstream
  입력 스트림.
  문자열을 공백과 '\\n' 을 기준으로 여러 개의 다른 형식으로 차례대로 분리할 때 편리하다.
  반복문 실행 시에 자료형에 맞는 데이터가 없을 때까지 실행된다.

```c++
// istringstream 사용 예시 1

istringstream iss("test\n123 afa 456");
string s1, s2;
int i1, i2;
iss >> s1 >> i1 >> s2 >> i2; // 문자열을 파싱하고 변수형에 맞게 변환한다.

cout << s1 << endl;
cout << i1 << endl;
cout << s2 << endl;
cout << i2 << endl;

// 결과
test
123
afa
456
```

```c++
// istringstream 사용 예시 2
int main()
{
    string str1 = "1d2s#$10DG";
    string str2 = "123lkj333d3j";

    istringstream iss1(str1);
    istringstream iss2(str2);

    int num1, num2;
    while (iss1 >> num1)
    {
        cout << num1 << " ";
    }
    cout << endl;
    while (iss2 >> num2)
    {
        cout << num2 << " ";
    }
    cout << endl;

    istringstream iss3(str1);
    istringstream iss4(str2);
    char ch1, ch2;
    while (iss3 >> ch1)
    {
        cout << ch1 << " ";
    }
    cout << endl;
    while (iss4 >> ch2)
    {
        cout << ch2 << " ";
    }
    cout << endl;
}

// 결과
1
123
1 d 2 s # $ 1 0 D G
1 2 3 l k j 3 3 3 d 3 j

// 위의 코드는 istringStream 이 stringstream 이어도 똑같이 작동한다.
```

- ostringstream
  출력 스트림
  문자열을 조립하거나 특정 형식을 문자열로 변환하기 위해 사용한다.
```c++
int main()
{
	ostringstream oss;
	string s1 = "abc", s2 = "giw";
	int i1 = 123145;
	double d1 = 3.591;
	oss << s1 << "\n" << i1 << "\n" << s2 << "\n" << d1; // 문자열을 붙인다.
	cout << oss.str(); // 문자열을 꺼낸다.
}

// 결과
abc
123145
giw
3.591

// 위의 코드는 ostringstream 이 stringstream 이어도 똑같이 작동한다.
```

- str(), clear()
  \- str(string s) : stringstream 에 저장된 문자열을 바꾼다. 이때 s가 ""일 경우 문자열을 삭제하는 것과 같다.
  \- str() : stringstream 이 저장하고 있는 문자열의 복사본을 반환한다.
  \- clear() : stringstream 재사용하려면 clear() 을 실행한다. 이 때 저장된 문자열이 삭제되지는 않는다.

- get(), unget()
  \- get() : 커서를 뒤로 옮기면서 반환한다.
  \- unget() : 커서를 앞으로 다시 옮긴다.

- eof()
  \- 파일의 끝에 도달했는지 알려주는 함수

- fail()
  \- 최근 수행한 연산에 에러가 발생했는지 확인한다.

- getline()
  \-  입력 스트림에서 데이터를 한 줄 씩 읽는 경우 사용한다.
  \- 이 메서드는 미리 설정한 버퍼가 가득 채워질 때까지 문자 한 줄을 읽는다. 이때 한줄의 끝을 나타내는 '\
\0' 문자도 버퍼의 크기에 포함된다.

