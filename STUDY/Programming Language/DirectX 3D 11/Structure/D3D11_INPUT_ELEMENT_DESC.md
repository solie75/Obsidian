input-assembler stage 에 대한 단일 요소의 설명이다.

## Syntax

```c++
typedef struct D3D11_INPUT_ELEMENT_DESC { 
	LPCSTR SemanticName; 
	UINT SemanticIndex; 
	DXGI_FORMAT Format; 
	UINT InputSlot; 
	UINT AlignedByteOffset; 
	D3D11_INPUT_CLASSIFICATION InputSlotClass; 
	UINT InstanceDataStepRate; 
} D3D11_INPUT_ELEMENT_DESC;
```

## Parameters

1. LPCSTR SemanticName 
	shader input-[[Signatures]]의 요소와 관련된 HLSL semantic.

2. UINT SemanticIndex 

	요소에 대한 semantic 인덱스. semantic index 는 정수 인덱스 수로 semantic 을 수정한다. semantic index 는 오직 같은 semantic을 가진 요소가 둘 이상일 경우에만 필요하다. 예를 들어, 4x4 matrix 는 네개의 컴포넌트가 각각  semantic name 을 갖는다. 그러나 네개의 컴포넌트 각각은 다른 semantic 인덱스들을 가지고 있다. 
-> 같은 SemanticName을 여러 번 정의할 수 있다. 이때는 인덱스로 분류하며 사용될 때에는 SemanticName 뒤에 그 인덱스를 붙인다. 그리고 SemanticIndex는 해당 SemanticName이 여러번 정의 되었을 경우 자신이 몇번째 인덱스인지를 정수로 표현한다.

3. DXGI_FORMAT Format 
	element data의 데이터 유형. ( [DXGI_FORMAT](https://learn.microsoft.com/en-us/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)) semantic의 크기. 

4. UINT InputSlot 
	input-assembler 를 확인(식별)하는 정수 값. 유효한 값은 0과 15 사이이다. 예를 들어 위치와 색상 성분으로 구성된 정점의 경우 두 종류의 겅분들을 하나의 입력 슬롯을 통해서 파이프 라인에 동급할 수도 있고, 둘을 나누어서 위치 성분들은 첫 번째 입력 슬롯을 통해서, 색 상 성분들은 두 번째 입력 슬롯을 통해서 공급할 수도 있다. Direct3D 는 서로 다른 입력 슬롯들로부터 입력된 성분들을 조합해서 정점을 생성한다.

5. UINT AlignedByteOffset
	선택적 사항이다. 정점의 시작부터의 Offset ( in byte )이다. SemanticName 이 가리키는 값의 크기는 알지만 하나의 정점 데이터에서 해당 SemanticName 이 가리키는 값이 어디에 있는지는 모른다. 따라서 정점 시작 주소로부터 얼마만큼의 offset 이후에 POSITION 이 존재하는지 알아야 한다.

6. D3D11_INPUT_CLASSIFICATION InputSlotClass 
	single input slot에 대한 input data를 식별한다. 해당 데이터가 각 정점 한개당 어디에 데이터가 들어있는지를 뜻한다.

7. UINT InstanceDataStepRate 
	버퍼에서 하나의 요소씩 진행되기 전에 동일한 각 인스턴스별  데이터를 사용하여 그릴 인트턴스의 수. per-vertex data 를 포함하는 요소에 대해서 0을 대입한다.(slot class 는 D3D11_INPUT_PER_VERTEX_DATA를 대입한다.)

## Remarks

input-layout object 는 하나의 array of structure을 가진다. 각 structure는 하나의 element를 정의한다. . 