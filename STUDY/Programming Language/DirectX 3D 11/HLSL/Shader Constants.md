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
각 상수 버퍼는 벡터를 4096개 까지 가질 수 있다. 각 벡터는 네개의 32비트 값 까지를 포함 할 수 있다. 각 pipeline stage 에 14 개 까지의 constant buffer 를 바인딩 할 수 있다. (2개의 추가적인 slot 은 내부용으로 예약 되어있다.)

texture buffer 는 texture 과 같이 엑서스가 되는 특정된 buffer resouce이다. 
texture 접근 (buffer access 과 비교하여) 은 임의의 인덱싱된 데이터에 대해 더 높은 성능을 낼 수 있다. 128개 가지의 texture buffer 를 각 pipeline 에 바인딩 할 수 있다. 

buffer resource 는 shader constant 의 세팅의 overhead 를 최소화 하도록 디자인 되었다.  constant 와 texture buffer 의 업데이트 를 관리하는데 effect framework  (see [**ID3D10Effect Interface**](https://learn.microsoft.com/en-us/windows/desktop/api/d3d10effect/nn-d3d10effect-id3d10effect))를 사용하거나 Direct3D API (see [Copying and Accessing Resource Data (Direct3D 10)](https://learn.microsoft.com/en-us/windows/desktop/direct3d10/d3d10-graphics-programming-guide-resources-mapping) 를 사용할 수 있다. 응용프로그램은 또한 데이터를 다른 버퍼(render target 또는 stream-output target 과 같은)에서 constant buffer 로 복사 할 수 있다.

For morel info on using constant buffers in a D3D11 application, see [Introduction to Buffers in Direct3D 11](https://learn.microsoft.com/en-us/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro) and [How to: Create a Constant Buffer](https://learn.microsoft.com/en-us/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-constant-how-to).

constant buffer 는 pipeline 에 바인딩 되는 것에 view 를 필요로 하지 않는다. 그러나 texture buffer 는 view 를 요구하고 texture slot 에 바운딩 되어야 한다. (또는 effect 를 사용할 때 [**SetTextureBuffer**](https://learn.microsoft.com/en-us/windows/desktop/api/d3d10effect/nf-d3d10effect-id3d10effectconstantbuffer-settexturebuffer) 를 사용하여 바운딩 되어야 한다. )

constant data 를 pack 하는 데에는 두가지 방법이 있다.
1. [register (DirectX HLSL)](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-variable-register)
2. [packoffset (DirectX HLSL)](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-variable-packoffset)

# constant buffer 의 구조

constant buffer 는 개별적으로 각 constant 를 커밋하기 위한 개별적인 호출을 하는 것보다  shader constant가 그룹화 되고 동시에 커밋 되는 것을 허용함으로써 shader constant 가 update(갱신)되는 것을 요구하는 bandwidth(대역폭)를 줄인다.

constant buffer를 효율적으로 사용하는 가장 좋은 방법은 shader 변수들을  업데이트 빈도에 따라 constant buffer 를 구성하는 것이다. 이것은 응용프로그램이 shader constant 를 요구하는 bandwidth(대역폭) 를 최소화 하도록 (허용) 한다. 예를 들어 shader 가 두 개의 constant buffer 를 선언하고, 각 업데이트 빈도에 따라 구성할 수 있다. 개체별로 업데이트 되어야 하는 데이터 (예를 들어 world matrix) 는 constant buffer로 그룹화 되며 각 개체에 대해 업데이트 된다. 이것은 장면을 특정 짓는 데이터와 별개이고 따라서 업데이트 빈도가 훨씬 덜해질 가능성이 높다.

```c++
cbuffer myObject
{       
    float4x4 matWorld;
    float3   vObjectPosition;
    int      arrayIndex;
}
 
cbuffer myScene
{
    float3   vSunPosition;
    float4x4 matView;
}
```

# 기본 constant buffers

$Global and $Param. 두 가지의 default constant buffer 가 사용가능하다. 전역 범위의 변수는 cbuffer 에 사용되는 것과 동일한 packing method (압축 방법)을 사용하여 암시적으로 $Global cbuffer 에 추가된다. 함수의 매개변수 리스트 내의 균일한 (uniform) 매개변수는 shader rk effect framework 외부에서 컴파일 될 때 $Param constant buffer 에 나타난다. effect framework 안에서 컴파일 될 때 모든 uniform 은 전역으로 정의된 변수 에 resolve 되어야 한다.

전역 범위에 있는 변수는 cbuffer에 사용되는 것과 동일한 압축 방법을 사용하여 암시적으로 $Global cbuffer에 추가됩니다. 함수의 매개변수 목록에 있는 균일 매개변수는 효과 프레임워크 외부에서 셰이더를 컴파일할 때 $Param 상수 버퍼에 나타납니다. 효과 프레임워크 내에서 컴파일될 때 모든 유니폼은 전역 범위에 정의된 변수로 해석되어야 합니다.

register란 무엇인가. starting register 란 무엇인가
swizzling 이란 무엇인가. (https://chulin28ho.tistory.com/666) -> hlsl 책으로 더 알아보고 hlsl 항목에 정리할 것

https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-constants



