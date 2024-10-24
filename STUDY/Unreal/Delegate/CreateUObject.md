Unreal Engine 의 Delegate 는 이벤트 및 콜백 시스템을 구현하는 데 사용된다. Delegate 는 특정 함수나 메서드에 대한 참조를 유지하고, 나중에 필요할 때 해당 함수를 호출할 수 있도록 한다. 
CreateUObject 함수는 델리게이트와 함께 사용되는 함수로, 주로 특정 UObject에 속한 멤버 함수를 델리게이트로 바인딩할 때 사용된다.
해당 함수를 사용하면 특정 클래스의 멤버 함수가 이벤트가 발생했을 때 호출되도록 할 수 있다.

```c++
template <typename UserClass, typename FuncType>
inline FDelegateHandle CreateUObject(UserClass* InUserObject, FuncType InFunc)
```

- **UserClass**: UObject를 상속하는 클래스 타입.
- **InUserObject**: 함수가 호출될 때 사용할 객체의 인스턴스.
- **InFunc**: 바인딩할 멤버 함수 포인터.

FDelegateHandle 형을 반환한다. 해당 핸들은 델리게이트에 대한 참조를 유지하고, 나중에 해당 델리게이트를 제거하거나 관리할 때 사용할 수 있다.