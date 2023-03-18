Shader Moder 4 에서 shader constant 는 메모리의 하나 이상의 buffer resource 에 저장된다. shader constant는 두 가지 타입의 버퍼로 구성될 수 있다 : constant buffers (cbuffers) 그리고 texture buffer (tbuffer).

constant buffer 는 엑세스 대기 시간(latency access)이 짧고 CPU 에서 더 자주 업데이트 되는 constant-variable usage 에 최적화 되어있다. 이러한 이유로, 추가적인 사이즈, layout, 접근 제한 은 이러한 리소스에 적용된다.
Texture buffer 는 texture 와 같이 접근되고 임의적인 인덱싱된 데이터에 대하여 더 뛰어나게 수행한다. 사용하는 리소스 타입이 무엇이던지 간에, 응용프로그램이 생성할 수 있는 contant buffer 또는 texture buffer 의 개수에는 제한이 없다.

constant buffer 또는 texture buffer 를 선언하는 것은 수동으로 **register** 를 할당하거나 데이터를 압축(packing) 하기 위한 **register** 및 **packoffset** 키워드가 추가된 C 에서의 구조체 선언과 매우 닮았다. 

# Parameters

- BufferType
|BufferType|Description|
|---|---|
|cbuffer|constant buffer|
|tbuffer|texture buffer|

- Name
선택적으로 고유의 buffer 명을 포함하는 아스키 문자열

**register(b#)**

constant data 를 수동적으로 압축(pack) 하는데 사용되는 선택적 키워드이다. constant 는 오직 시작 register 가 register number (#) 로 주어지는 constant buffer 의 register 에만 압축(pack)될 수 있다. 

- VariableDeclaration (변수 선언)

Variavle declaration 은 구조체의 맴버를 선언하는 것과 비슷하다. 이것은 HLSL type 또는 effect object 중 아무거나 될 수 있다. (texture 또는 sampler object 제외)

**packoffset(c#.xyzw)**

constant data 를 수동적으로 압축(pack) 하는데 사용되는 선택적 키워드이다. constant 는 register number 가 (#) 로 주어지는 constant buffer 에 압축(pack) 될 수 있다. Sub-component packing (xyzw swizzling을 사용하는) 은 크기가 단일 register 에 딱 맞는 (register 범위를 넘지 않음) constant 에 사용할 수 있다.  예를 들어, float4 는 y-component 로 시작하는 단일 register 에 pack 될 수 없다. 왜냐하면 그것이 four-component register 에 맞지 않기 때문이다.

# Remarks

constant buffer 는 개별적으로 각 constant 를 커밋하기 위한 개별적인 호출을 하는 것보다  shader constant가 그룹화 되고 동시에 커밋 되는 것을 허용함으로써 shader constant 가 update(갱신)되는 것을 요구하는 bandwidth(대역폭)를 줄인다.

constant buffer 는 buffer 와 같이 접근되는 특정된 buffer resource 이다.


register란 무엇인가. starting register 란 무엇인가
swizzling 이란 무엇인가. (https://chulin28ho.tistory.com/666) -> hlsl 책으로 더 알아보고 hlsl 항목에 정리할 것

https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-constants



CEntity.h 에서

~CEntity() 가 virtual 로 선언되는 이유
-> 모든 object 들의 소멸자 순서는 생성과 ?반대로 되어야 하기 때문에?

 g_iNextID 는 어떤 용도로 쓰는 것이고 왜 static 으로 선언한거지?
->g_iID 는 모든 CEntity를 상속받은 클래스들에 부여된다. 이때 생성되는 자식 클래스 순서대로 다른 고유의 ID 를 가져야 하며 이를 위한 방법으로 ID 를 0으로 초기화 하고 CEntity 를 상속받는 클래스가 생성될 때마다 그 ID 에 1을 더하는 것이다. 따라서 g_iNextID 는 생성자에서 사용되어 그 전에 생성된 entity 자식 클래스보다 1이 더큰 고유 아이디를 갖게 된다.

 m_strName 은 그냥 wstring형 인데 왜 SetName 함수의 인자 _strName 은 const wstring& 형 인가?
->

virtual CEntity* Clone() = 0; 순수가상함수 선언으로 모든 자식 클래스는 클론 함수를 필수로 가져야 한다. 왜 모든 자식 클래스는 Clone() 함수를 가져야 하는다 이때 CEntity가 아닌 CEntity*인 이유는 무엇인가.

-> CEntity의 생성자 중 매개변수로 const CEntity& _other 를 받는 rjtdms 그 _other 로부터 똑같은 m_strName 을 얻고 g_iNextID 는 상관없이 증가되어 생성된다. 이때 _other 는 변형이 없어야 됨으로 const CEntity& 형으로 복사하고자 하는 entity 자식 클래스로 정의된 객체의 주소를 가져온다.
