`OnComponentBeginOverlap`는 Unreal Engine에서 특정 컴포넌트가 다른 컴포넌트와 충돌(오버랩)할 때 호출되는 이벤트 델리게이트입니다. 이 이벤트는 주로 콜리전 감지와 관련된 로직을 처리하는 데 사용됩니다. 예를 들어, 플레이어 캐릭터가 트리거 영역에 들어갔을 때 특정 동작을 수행하도록 설정할 수 있습니다.

`OnComponentBeginOverlap`는 특정 콜리전 컴포넌트가 다른 콜리전 컴포넌트와 겹치는 순간 호출됩니다. 이 이벤트는 콜리전 처리, 게임 로직, 상호작용 등을 구현하는 데 유용합니다.

```c++
// UprimitiveComponent
DECLARE_DYNAMIC_MULTICAST_DELEGATE_SixParams(
    FComponentBeginOverlapSignature,
    UPrimitiveComponent*, OverlappedComponent,
    AActor*, OtherActor,
    UPrimitiveComponent*, OtherComp,
    int32, OtherBodyIndex,
    bool, bFromSweep,
    const FHitResult&, SweepResult);

```

## 예시

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

    UFUNCTION() //...ㄱ)
    void OnOverlapBegin(
        UPrimitiveComponent* OverlappedComp,
        AActor* OtherActor,
        UPrimitiveComponent* OtherComp,
        int32 OtherBodyIndex,
        bool bFromSweep,
        const FHitResult& SweepResult);
};
```
..ㄱ) OnComponentBeginOverlap 이 Dynamic Delegate 이기 때문에 UFUNCTION() 을 선언해주어야 한다.

```c++
// MyActor.cpp
#include "MyActor.h"
#include "GameFramework/Actor.h"
#include "Components/SphereComponent.h"

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
	- **OverlappedComp**: 오버랩된 콜리전 컴포넌트입니다. 이벤트가 발생한 컴포넌트입니다.
	- **OtherActor**: 오버랩된 다른 액터입니다.
	- **OtherComp**: 오버랩된 다른 액터의 콜리전 컴포넌트입니다.
	- **OtherBodyIndex**: 다른 몸체의 인덱스입니다.
	- **bFromSweep**: 이 충돌이 스위핑(sweeping)으로 인해 발생했는지 여부를 나타냅니다.
	- **SweepResult**: 스위핑 충돌의 결과를 포함하는 히트 결과 구조체입니다

- 특징 및 장점
1. **실시간 상호작용 감지**: `OnComponentBeginOverlap` 이벤트는 실시간으로 상호작용을 감지하여, 게임 내에서 즉각적인 반응을 구현할 수 있습니다.
2. **다양한 활용성**: 트리거 영역, 상호작용 오브젝트, 게임 플레이 메커니즘 등을 구현하는 데 사용됩니다.
3. **유연한 이벤트 처리**: 이벤트 델리게이트를 사용하여 다양한 객체와 상호작용할 때 필요한 로직을 유연하게 처리할 수 있습니다.