## 1. Vertex and InputLayout

Direct3D 의 정덤은 <span style="color: yellow">공간적 위치 이외에 추가적인 자료를 가질수 있다. </span>
-> 원하는 자료로 정점 형식을 커스텀 할 수 있다.
-> 우선 자료를 담을 구조체를 정의해야 한다.

```c++
// 구조체 정의의 예

struct Vertex1
{
	XMFLOAT3 Pos;
	XMFLOAT4 Color;
};

struct Vertex2
{
	XMFLOAT3 Pos;
	XMFLOAT3 Normal;
	XMFLOAT2 Tex0;
	XMFLOAT2 Tex1;
}
```

-> 이 정점 구조체의 각 성분(필드)가 어떤 용도인지를 Direct3D 에 알려주어야 한다.

이때 그러한 정보는 Direct3D 에게 알려주는 수단이 바로 [[ID3D11InputLayout]] 형식의 <span style="color: yellow">input layout</span> 이다. 
-> input layout 은 [[D3D11_INPUT_ELEMENT_DESC]] 구조체 들로 이루어진 배열을 통해 구축된다.
-> 해당 배열의 원소가 되는 하나의 D3D11_INPUT_ELEMENT_DESC 구조체는 정점 구조체의 한 성분을 서술하는 역할을 한다.
	예를 들어 정점 구조체의 성분이 두 개 라고 한다면 D3D11_INPUT_ELEMENT_DESC 배열의 원소도 두개여야 한다. 해당 배열을 <span style="color: yellow "> input layout description array</span> 이라 부른다.

-> input layout description array 와 input layout 을 위한 ID3D11InputLayout interface 를 얻었다면  ID3D11Device::[[CreateInputLayout( )]] 메서드를 호출하여 input layout 을 생성한다.

vertex shader 는 일단의 <span style="color: yellow">정점 성분들을 입력 매개변수</span>들로서 받는다. 그 입력 매개변수들의 형식과 개수를 통칭하여 <span style="color: yellow">input signature </span>이라 부른다. 

커스텀 vertex structure 의 element 들을 반드지 vertex shader 의 적절한 input parameter 에 대응 되어야 한다.  input layout 을 생성할 때 이 input parameter 를 통해 vertex shader 의 input signature 에 대한 정보를 제공한다. 따라서 <span style="color: yellow">Direct3D는 input layout 생성 시점에서 input layout과 input signature 가 부합하는지 점검하고 vertex structure 와 shader input 사이의 대응 관계를 형성</span>할 수 있게 한다. 생성된 input layout 은 다른 shader 들에게 재사용 될 수 있다. 이때 해당 shader 들은 input signature 가 정확히 동일해야 한다.

---
- AK 의 코드의 경우 
	1. .fx 파일을 생성하고 shader code 를 작성한다. 
	2. 이 shader code는 input signature 역할을 한다. 
	3. 해당 shader code 는 [[D3DCompileFromFile( )]]에 의해 컴파일 되고 그로 인해 생성된 컴파일된 객체는 [[CreateVertexShader( )]] 또는 [[CreatePixelShader( )]] 등을 통해 쉐이더를 생성한다. 
	4. 생성된 쉐이더는 blob 형식의 객체에 저장된다.
	5. 생성된 blob 형식의 쉐이더 객체는 [[CreateInputLayout( )]] 의 매개변수로 사용된다.
- ---

생성된 input layout 은 [[IASetInputLayout( )]]으로 device 에 바인딩 된다.
하나의 input layout 이 device 에 바인딩 되었다면 , 다른 input layout 을 바인딩 할 때까지 그 input layout 은 계속 적용된다.


## 2. VertexBuffer

GPU가 vertex array 에 접근하려면 array의 vertex 들을 Buffer 라는 특정 resource 에 담아 두어야 한다. 이 때, vertex 들을 담은 버퍼를 VertexBuffer 라고 하며 자료를 담는것 뿐만 아니라 CPU 나 GPU의 resource 에 대한 접근 방식, buffer가 바인딩 되는 pipeline의 지점에 대한 정보 등도 가지고 있다. 

- 생성 과정
	1. [[D3D11_BUFFER_DESC]] 를 세팅한다.
	2. [[D3D11_SUBRESOURCE_DATA]] 를 세팅한다. (해당 구조체는 <span style="color: yellow">버퍼의 초기화에 사용할 자료를 서술</span>한다.)
	3. ID3D11Device::[[CreateBuffer( )]] 를 호출하여 버퍼를 생성한다.

