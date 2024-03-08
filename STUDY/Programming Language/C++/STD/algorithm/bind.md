
# std::bind

함수 객체 생성 시에 인자를 특정할 수 있다.
```c++
template <class _Fx, class... _Types>
_NODISCARD _CONSTEXPR20 _Binder<_Unforced, _Fx, _Types...> bind(_Fx&& _Func, _Types&&... _Args) {
    return _Binder<_Unforced, _Fx, _Types...>(_STD forward<_Fx>(_Func), _STD forward<_Types>(_Args)...);
}
```


예시
```c++
void add(int _x, int _y)
{
	std::cout << _x << " + " << _y << " = " << _x + _y << std::endl;
}

void subtract(int _x, int _y)
{
	std::cout << _x << " - " << _y << " = " << _x - _y << std::endl;
}

int main()
{
	auto add_with_2 = std::bind(add, 2, std::placeholders::_1);
	add_with_2(3);
	add_with_2(3, 4);

	auto subtract_from_2 = std::bind(subtract, std::placeholders::_1, 2);
	auto negate = std::bind(subtract, std::placeholders::_2, std::placeholders::_1);

	subtract_from_2(3);
	negate(4, 2);
}

// 결과
2 + 3 = 5
2 + 3 = 5
3 - 2 = 1
2 - 4 = -2
```

1. add_with_2  함수 객체는  add 함수를 호출하는데, 첫 번째 인자로는 항상 2가 전달되고, 두 번째 인자로는 std::placeholders::\_1 로 대체된다.
이는 나중에 add_with_2() 를 호출할 때 전달되는 인수이다. 예를 들어 add_with_2(3) 을 호출하면 add(2, 3) 이 호출되는 것과 같다. 만약 add_with_2(3, 4) 를 호출한다면 add(2, 3) 이 호출되고 4는 무시된다.
2. subtract_from_2 함수 객체는 subtract() 함수를 호출하는데, 첫번째 인수로 항상 placeholders::\_1 로 대체되고 두번째 인자로는 항상 2가 전달 된다. 예를 들어 subtract_from_2(3) 을 호출하면 subtract(3, 2) 를 호출하는 것과 같다.
3. negate 함수 객체는 subtract() 함수를 호출하는데, 첫 번째 인자로는 placeholders::\_2 로, 두 번째 인자로는 placeholders::\_1 로 대체된다. 예를 들어 negate(4, 2)를 호출하면 subtract(2, 4) 가 호출된다. 
즉 std::bind() 를 사용하여 함수에 인수를 바인딩함으로써, 호출될 때마다 일부 인수를 고정하거나 인수의 순서를 변경할 수 있다.