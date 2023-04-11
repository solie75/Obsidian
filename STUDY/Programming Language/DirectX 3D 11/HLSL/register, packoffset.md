# register

다음의 구문을 사용하는 특정 register 에 shader 변수를 할당하기 위한 선택적 키워드
```HLSL
: register([shader_profile], Type#[subcomponent])
```

### parameter

- register

요구되는 키워드

[shader_profile]

선택적 shader profile 로 shader target 이나 간다히는 ps 또는 vs 가 될 수 잇다.

- Type#[subcomponent]

register type, number, subcomponent devlaration

type 은 다음 중 한가지이다.

|Type|Register Description|
|---|---|
|b|constant buffer|
|t|texture and texture buffer|
|c|buffer offset|
|s|sampler|
|u|unordered access view|

'#' 는 register 의 정수로된 number 이다.
subcomponent 는 선택적 정수 number 이다.

### Remarks

같은 변수 선언데 한개 이상의 register 할당을 공백으로 구분하여 추가할 수 있다.

register 는 그래픽 프로그래밍에서 사용되는 데이터 타입을 레지스터에 저장하거나 레지스터에서 읽어들이기 위한 예약된 키워드이다. 즉, register를 사용하여 쉐이더 내에서 레지스터에 데이터를 저장하거나 이전에 저장된 데이터를 읽어들일 수 있다. 레지스터는 그래픽 카드의 하드웨어 자원으로 써, 쉐이더 프로그램이 실행될 때 사용된다. 이를 통해 쉐이더가 더욱 빠르고 효율적으로 실행되도록 할 수 있다. 예를 들어 vertex shader 에서는 정점 좌표와 색상 정보를 레지스터에 저장하고, fragment shader 에서는 계산된 생상 값을 레지스터에 저장하여 화면에 표시할 픽셀의 색상 값을 결정하는데 사용된다.

register 는 하드웨어에서 사용되는 개념으로 물리적인 모습은 존재하지 않는다. 대신 그래픽 카드의 레지스터 파일에 저장되며, 레지스터 파일은 매우 빠른 속도로 데이터를 저장하고 검색할 수 있는 고속 메모리이다. 또한 레지스터 파일은 그래픽 카드의 프로세서에 의해 직접적으로 접근되기 때문에 처리 속도가 매우 빠르다.
HSLS 에서 register 는 데이터 타입을 지정하고, 해당 데이터를 레시스터 파일에 할당하는데 사용된다. 

register 키워드는 GPU 의 레지스터 파일에서 각각 할당된 레지스터의 세트를 나타내는 것으로 메모리 주소와는 다른 개념이다.

CPU 에서 사용하는 일반적인 메모리 주소와는 다르게, GPU 의 레지스터 주소는 하드웨어에 의해 직접적으로 관리되므로 프로그래머는 GPU에서의 레지스터에 접근하기 위해 메모리 주소를 사용하지 않는다. 대신, 레지스터 세트와 해당 세트 내의 레지스터 번호를 사용하여 GPU에 접근한다.

따라서 register 키워드를 사용하여 HLSL 에서 변수를 레지스터에 할당할 때, 해당 변수가 GPU 에서의 어떤 레지스터 세트와 레지스터 번호에 할당될 지 정하게 된다. 이렇게 하면 프로그래머는 변수가 어디에 저장되는지를 직접 제어할 수 있으므로, 성능을 최적화 할 수 있다.

GPU 의 레지스터 파일이란 GPU 내부에서 레지스터를 저장하는 데 사용되는 하드웨어 구성 요소이다. CPU 의 레지스터와 마찬가지로 GPU  의 레지스터 파일은 빠른 데이터 엑세스를 위해 설계되어 있다. GPU 의 레지스터 파일은 일반적으로 수백 개의 레지스터를 저장 할 수 있으며, 대부분의 GPU 아키텍처는 레지스터 파일을 복수의 레지스터 세트로 구성한다. 이러한 레지스터 세트는 일반적으로 동일한 유형의 데이터를 저장하며, 각 레지스터 세트는 레지스터 번호를 기반으로 참조된다. 레지스터 파일은 GPU 내부에서 엑세스 하기 때문에, 레지스터 액서스는 메모리 액세스 보다 훨씬 빠르다.

C++ 변수를  CPU의 메모리를 사용하여 저장할 때, 그 메모리를 특정하지 않는 것과는 다르게 HLSL 에서 변수를 register에 직접 할당하여 GPU의 레지스터 파일 내부의 특정한 레지스터를 사용하도록 지정할 수 있다. 하지만 레지스터 파일의 크기는 제한되어 있기 때문에, 모든 변수를 레지스터에 할당할 수 는 없다. 따라서 프로그래머는 적절한 변수를 레지슽에 할당하여 성능을 최적화 할 필요가 있다. 또한 register 를 사용하여 변수를 레지스터에 할당하는 것은 성능을 향상시키지만 코드의 복잡도를 증가시킬 수 있으므로 적절한 사용이 필요하다.

