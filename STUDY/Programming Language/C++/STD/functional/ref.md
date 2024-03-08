```c++
template <class _Ty>
_NODISCARD _CONSTEXPR20 reference_wrapper<_Ty> ref(_Ty& _Val) noexcept {
    return reference_wrapper<_Ty>(_Val);
}  // ...ㄱ)

template <class _Ty>
void ref(const _Ty&&) = delete;

template <class _Ty>
_NODISCARD _CONSTEXPR20 reference_wrapper<_Ty> ref(reference_wrapper<_Ty> _Val) noexcept {
    return _Val;
}  // ...ㄴ)
```

- Construct reference_wrapper
...ㄱ)  _elem_ 에 대한 참조를 유지하기에 적합한 reference_wrapper 타입의 객체를 생성한다. 
...ㄴ)  만약 인자가 그 자체로 reference_wrapper 라면, x 의 복사본을 생성하여 반환한다.
결국 해당 함수는 적절한 reference_wrapper 의 생성자를 호출한다.

- parameters
_elem_ : An _lvalue reference_, whose reference is stored in the object.
_x_ : A reference wrapper object, which is copied.

- return value
a reference_wrapper object of the appropriate type to hold an element of type T.

- 예시
```c++
int main() {
	int foo(10);

	auto bar = std::ref(foo);
	int &bar2 = foo;

	++bar;
	std::cout << foo << '\n';

	++bar2;
	std::cout << foo << '\n';

	return 0;
}

// 결과
11
12
```