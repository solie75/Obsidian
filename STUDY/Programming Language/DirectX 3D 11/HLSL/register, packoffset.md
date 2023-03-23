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

### 예시

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
