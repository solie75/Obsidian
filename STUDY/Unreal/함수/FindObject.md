언리얼 엔진에서 `FindObject` 함수는 주어진 클래스 타입과 이름을 기반으로 특정 오브젝트를 찾는 데 사용됩니다. 이 함수는 `UObject` 클래스의 템플릿 함수로, 게임 내에서 특정 오브젝트를 동적으로 검색하고자 할 때 유용합니다.

# 정의
```c++
template< class T > T* FindObject( UObject* Outer, const TCHAR* Name, bool ExactClass = false );
```

# 매개변수
- `T` : 찾고자 하는 오브젝트의 클래스 타입입니다.
- `Outer` : 오브젝트의 외부 컨테이너입니다. 주로 패키지나 레벨을 가리킵니다. `nullptr`로 지정하면 모든 오브젝트를 검색합니다.
- `Name` : 찾고자 하는 오브젝트의 이름입니다.
- `ExactClass` : `true`로 설정하면 정확히 지정한 클래스 타입과 일치하는 오브젝트만 반환합니다. `false`로 설정하면 상속된 클래스 타입도 포함됩니다.

# 반환값
`T*` : 찾은 오브젝트의 포인터를 반환합니다. 만약 오브젝트를 찾지 못하면 `nullptr`를 반환합니다.

# 예시

```c++
// 특정 패키지 내에서 이름이 "MyActor"인 AActor 클래스를 찾습니다.
AActor* MyActor = FindObject<AActor>(ANY_PACKAGE, TEXT("MyActor"));

if (MyActor != nullptr)
{
    // 오브젝트를 찾았을 때 수행할 작업
    UE_LOG(LogTemp, Log, TEXT("Actor found: %s"), *MyActor->GetName());
}
else
{
    // 오브젝트를 찾지 못했을 때 수행할 작업
    UE_LOG(LogTemp, Warning, TEXT("Actor not found"));
}
```

# 주의사항

- **성능**: `FindObject`는 오브젝트를 이름으로 검색하기 때문에 성능에 영향을 미칠 수 있습니다. 따라서 자주 호출되는 코드에서는 주의가 필요합니다.
- **정확한 이름**: 오브젝트 이름은 대소문자를 구분합니다. 따라서 정확한 이름을 제공해야 합니다.
- **Outer**: 검색 범위를 제한하기 위해 적절한 `Outer`를 지정하는 것이 좋습니다. `ANY_PACKAGE`를 사용하면 모든 패키지를 검색하지만, 이는 성능에 영향을 미칠 수 있습니다.

