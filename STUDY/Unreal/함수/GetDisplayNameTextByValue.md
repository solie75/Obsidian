특히 `UEnum` 클래스와 관련이 있습니다. 이 함수는 특정 열거형(enum) 값에 대응하는 표시 이름(display name)을 반환하는 데 사용됩니다. 열거형의 값이 숫자 또는 키로 저장되어 있지만, 이 값에 대응하는 사람이 읽을 수 있는 이름을 가져올 때 유용합니다

# 정의
```c++
FText UEnum::GetDisplayNameTextByValue(int64 InValue) const;
```

# 매개변수
`InValue`: 열거형 값입니다. `int64` 타입으로 주어지며, 이 값에 대응하는 표시 이름을 반환합니다.

# 반환값
`FText`: 주어진 열거형 값에 대응하는 표시 이름을 반환합니다. 반환된 텍스트는 로컬라이제이션(다국어 지원)을 포함할 수 있는 `FText` 형식입니다

# 예시
```c++
// 특정 UEnum 객체를 가져옵니다.
UEnum* MyEnum = FindObject<UEnum>(ANY_PACKAGE, TEXT("MyEnumType"));

if (MyEnum != nullptr)
{
    // 열거형 값에 대응하는 표시 이름을 가져옵니다.
    FText DisplayName = MyEnum->GetDisplayNameTextByValue(1);

    // 표시 이름을 출력합니다.
    UE_LOG(LogTemp, Log, TEXT("Display Name: %s"), *DisplayName.ToString());
}
else
{
    UE_LOG(LogTemp, Warning, TEXT("Enum not found"));
}

```
이 함수를 사용할 때는 특정 열거형 타입을 먼저 가져와야 합니다. 이는 보통 `FindObject` 또는 `StaticFindObject` 함수 등을 통해 수행됩니다. 열거형을 찾은 후, 열거형의 특정 값에 대응하는 사람이 읽을 수 있는 이름을 가져오는 데 `GetDisplayNameTextByValue`를 사용합니다.

** metadata 를 선언한 열거형 예시
```c++
UENUM(BlueprintType)
enum class EGameState : uint8
{
    GS_Starting UMETA(DisplayName = "Starting"),
    GS_Playing  UMETA(DisplayName = "Playing"),
};
```