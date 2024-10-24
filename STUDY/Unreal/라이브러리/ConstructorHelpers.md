# ConstructorHelpers::FObjectFinder

Unreal Engine에서 특정 경로에 있는 자산을 찾고 로드하는 데 사용되는 유틸리티 이다.
주로 생성자에서 사용되며, 에디터와 런타임 모두에서 특정 자산을 쉽게 로드할 수 있도록 도와준다

# ConstructorHelpers::FClassFinder

Unreal Engine에서 특정 경로에 있는 블루프린트 클래스나 C++ 클래스를 찾고 로드하는 데 사용되는 유틸리티
주로 클래스의 생성자에서 사용되며, 자산을 쉽게 로드하고 초기화할 수 있게 해줍니다.
```c++
static ConstructorHelpers::FClassFinder<APawn> PawnClassFinder(TEXT("/Game/Blueprints/MyPawnBlueprint.MyPawnBlueprint_C"));
```
`FClassFinder`는 템플릿 클래스로, 특정 타입(`APawn`, `ACharacter`, `UUserWidget` 등)의 클래스를 찾도록 지정할 수 있습니다. 경로는 `TEXT` 매크로를 사용하여 유니코드 문자열로 제공됩니다. <span style="color:red"> 여기서 `_C`는 블루프린트 클래스의 컴파일된 버전을 나타냅니다. </span>
\_C 는 블루프린트 클래스를 로드할 때 붙이고 다른 기본적인 자산 (Static Mesh, Skeletal Mesh, Meterial 등) 을 로드할 때에는 붙이지 않는다.