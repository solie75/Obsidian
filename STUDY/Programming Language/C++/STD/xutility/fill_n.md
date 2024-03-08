## std::fill_n

```c++
template< class OutputIt, class Size, class T >
OutputIt fill_n( OutputIt first, Size count, const T& value );
```

first : 배열의 시작 주소
count : 변경하려는 원소의 개수
value : 변경 값

예시
```c++
void Print(std::vector<int> _vec)
{
	std::vector<int>::iterator iter = _vec.begin();
	for (; iter != _vec.end(); ++iter)
	{
		std::cout << *iter << ' ';
	}
	std::cout << std::endl;
}

int main()
{
	std::vector<int> myvector(8, 10);
	Print(myvector);

	std::fill_n(myvector.begin(), 4, 20);
	Print(myvector);

	std::fill_n(myvector.begin() + 3, 3, 33);
	Print(myvector);

	return 0;
}

// 결과
10 10 10 10 10 10 10 10
20 20 20 20 10 10 10 10
20 20 20 33 33 33 10 10
```

