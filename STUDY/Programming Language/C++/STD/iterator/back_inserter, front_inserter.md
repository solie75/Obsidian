# std::back_inserter()

```c++
template <class _Container>
_NODISCARD _CONSTEXPR20 back_insert_iterator<_Container> back_inserter(_Container& _Cont) noexcept /* strengthened */ {
    // return a back_insert_iterator
    return back_insert_iterator<_Container>(_Cont);
}
```

parameter
- \_Cont : push_back 함수를 제공하는 컨테이너

return value
- \_Con 의 끝에 더해질 수 있는 std::back_insert_iterator 이다.

```c++
int main()
{
	std::vector<int> v{ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
	std::fill_n(std::back_inserter(v), 3, -1);
	for (int n : v)
	{
		std::cout << n << ' ';
	}
	std::cout << std::endl;
}

// 결과
1 2 3 4 5 6 7 8 9 10 -1 -1 -1
```

# std::front_inserter()

```c++
template <class _Container>
_NODISCARD _CONSTEXPR20 front_insert_iterator<_Container> front_inserter(_Container& _Cont) {
    return front_insert_iterator<_Container>(_Cont);
}
```

x 의 처음에 새로운 요소를 추가하는 front-insert iterator 를 생성한다.

예시
```c++
int main()
{
	std::deque<int> foo, bar;
	for (int i = 1; i <= 5; ++i)
	{
		foo.push_back(i);
		bar.push_back(i * 10);
	}
	std::copy(bar.begin(), bar.end(), std::front_inserter(foo));

	std::cout << "foo contains: ";
	for (std::deque<int>::iterator iter = foo.begin(); iter != foo.end(); ++iter)
	{
		std::cout << *iter << ' ';
	}

	std::cout << " \n";

	return 0;
}

// 결과
foo contains: 50 40 30 20 10 1 2 3 4 5
```