- 생성된 buffer 의 vectex 들을 파이프 라인으로 공급하기 위해 ID3D11DeviceContext::[[IASetVertexBuffers( )]]를 호출하여 buffer 를 device 의 하나의 slot에 바인딩 해야 한다.

- device에 바인딩된 buffer 를 이제 그리기 위해서 ID3D11DeviceContext::[[Draw( )]] 를 호출한다.
	1. 이 때 매개변수로 draw 할 vertex 개수와 vertex buffer 의 vertex 중에서 draw 에 사용할 첫 번째 vertex 의 index (0 기반) 을 지정한다.


## 3. Index and IndexBuffer

Index 역이 GPU가 접근할 수 있는 특정 resource 에 넣어야 하고 이 특정 resource가 IndexBuffer 이다. vertex buffer 에서 vertex 대신에 index 를 넣으면 index buffer 이다.

```c++
// index
UINT indices[24] = {
	0, 1, 2, // 삼각형 0
	0, 2, 3, // 삼각형 1
	0, 3, 4, // 삼각형 2
	0, 4, 5, // 삼각형 3
	0, 5, 6, // 삼각형 4
	0, 6, 7, // 삼각형 5
	0, 7, 8, // 삼각형 6
	0, 8, 1 // 삼각형 7
}
```

1. [[D3D11_BUFFER_DESC]] 를 세팅한다. 이 때 blind flag 로 D3D11_BIND_INDEX_BUFFER 를 지정한다.
```c++
D3D11_BUFFER_DESC ibd;
ibd.Usage = D3D11_USAGE_IMMUTABLE;
ibd.ByteWidth = sizeof(indices);
ibd.BindFlags = D3D11_BIND_INDEX_BUFFER;
ibd.CPUAccessFlags = 0;
ibd.MiscFlags = 0;
ibd.StructureByteStride = 0;
```
2. 색인 버퍼를 초기화할 자료인 [[D3D11_SUBRESOURCE_DATA]] 를 설정한다.
```c++
D3D11_SUBRESOURCE_DATA iinitData;
iinitData.pSysMem = indices;
```
3. [[CreateBuffer( )]] 를 호출하여 index buffer 를 생성한다.
```c++
ID3D11Buffer* mIB;
HR(DEVICE->CreateBuffer(&ibd, &iinitData, &mIB))
```
4. ID3D11DevcieContext::[[IASetIndexBuffer( )]] 를 호출하여 input assembler stage 에 바인딩한다.
5. index buffer 를 이용하여 기본도형을 그릴 때에는 [[DrawIndexed( )]] 를 호출한다.

다음은 구와 상자와 원기둥의 vertex buffer 와 index buffer 들을 하나로 합친 예시이다.
![[Pasted image 20230215234839.png]]

상자의 index 를 절대적으로 본다면
0, 1, ... , numBoxVertices -1
이지만 연결된 전역 index buffer 에서 상자 vertex 들의 index 는 다음과 같다.
firstBoxVertexPos, firstBoxVertexPos + 1, ... , firstBoxVertexPos + numBoxVertices -1

따라서 전역 index buffer 의 첫 번째 buffer 를 제외하고 나머지는 기존의 절대적 index의 각 요소에 첫번재 인덱스를 더해야 한다. 이렇듯 전역 vertex buffer 에서 한 물체의 첫 vertex 위치를 그 물체의 base vertex location(기준 정점 위치) 라고 한다. 일반적으로 (버퍼들을 차례로 연결하여 합친 경우) 한 물체의 새 index 들은 그 base vertex location 를 각 index 에 더한 것이다. 이때 이러한 작업을 DrawIndexed 의 세 번째 매개변수로 base vertex location 을 지정하면 Direct3D 가 수행한다.

이제 다음과 같이 세 번의 그리기 호출로 구, 상자, 원기둥을 draw 할 수 있다.
```c++
CONTEXT -> DrawIndexed(numSphereIndices, 0, 0);
CONTEXT -> DrawIndexed(numBoxIndices, firstBoxIndex, firstBoxVertexPos);
CONTEXT -> DrawIndexed(numCylIndices, firstCylIndex, firstCylVertexPos);
```

