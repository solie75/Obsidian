## Getting started with the Input-Assembler Stage

 IA stage 를 초기화 하기 위해서 는 몇개의 단계가 필요하다. 예를 들어 파이프 라인에 필요한 정점 데이터를 가진 버퍼 리소스를 생성해야하고 IA stage 에 버퍼가 어디에 존재하는지 그리고 그 버퍼가 포함하는 데이터가 어떤 유형인지는 전달해야하며 데이터로부터 assemble 할 primitive들의 유형을 특정해야 한다.

## Create Input Buffers

input Buffer 에는 두가지 유형이 있다 : [vertex buffer](Buffer#^2a545d)와 index buffer. <span style="color: yellow ">vertex buffer 는 IA stage 에 vertex 데이터를 공급한다.</span> <span style="color: yellow">Index Buffer 는 선택적인 것으로 vertex buffer에 속한 vertex 에 대한 index들을 공급한다.</span> 아마도 한개 이상의 vertex buffer와 선택적으로 index buffer 를 생성할 것이다. <span style="color:yellow">buffer resource 를 생성한 다음, IA stage에 대한 data layout 을 설명하는 input-layout object를 생성해야 하며 그 buffer resource 를 IA stage 에 바인딩해야 한다.</span> 만약 shader 가 버퍼를 사용하지 않는다면 buffer 의 생성이나 바인딩은 필요하지 않다.

## Create the Input-Layout Object

^e07bea

 <span style="color: yellow">input-layout object는 IA stage의 입력 상태(input state)를 압축한다.</span> 이것(input layout Object)은 IA stage에 바운딩 되는 input data의 설명을 포함한다. 그 <span style="color:yellow ">데이터(input data)는 하나 이상의 vertex buffer에 해당하는 메모리로부터 IA stage 에 스트리밍된다.</span> 그 설명은 하나 이상의 vertex buffer 로부터 input data 바운딩 되었는지 확인(식별)하고 ths shader input parameter type 과는 반대로(against, 해석이 불명확함) 런타임에 input data types를 확인하는 기능을 준다. 이 type은 그 type은 호환될 수 있다는 것을 입증하는 것 뿐만 아니라 shader가 요구하는 각각의 요소들은 buffer resource 에서 통용된다.

<span style="color: yellow">input-layout object는 input-element description의 배열 과 컴파일된 shader를 가리키는 포인터 로부터 생성된다.</span> 
그 inpout-element description 배열 은 하나 이상의 input elements 를 포함한다. 각 input element는 단일 vertex buffer 에 있는 단일 vertex-data elelment 를 설명한다. <span style="color: yellow">input-element description 의 모든 설정은 IA stage 에 바인딩 될 모든 vertex buffer에 있는 모든 vertex-data elements 를 설명한다.</span>

다음 layout description 은 세 개의 정점을 가진 단일 vertex buffer를 설명한다. 

D3D11_INPUT_ELEMENT_DESC layout[] =
{
    { L"POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0, 
          D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { L"TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT, 0, 12, 
          D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { L"NORMAL", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 20, 
          D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

<span style="color: yellow">하나의 input-element description 은 vertex buffer의 단일 vertex 에 속해 있는 크기, 유형, 위치, 목적 등의 각 요소들을 설명한다</span>. 각 행은 semantic, semantic index, data format을 사용함으로써 data의 유형을 식별한다. sementic은 데이터가 어떻게 사용될 지 식별하는 text string 이다. 위의 코드 예에서 첫 번째 행은 3개의 component position data( 예로, xyz )를 식별한다. 두 번째 행은 두개의 component position data(예를 들어, UV)를 식별한다. 그리고 세 번째 행은 normal data 를 식별한다.

하나의 input-element description 예시에서 (두 번째 매개변수인) semantic index 은 세개의 행에서 모두 0으로 설정되어 있다. sementic index 는 같은 sementic을 사용하는 행들을 구별하는데 도움을 준다. 이 예시에서는 비슷한 semantic이 없기 때문에, semantic index 는 기본 값인 0으로 세팅되었다.
같은 이름의 semantic이 없을 때 해당이름0 이고 이때 0은 생략한다. 반면 같은 이름의 semantic이 n 개 일 때 semantic 들의 이름은 순서대로 semantic0, semantic1, semantic2 ... semantic n 이 된다.

세번째 parameter는 format 이다. format 은 각 요소의 컴포넌트의 수와 각 요소에 대한 데이터의 크기를 정의하는 데이터 유형을 특정한다. 그 format은 리소스가 생성 될 때에 fully typed (<span style="color: green">완전한 타입이란..?</span>) 이 될 수 있고 또는 하나의 요소 내의 컴포넌트 수를 식별하는 DXGI_FORMAT 를 사용하여 리소스를 생성할 수 있지만 data type은 undefined 로 남는다.

### Input Slots

데이터는 **Input slots**로 불리는 입력을 통해서 IA stage 에 들어간다. 

![[Pasted image 20230102044355.png]]

IA stage 는 input data를 제공하는 n개의 vertex buffer까지를 수용하도록 디자인 된 n 개의 input slots 를 가지고 있다. 각 vertex buffer는 다른 slot에 배정되어야 한다. <span style="color: yellow">이러한 정보는 input-layout object 가 생성될 때 input-layout 선언에 저장된다. </span> 또한 각 버퍼의 시작 지점 부터 읽힐 버퍼의 첫번째 요소(element)까지를 나타내는 offset 을 특정할 수 있다.

네번 째 매개변수는 input slot 이고 다섯 번째 매개변수는 input offset 이다. 여러 개의 버퍼를 사용할 때, 그것들을 하나 혹은 여러개의 input slot 에 바인딩 할 수 있다. <span style="color:yellow ">input offset은 버퍼의 시작점과 data의 시작 사이의 byte 수이다. </span>

### Reusing Input-Layout Object

각 input-layout object 는 [shader signature](Signatures) 에 근거하여 생성된다. 이를 통해 
input-layout-object element가 shader-input signature에 부합한지(유효한지)에 대한 유효성 검사를 하는 API는 완전히 일치하는 type과 semantic을 갖는다. 모든 shader-input signature가 완전히 일치하는 한 많은 shader에 대한 단일 input-layout object를 를 생성할 수 있다. 

## Bind Objects to the Input-Assembler Stage

vertex buffer resource와 input layout object 를 생성한 후에 ID3D11DeviceContext::IASetVerTexBuffers 와 ID3D11DeviceContext::IASetInputLayout을 호출함으로서 IA stage 에 그것들을 바인딩할 수 있다. 다음의 예시 코드는 단일 vertex buffer 와 input layout object 를 IA stage 에 바인딩하는 것을 보여준다.

```c++
UINT stride = sizeof( SimpleVertex );
UINT offset = 0; 
g_pd3dDevice->IASetVertexBuffers( 
	0, // the first input slot for binding 
	1, // the number of buffers in the array 
	&g_pVertexBuffer, // the array of vertex buffers 
	&stride, // array of stride values, one for each buffer 
	&offset 
); // array of offset values, one for each buffer 

// Set the input layout 
g_pd3dDevice->IASetInputLayout( g_pVertexLayout );
```

input-layout object를 바인딩 하는 것은 오직 그 object 의 포인터만을 요구한다.

앞선 예에서, 단일 vertex buffer 는 바인딩됨을 볼 수 있다. 하지만 다중(multiple) vertex buffer는 ID3D11DeviceContext::IASetVertexBuffer 를 한번 호출하는 것으로 바인딩을 할 수 있다. 다음의 코드는 세 개의 vertex buffer 를 바인딩 하는 것을 보여준다.

```c++
UINT strides[3];
strides[0] = sizeof(SimpleVertex1);
strides[1] = sizeof(SimpleVertex2);
strides[2] = sizeof(SimpleVertex3);
UINT offsets[3] = { 0, 0, 0 };
g_pd3dDevice->IASetVertexBuffers( 
    0,                 //first input slot for binding
    3,                 //number of buffers in the array
    &g_pVertexBuffers, //array of three vertex buffers
    &strides,          //array of stride values, one for each buffer
    &offsets );        //array of offset values, one for each buffer
```

하나의 index buffer 는  ID3D11DeviceContext::IASetIndexBuffer 를 호출하여 바인딩한다.

## Specify the Primitive Type

input buffer 가 바인딩 된 후에 , IA stage 는 어떻게 vertex들을 primitive들에 assemble 할 것인지에 대한 분부(order)를 받아야한다. 이덕은 ID3D11DeviceContext::IASetPrimitiveTopology를 호출하여 primitive type 을 특정 지음으로써 완료된다. 다음의 코드는 데이터를인접성 없은 triangle list로 정의하기 위해서 이 함수를 호출한다.

```c++
g_pd3dDevice->IASetPrimitiveTopology(
	D3D_PRIMITIVE_TOPOLOGY_TRIANGLELIST 
);
```
나머지 primitive type들은 D3D_PRIMITIVE_TOPOLOGY에 리스팅 되어 (be listed) 있다.

## Call Drow Methods

<span style="color: yellow">input resource 가 파이프라인에 바인딩 된 이후, 하나의 응용프로그램은 primitive를 렌더링 하기 위해서 draw method를 호출한다.</span> 다음의 테이블에서 보이는 것과 같이 몇개의 draw method가 있다. 어떤 것은 index buffer 를 사용하고, 어떤 것은 instance data를 사용한다. 그리고 input-assembler stage에 대한 입력으로써 streaming-output stage 에 있는 어떤 reuse data또한 그리 사용된다.

| **Draw Methods** | **Description** |
| --- | --- |
| [**ID3D11DeviceContext::Draw**](https://learn.microsoft.com/en-us/windows/desktop/api/D3D11/nf-d3d11-id3d11devicecontext-draw) | Draw non-indexed, non-instanced primitives. |
| [**ID3D11DeviceContext::DrawInstanced**](https://learn.microsoft.com/en-us/windows/desktop/api/D3D11/nf-d3d11-id3d11devicecontext-drawinstanced) | Draw non-indexed, instanced primitives. |
| [**ID3D11DeviceContext::DrawIndexed**](https://learn.microsoft.com/en-us/windows/desktop/api/D3D11/nf-d3d11-id3d11devicecontext-drawindexed) | Draw indexed, non-instanced primitives. |
| [**ID3D11DeviceContext::DrawIndexedInstanced**](https://learn.microsoft.com/en-us/windows/desktop/api/D3D11/nf-d3d11-id3d11devicecontext-drawindexedinstanced) | Draw indexed, instanced primitives. |
| [**ID3D11DeviceContext::DrawAuto**](https://learn.microsoft.com/en-us/windows/desktop/api/D3D11/nf-d3d11-id3d11devicecontext-drawauto) | Draw non-indexed, non-instanced primitives from input data that comes from the streaming-output stage. |

각 draw method 는 하나의 topology type 을 렌더링한다. 렌더링하는 동안 불완전한 primitive 들(충분한 정점들이 없는, 인덱스가 부족한, 부분적인 primitive 그외 등등)은 별다른 언급 없이 폐지(삭제)된다.

