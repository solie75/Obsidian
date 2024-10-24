Delegate 는 특정 이벤트나 함수를 바인딩하고 호출하는데 사용된다.
ExecuteIfBound() 함수는 Delegate 가 유효하게 바인딩 된 경우에만 호출한다.
-> Delegate 가 바인딩 되지 않았을 때 발생할 수 있는 오류를 방지할 수 있다.

```c++
DECLARE_DELEGATE_OneParam(FMyDelegate, int32);

class MyClass
{
public:
    FMyDelegate MyDelegate;

    void BindDelegate()
    {
        MyDelegate.BindUObject(this, &MyClass::MyFunction);
    }

    void MyFunction(int32 Value)
    {
        UE_LOG(LogTemp, Warning, TEXT("MyFunction called with value: %d"), Value);
    }
};

```

```c++
void CallDelegate()
{
    MyClass Instance;

    // Delegate 바인딩
    Instance.BindDelegate();

    // Delegate가 바인딩되어 있는 경우에만 호출
    Instance.MyDelegate.ExecuteIfBound(42);

    // 바인딩 해제
    Instance.MyDelegate.Unbind();

    // 바인딩이 해제된 상태에서 호출을 시도
    Instance.MyDelegate.ExecuteIfBound(42);  // 이 호출은 아무런 효과가 없습니다.
}

```

위의 코드에서 `Instance.MyDelegate.ExecuteIfBound(42);`는 Delegate가 유효하게 바인딩된 경우에만 `MyFunction`을 호출합니다. 바인딩이 해제된 경우에는 아무런 동작도 하지 않습니다.

