
std::vector, std::array, std::string 과 같은 컨테이너나 배열에 대래서 특정 인덱스에 접근하는데 그 인덱스가 컨테이너의 크기를 벗어난 경우 std::out_of_range 예외가 발생한다.

```c++
int main()
{
	std::vector<int> myVector = { 1, 2, 3, 4, 5 };

	try
	{
		// 벡터의 크기를 벗어난 인덱스에 접근하려고 시도
		int value = myVector.at(10);   //...1)
		std::cout << "Value at index 10 : " << value << std::endl;
	}
	catch (const std::out_of_range& e)   //...2)
	{
		// 예외 처리 : 범위를 벗어난 경우
		std::cerr << "Caught exception : " << e.what() << std::endl;   //...3)
	}

	return 0;
}

//  결과
Caught exception : invalid vector subscript
```
위의 예시에서 
..1 )  myValue.at(10) 은 벡터의 크기를 벗어난 인덱스에 접근하려고 시도하고 있다.
..2 )  std::out_of_range 예외가 발생하면 catch 블록이 실행되어 예외를 처리한다.
..3 )  e.what() 을 통해 예외의 내용을 얻을 수 있다.

