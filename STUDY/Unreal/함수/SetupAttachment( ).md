`SetupAttachment`는 Unreal Engine에서 컴포넌트를 다른 컴포넌트에 첨부(attach)할 때 사용되는 함수입니다. 이 함수는 컴포넌트 계층 구조를 설정하여 부모-자식 관계를 정의하고, 부모 컴포넌트의 위치, 회전, 스케일 등을 자식 컴포넌트와 동기화할 수 있게 합니다.

`SetupAttachment`는 주로 `AActor`의 생성자에서 호출되어, 생성된 컴포넌트들을 적절하게 연결합니다.

```c++
#include "MyActor.h"
#include "Components/StaticMeshComponent.h"
#include "Components/SphereComponent.h"

// AMyActor의 생성자
AMyActor::AMyActor()
{
    // 콜리전 컴포넌트 생성
    SphereComponent = CreateDefaultSubobject<USphereComponent>(TEXT("SphereComponent"));
    RootComponent = SphereComponent;

    // 메쉬 컴포넌트 생성 및 콜리전 컴포넌트에 첨부
    MeshComponent = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("MeshComponent"));
    MeshComponent->SetupAttachment(SphereComponent);
}
```

위 예제에서 `MeshComponent`는 `SphereComponent`에 첨부되어 `SphereComponent`가 부모 컴포넌트가 됩니다. 이렇게 하면 `SphereComponent`의 이동, 회전, 스케일이 `MeshComponent`에도 영향을 미치게 됩니다.

```c++
void SetupAttachment(USceneComponent* Parent);
```