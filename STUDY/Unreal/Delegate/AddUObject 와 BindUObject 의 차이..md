# BindUObject

BindUObject 는 UObject 의 맴버 함수를 Delegate 에 바인딩할 때 사용된다. 이때 바인딩된 멤버함수는 Delegate 가 호출될 때 실행된다.
파라미터로 받은 UObject 가 가비지 컬렉션이 될 때 자동으로 연결을 해제한다. 바인딩된 UObject 가 삭제되면 Delegate 바인딩도 자동으로 해제된다.

```c++
// Delegate 선언
DECLARE_DELEGATE_OneParam(FMyDelegate, int32);

// TESTA 클랫의 멤버 함수
void MyFunction(int32 Data)
{
	UE_LOG(LogTemp, Warning, Text("Data Received : %d"), Data);
}

// Delegate 에 바인딩.
FMyDelegate MyDelegate;
MyDelegate.BindUObject(this, &TestA::MyFunction);
```

# AddUObject

AddUObject 는 multicast delegate 에서 사용된다. 여러 리스너를 하나의 delegate 에 추가할 때 사용된다.
일반적으로 AddUObject 는 'MULTICAST' delegate 와 함께 사용된다.

```c++
// Delegate 선언
DECLARE_DYNAMIC_MULTICAST_DELEGATE_OneParam(FOnDataChanged, int32, Data);

// TESTB 클래스의 멤버 함수
void OnDataChanged(int32 Data)
{
	UE_LOG(LogTemp, Warning, Text("Data Received : %d"), Data);
}

// Delegate 에 바인딩.
FOnDataChanged OnDataChanged;
OnDataChanged.AddUObject(this, &TestB::OnDataChanged);
```