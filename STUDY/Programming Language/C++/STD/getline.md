입력슽림에서 한 줄의 데이터를 읽어온다.

문자열을 입력받아서 입력 스트림에서 지정한 구분자(delimiter)문자가 나올때까지 문자열을 채우고, 해당 구분자 문자를 버린 후 문자열을 반환한다.

```c++
#include <iostream>
#include <string>

int main() {
	std::string input;

	std::cout << "Enter a line of text: ";
	std::getLine(std::cin, input); // 표준 입력에서 한 줄의 데이터를 읽어온다.

	std::cout << "Entered: " << input << std::endl;

	return 0;
}
```

위의 코드에서 std::getline 함수는 std::cin (표준 입력) 스트림에서 데이터를 읽어와서 input 변수에 저장한다. 사용자가 입력한 한 술의 데이터를 읽어온다. 

구분자(deliniter) 문자를 지정하지 않으면 std::getline 함수는 개행 문자 ('/n') 를 기본 구분자로 사용하여 한 줄의 데이터를 읽어온다. 이를 통해 한번에 사용자의 입력을 받을 수 있다.