- 레지스터 파일의 종류

1. 벡터 레지스터 파일(Vector Register FIle) : 벡터 연산에 사용되는 레지스터 파일로 4차원 벡터 형태의 데이터를 저장할 수 있다. 이 벡터 레지스터 파일은 GPU 아키텍처마다 크기가 다르지만, 일반적으로 128~1024개의 벡터 레지스터를 가지고 있습니다.
2. 스칼라 레지스터 파일(Scalar Register FIle) : 스칼라 연산에 사용되는 레지스터 파일로, 스칼라 형태의 데이터를 저장할 수 있다. 벡터 레지스터와 마찬가지로 GPU 아키텍처마다 크기가 다르지만, 일반적으로 32~512개의 스칼라 레지스터를 가지고 있습니다.
3. 텍스처 레지스터 파일(Texture Register File) : 텍스처 매핑에 사용되는 레지스터 파일로, 텍스처 좌표 등의 데이터를 저장할 수 있다. 이 레지스터 파일은 GPU 아키텍처마다 크기가 다르지만, 일반적으로 32~128개의 텍스처 레지스터를 가지고 있습니다.
4. 상수 레지스터 파일(Constant Register File) : 상수 데이터를 저장하기 위한 레지스터 파일로, 쉐이더에서 고정된 값을 사용할 때 주로 사용된다. 이 레지스터 파일은 GPU 아키텍처마다 크기가 다르지만, 일반적으로 32~256개의 상수 레지스터를 가지고 있습니다. 

- 레지스터 크기
GPU 의 레지스터 크키는 bit 단위로 표현된다. 레지스터는 고정 크기의 메모리 블록으로 구성되어 있으며, 일반적으로 32비트 또는 64비트 크기로 구성된다. 이는 GPU 아키텍처에 따라 다르며, 최근에 출시된 GPU의 경우 32 비트 레지스터를 사용하는 것이 일반적이다.

예를 들어, NVIDIA의 CUDA 아키텍처에서는 레지스터의 크기가 32비트이며, 32개의 레지스터를 가지는 스레드 블록(thread block)이라는 실행 단위가 있습니다. 이는 CUDA 프로그래밍에서 중요한 개념 중 하나입니다. 또한, AMD의 GCN 아키텍처에서는 64비트 벡터 연산을 지원하는 레지스터가 사용되고 있습니다.

### 예시

```hlsl
cbuffer Constants : register(b0)
{
    float4x4 viewProjMatrix;
    float3 lightDir;
}
```
위 코드에서 register(b0)는 상수 버퍼 Constants를 GPU 의 상수 레지스터 세트 중 첫 번재 세트에 할당하도록 지정하는 것이다. 

```
sampler myVar : register(ps_5_0, s);
```

```
sampler myVar : register(vs, s[8]);
```

```
sampler myVar : register(ps, s[2])
              : register(ps_5_0 , s[0])
	          : register(vs, s[8]);
```


# packoffset

다음의 구문을 따르는 선택적 shader constant pack 키워드

```
: packoffset(c[Subcomponent][.component])
```

### parameters

|item|description|
|---|---|
|packoffset|요구되는 키워드|
|c|constant register 에만 적용되는 packing|
|$[subcomponent][.component]$|선택적 subcomponent 와 component. subcomponent 는 정수인 register number이다. component 는 [.xyzw]형식이다. |

### Remark

변수 유형을 정의할 때 수동으로 shader constant 를 pack 하기 위해서 이 키워드를 사용한다. constant 를 pack 할 때, constant type 를 섞을 수 없다.

- global constant. global 변수는 컴파일러에 의해서 $Global cbuffer 에 global constant 로써 추가된다. 자동적으로 pack 되는 요소들은 마지막 수동으로 pack 되는 변수 후에 나타난다. global constant 들을 packing 할 때  유형을 섞을 수 있다.
- uniform constant. 함수의 매개변수 리스트의 uniform parameter 는 컴파일러에 의해 $Param constant buffer 에 추가된다. effect framework 에 컴파일 되는 경우 uniform constant 는 전역 변수로 정의된 uniform 변수에 resolve 되어야 한다. uniform constant 는 수동적으로 offset 될 수 없다. 권장 사용은 응용 프로그램 데이터를 셰이더에 전달하는 수단이 아니라 전역 변수로 다시 별칭을 지정하는 셰이더의 특수화에만 사용하는 것이다.

### 예시

여기 shader constant 를 수동으로 pack 하는 몇가지 예시가 있다.
register 경계를 넘지 않도록 크기가 충분히 큰 벡터 및 스칼라의 subcomponent를 압축합니다. 예시로 다음이 유효하다.

```
cbuffer MyBuffer 
{ 
  float4 Element1 : packoffset(c0); 
  float1 Element2 : packoffset(c1); 
  float1 Element3 : packoffset(c1.y); 
}
```
