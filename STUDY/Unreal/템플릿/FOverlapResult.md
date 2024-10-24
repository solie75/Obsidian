`FOverlapResult`는 Unreal Engine에서 콜리전 검사나 오버랩 이벤트를 처리할 때 사용되는 구조체입니다. 오버랩 이벤트는 하나의 콜리전 객체가 다른 콜리전 객체와 겹칠 때 발생하며, `FOverlapResult`는 이러한 겹침에 대한 정보를 제공합니다. 이를 통해 개발자는 겹친 객체에 대한 세부 정보를 쉽게 접근할 수 있습니다.

```c++
struct FOverlapResult
{
    /** Actor that the check hit. */
    TWeakObjectPtr<AActor> Actor;

    /** PrimitiveComponent that the check hit. */
    TWeakObjectPtr<UPrimitiveComponent> Component;

    /** Identifier of the overlapping item of the Component. */
    int32 ItemIndex;

    /** Whether the overlap has an actual blocking hit. */
    bool bBlockingHit;

    // Constructors
    FOverlapResult()
        : ItemIndex(INDEX_NONE)
        , bBlockingHit(false)
    {
    }
};

```

- **Actor**:
    
    - 오버랩된 액터에 대한 약한 참조 포인터입니다. 오버랩된 객체가 액터일 경우 이 멤버를 통해 해당 액터에 접근할 수 있습니다.
- **Component**:
    
    - 오버랩된 프리미티브 컴포넌트에 대한 약한 참조 포인터입니다. 오버랩된 객체가 컴포넌트일 경우 이 멤버를 통해 해당 컴포넌트에 접근할 수 있습니다.
- **ItemIndex**:
    
    - 컴포넌트 내에서 오버랩된 항목의 인덱스를 나타냅니다. 주로 복합 메시나 여러 충돌 요소를 가진 컴포넌트에서 사용됩니다.
- **bBlockingHit**:
    
    - 오버랩이 실제로 블로킹(hit)인지 여부를 나타내는 불리언 값입니다.


