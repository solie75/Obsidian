# std::move

```c++
template <class _Ty>
_NODISCARD constexpr remove_reference_t<_Ty>&& move(_Ty&& _Arg) noexcept { // forward _Arg as movable
    return static_cast<remove_reference_t<_Ty>&&>(_Arg);
}
```

템플릿 매개변수로 전달된 값을 rvalue 로 캐스팅한다.
이동 시맨틱스로  활용되어 객체의 소유권을 전달하는 데 사용된다.
반환값 또한 rvalue 로 여겨지게 된다.

보통 rvalue 는 주소 값이다 얻을 수 없는 역참조해서는
보통 rvalue 는 literal 이거나 일시적(함수의 반환값이나 명시적 생성자 호출)이기 때문에역참조로는 얻을 수 없는 주소 값이다.
한 객체가 이 함수를 거침으로 그 객체를 참조하는 rvalue 가 얻어진다.

인자가 rvalue 일때 대다수의 표준 컴포넌트들은 move semantic 을 구현하여 객체의 asset 과properties 의 소유권을 직접적으로 복사 없이 전환한다. 

표준 라이브러리에서 이동(moving) 은 이동을 끝마친 원래의 객체는 유효하지만 지정되지 않은 상태로 남아있다.
이것은 그러한 operation 수행 이후에 이동된 객체의 값이 삭제되거나 새 값이 할당 되어야 함을 의미한다. 
그렇지 않으면 그것에 accessing 할 때 지정되지 않을 값을 생성(산출)한다.

- parameter
_arg_ : 객체

- return value
arg 를 참조하는 rvalue reference

- 예시
```c++
int main() {
	std::string foo = "foo-string";
	std::string bar = "bar-string";
	std::cout << "foo before push to myvector : " << foo << std::endl;
	std::cout << "bar before push to myvector with std::move : " << bar << std::endl;

	std::vector<std::string> myvector;

	myvector.push_back(foo);                    // copies
	myvector.push_back(std::move(bar));         // moves

	std::cout << "myvector contains:";
	for (std::string& x : myvector)
	{
		std::cout << ' ' << x;
	}
	std::cout << std::endl;

	std::cout << "foo : " << foo << std::endl;
	std::cout << "bar : " << bar << std::endl;

	return 0;
}

// 결과
foo before push to myvector : foo-string
bar before push to myvector with std::move : bar-string
myvector contains: foo-string bar-string
foo : foo-string
bar :
```

myvector 의 두번째 요소 자리에 std::move(bar) 를 대입하여 myvector 의 두번째 요소에 bar 의 모든 요소를 그대로 이동시키고 정작 bar 는 텅 빈 값만을 가지고 있게 된다.