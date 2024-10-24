`TSubclassOf`는 Unreal Engine에서 특정 클래스의 서브클래스 타입을 안전하게 참조하기 위한 템플릿 클래스입니다. 주로 블루프린트 또는 코드에서 클래스를 변수로 참조할 때 사용됩니다. `TSubclassOf`는 특정 부모 클래스를 상속받는 모든 클래스를 가리킬 수 있으며, 이를 통해 타입 안전성을 유지하면서 다양한 클래스를 동적으로 참조하고 인스턴스화할 수 있습니다.

```c++
헤더파일

#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
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
    // UMyObjectClass 변수 선언
    UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "Config")
    TSubclassOf<class UMyObjectClass> MyObjectClass;
};

```

```c++
소스파일

#include "MyActor.h"
#include "MyObjectClass.h"

AMyActor::AMyActor()
{
    PrimaryActorTick.bCanEverTick = true;
}

void AMyActor::BeginPlay()
{
    Super::BeginPlay();

    if (MyObjectClass)
    {
        // MyObjectClass의 인스턴스를 생성
        UMyObjectClass* MyObjectInstance = NewObject<UMyObjectClass>(this, MyObjectClass);
        if (MyObjectInstance)
        {
            // 인스턴스가 성공적으로 생성된 경우, 추가 로직을 수행할 수 있습니다.
            MyObjectInstance->DoSomething();
        }
    }
}

```

- **`TSubclassOf` 정의**:
    
    - `TSubclassOf` 템플릿 클래스는 특정 클래스의 서브클래스 타입을 나타냅니다.
    - 예제에서는 `TSubclassOf<class UMyObjectClass>`로 `UMyObjectClass`의 서브클래스를 가리킬 수 있도록 했습니다.
- **`UPROPERTY` 매크로**:
    
    - `EditAnywhere`와 `BlueprintReadOnly`를 사용하여 에디터와 블루프린트에서 이 속성을 설정할 수 있습니다.
    - `Category`는 에디터에서의 속성 분류를 지정합니다.
- **인스턴스 생성**:
    
    - `NewObject<UMyObjectClass>(this, MyObjectClass)`를 사용하여 `MyObjectClass`의 인스턴스를 생성합니다.
    - 이 방법은 런타임 시점에서 클래스 타입을 동적으로 결정할 수 있게 해줍니다.

### `TSubclassOf`의 장점

- **타입 안전성**: 특정 클래스의 서브클래스만 참조할 수 있어 타입 안전성을 유지합니다.
- **유연성**: 런타임에 다양한 서브클래스를 동적으로 참조하고 인스턴스화할 수 있습니다.
- **에디터 및 블루프린트 통합**: 에디터와 블루프린트에서 클래스를 설정할 수 있어, 디자이너와 프로그래머 간의 협업이 원활합니다.