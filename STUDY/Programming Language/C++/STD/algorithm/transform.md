# std::transform

```c++
// algorithm.h
template <class _InIt, class _OutIt, class _Fn>
_CONSTEXPR20 _OutIt transform(const _InIt _First, const _InIt _Last, _OutIt _Dest, _Fn _Func) {
    // transform [_First, _Last) with _Func
    _Adl_verify_range(_First, _Last);
    auto _UFirst      = _Get_unwrapped(_First);
    const auto _ULast = _Get_unwrapped(_Last);
    auto _UDest       = _Get_unwrapped_n(_Dest, _Idl_distance<_InIt>(_UFirst, _ULast));
    for (; _UFirst != _ULast; ++_UFirst, (void) ++_UDest) {
        *_UDest = _Func(*_UFirst);
    }

    _Seek_wrapped(_Dest, _UDest);
    return _Dest;
}
```

매개변수에서
1. \_InIt_ : 입력 범위의 반복자 타입.
2. \_OutIt_ : 출력 범위의 반복자 타입.
3. \_Fn : 변환 함수(함수 객체 또는 함수 포인터)

과정
1. \_UFirst 부터 \_ULast 까지의 입력 범위를 확인한다. 
2. \_UFirst 와 \_ULast 를 언래핑하여 실제 반복자를 가져온다. 
3. 출력 범위의 실제 반복자를 가져온다. 
4. 입력 범위를 순회하면서 각 요소에 \_Func 함수를 적용하고 그 결과를 출력 범위에 작성한다.
5. 출력 범위의 실제 반복자를 원래 형태로 되돌린다.

예시
```c++
int main()
{
	std::vector<int> a(1);
	std::vector<int> b(2);
	std::vector<int> c(3);
	std::vector<int> d(4);

	std::vector<std::vector<int>> container;
	container.push_back(b);
	container.push_back(d);
	container.push_back(a);
	container.push_back(c);

	std::function<size_t(const std::vector<int>&)> sz_func = &std::vector<int>::size;

	std::vector<int> size_of_vector(4);\
	std::transform(container.begin(), container.end(), size_of_vector.begin(), [](std::vector<int> _v) {return _v.size() +1; });
	
	for (auto itr = size_of_vector.begin(); itr != size_of_vector.end(); ++itr)
	{
		std::cout << "벡터 크기 : " << *itr << std::endl;
	}
}

// 결과
벡터 크기 : 3
벡터 크기 : 5
벡터 크기 : 2
벡터 크기 : 4
```

container.begin() 부터 container.end() 까지 \[](std:;vector<\int>(\_v){return \_v.size()+1; } ); 에 입력 인자로 전한다. 해당 함수의 반환값은 size\_of\_vector.begin() 에 저장된다. 
이때 containter 의 첫번째 요소가 함수를 거쳐 그 반환값이 size\_of\_vector 의 첫번째 요소에 저장되고, containter 의 두 번째 요소가 함수를 거쳐 그 반환값이 size\_of\_vector 의 두 번째 요소에 저장 된다. 이때 size\_of\_vector 의 크기가 4미만 이면 오류를 일으키고 4 초과 이면 5번 째 요소 부터는 출력 팡에 벡터 크기 : 0 으로 나타난다.

** 위의 람다 함수 대신에 mem_fn() 대신할 수도 있지만 자주 사용되지 않는데 mem_fn 을 사용하기 위해서는 <\functional> 헤더를 추가해야 하지만 람다 함수는 그냥 쓸 수 있다.

```c++
int main()
{
	std::vector<int> a(1);
	std::vector<int> b(2);
	std::vector<int> c(3);
	std::vector<int> d(4);

	std::vector<std::vector<int>> container;
	container.push_back(b);
	container.push_back(d);
	container.push_back(a);
	container.push_back(c);

	std::function<size_t(const std::vector<int>&)> sz_func = &std::vector<int>::size;

	std::vector<int> size_of_vector(4);
	std::transform(container.begin(), container.end(), size_of_vector.begin(), std::mem_fn(&std::vector<int>::size));
	
	for (auto itr = size_of_vector.begin(); itr != size_of_vector.end(); ++itr)
	{
		std::cout << "벡터 크기 : " << *itr << std::endl;
	}
}
```
