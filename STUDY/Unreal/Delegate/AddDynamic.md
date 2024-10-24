`AddDynamic`는 Unreal Engine에서 델리게이트(Delegate)에 함수를 바인딩하기 위해 사용되는 매크로입니다. 이 매크로는 주로 이벤트 시스템에서 특정 이벤트가 발생했을 때 호출될 함수를 지정하는 데 사용됩니다. `AddDynamic`를 사용하면 C++ 클래스의 멤버 함수를 델리게이트에 연결하여, 런타임에 이벤트가 발생했을 때 자동으로 해당 함수를 호출하도록 할 수 있습니다.

`AddDynamic`는 델리게이트 시스템을 사용하여 이벤트와 콜백 함수를 연결하는 방법을 제공합니다. 이를 통해 다양한 이벤트에 대해 동적으로 반응하는 코드를 작성할 수 있습니다.

델리게이트는 Unreal Engine에서 다양한 유형의 이벤트와 콜백을 처리하는 데 사용됩니다. `AddDynamic`는 주로 `DECLARE_DYNAMIC_MULTICAST_DELEGATE`와 함께 사용됩니다.

## 예제

`AddDynamic`를 사용하여 오버랩 이벤트에 대한 콜백 함수를 설정하는 예제

```c++
// MyActor.h
#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
#include "Components/SphereComponent.h"
#include "MyActor.generated.h"

UCLASS()
class MYPROJECT_API AMyActor : public AActor
{
    GENERATED_BODY()
    
public:
    AMyActor();

protected:
    virtual void BeginPlay() override;

public:
    UPROPERTY(VisibleAnywhere)
    USphereComponent* SphereComponent;

    UFUNCTION()
    void OnOverlapBegin(
        UPrimitiveComponent* OverlappedComp,
        AActor* OtherActor,
        UPrimitiveComponent* OtherComp,
        int32 OtherBodyIndex,
        bool bFromSweep,
        const FHitResult& SweepResult);
};

```

```c++
// MyActor.cpp
#include "MyActor.h"
#include "Components/SphereComponent.h"
#include "GameFramework/Actor.h"

AMyActor::AMyActor()
{
    PrimaryActorTick.bCanEverTick = true;

    // 콜리전 컴포넌트 생성 및 초기화
    SphereComponent = CreateDefaultSubobject<USphereComponent>(TEXT("SphereComponent"));
    RootComponent = SphereComponent;

    // 오버랩 이벤트 바인딩
    SphereComponent->OnComponentBeginOverlap.AddDynamic(this, &AMyActor::OnOverlapBegin);
}

void AMyActor::BeginPlay()
{
    Super::BeginPlay();
}

void AMyActor::OnOverlapBegin(
    UPrimitiveComponent* OverlappedComp,
    AActor* OtherActor,
    UPrimitiveComponent* OtherComp,
    int32 OtherBodyIndex,
    bool bFromSweep,
    const FHitResult& SweepResult)
{
    if (OtherActor && (OtherActor != this) && OtherComp)
    {
        // 오버랩 시 수행할 동작
        UE_LOG(LogTemp, Warning, TEXT("Overlap with %s"), *OtherActor->GetName());
    }
}

```

- 매개변수 
	- **this**: 현재 객체의 포인터로, 이벤트가 발생했을 때 호출될 함수를 포함하는 객체입니다.
	- **&AMyActor::OnOverlapBegin**: 델리게이트에 바인딩할 멤버 함수의 포인터입니다. 이 함수는 이벤트가 발생했을 때 호출됩니다.

- 특징 및 장점
	- **동적 바인딩**: `AddDynamic`를 사용하면 런타임에 이벤트와 함수를 동적으로 연결할 수 있습니다. 이는 코드의 유연성과 재사용성을 높입니다.
	- **안전성**: Unreal Engine의 델리게이트 시스템은 함수 포인터와 달리 안전성을 제공하며, 객체가 삭제되었을 때 자동으로 바인딩을 해제합니다.
	- **유연성**: 다양한 이벤트와 콜백 함수를 쉽게 연결할 수 있어, 복잡한 이벤트 처리 로직을 간단하게 구현할 수 있습니다.

- 주의 사항
	- **UFUNCTION 매크로**: `AddDynamic`로 바인딩할 함수는 반드시 `UFUNCTION` 매크로로 선언되어야 합니다. 그렇지 않으면 컴파일 오류가 발생합니다.
	- **동일한 서명**: 바인딩할 함수는 델리게이트의 서명(매개변수와 반환 타입)과 동일해야 합니다.



`AddDynamic`는 Unreal Engine에서 델리게이트에 함수를 동적으로 바인딩하는 데 사용되는 중요한 매크로입니다. 이를 통해 이벤트와 콜백 함수를 연결하여 런타임에 다양한 이벤트에 반응하는 로직을 구현할 수 있습니다. `UFUNCTION` 매크로와 함께 사용하여 안전하고 유연한 이벤트 처리를 할 수 있습니다.