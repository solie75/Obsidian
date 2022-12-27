## Syntax

```c++
HRESULT CreateBuffer( 
	[in] const D3D11_BUFFER_DESC *pDesc, 
	[in, optional] const D3D11_SUBRESOURCE_DATA *pInitialData, 
	[out, optional] ID3D11Buffer **ppBuffer
);
```

## Parameters

1. const D3D11_BUFFER_DESC *pDesc : 버퍼를 설명하는 D3D11_BUFFER_DESC 구조체를 가리키는 포인터.
2. [in, optional] const D3D11_SUBRESOURCE_DATA *pInitialData : 데이터 초기화를 설명하는 D3D11_SUBRESOURCE_DATA 구조체를 가리키는 포인터. 오직 NULL 을 사용하여 공간을 할당 받는다. (flag가 D3D11_USAGE_IMMUTABLE이면 NULL이 될 수 없다. )
3. [out, optional] ID3D11Buffer **ppBuffer : 생성된 버퍼 객체에 대한 ID3D11Buffer 인터페이스를 가리키는 포인터의 주소.

## Return value

type : HRESULT

이 메소드는 만약 버퍼를 생성하기 위한 메모리가 충분하지 않다면 E_OUTOFMEMORY 를 반환한다.


## How to Create a Vertex Buffer

- 정적 정점 버퍼 (static vertax buffer)를 초기화
	1. 정점을 설명하는 구조체를 정읳나다. 예를 들어, 만약 정점 데이터가 position 데이터와 color 데이터를 포함한다면 구조체는 포지션을 설명하는 벡터하나와 색을 설명하는 벡터 하나를 가진다.
	2. 1번에서 정의된 구조체에 대해서 (malloc 또는 new 를 사용하여) 메모리를 할당 받는다. geometry를 설명하는 실제의 정점 데이터를 가지고 이 버퍼를 채운다.
	3. D3D11_BUFFER_DESC 구조체를 채움으로서 a buffer description을 생성한다. D3D11_BIND_VERTEX_BUFFER flag를 BindFlags 맴버에 전달하고 1번의 구조체의 크기를 ByteWidth 맴버에 전달한다.
	4. D3D11_SUBRESOURCE_DATA 구조체를 채움으로서 subresource 데이터 설명으 생성한다. D3D11_SUBRESOURCE_DATA 구조체의 pSysMem 맴버는 직접적으로 2번에서 생성된 resource data 를 가리켜야 한다.
	5. D3D11_BUFFER_DESC 구조체, D3D11_SUBRESOURCE_DATA 구조체 그리고 초기화할 ID3D11Buffer 인터페이스를 가리키는 포인터를 전달하는 동안 ID3D11Device::CreateBuffer 를 호출한다. 

다음의 코드 예시는 정점 버퍼를 어떻게 생성하는지를 보여준다. 

```c++
ID3D11Buffer*      g_pVertexBuffer;

// Define the data-type that
// describes a vertex.
struct SimpleVertexCombined
{
    XMFLOAT3 Pos;  
    XMFLOAT3 Col;  
};

// Supply the actual vertex data.
SimpleVertexCombined verticesCombo[] =
{
    XMFLOAT3( 0.0f, 0.5f, 0.5f ),
    XMFLOAT3( 0.0f, 0.0f, 0.5f ),
    XMFLOAT3( 0.5f, -0.5f, 0.5f ),
    XMFLOAT3( 0.5f, 0.0f, 0.0f ),
    XMFLOAT3( -0.5f, -0.5f, 0.5f ),
    XMFLOAT3( 0.0f, 0.5f, 0.0f ),
};

// Fill in a buffer description.
D3D11_BUFFER_DESC bufferDesc;
bufferDesc.Usage            = D3D11_USAGE_DEFAULT;
bufferDesc.ByteWidth        = sizeof( SimpleVertexCombined ) * 3;
bufferDesc.BindFlags        = D3D11_BIND_VERTEX_BUFFER;
bufferDesc.CPUAccessFlags   = 0;
bufferDesc.MiscFlags        = 0;

// Fill in the subresource data.
D3D11_SUBRESOURCE_DATA InitData;
InitData.pSysMem = verticesCombo;
InitData.SysMemPitch = 0;
InitData.SysMemSlicePitch = 0;

// Create the vertex buffer.
hr = g_pd3dDevice->CreateBuffer( &bufferDesc, &InitData, &g_pVertexBuffer );
```

## Remarks

다음은 시간이 흐름에 따라 변하는 정점 버퍼를 초기화 하는 방법이다.

1. D3D11_USAGE_STAGING으로 두번째 버퍼를 생성한다. ID3D11DeviceContext::Map, ID3D11DeviceContext::Unmap을 사용하여 두번째 버퍼를 채운다. ID3D11DeviceContext::CopyResource를 사용하여 staging buffer 로부터 default buffer 로 복사한다.
2. ID3D11DeviceContet::UpdataSubresource 를 사용하여 메모리로부터 데이터를 복사한다.
3. D3D11_USAGE_DYNAMIC으로 버퍼를 생성한다. 그리고 그 버퍼를 ID3D11DeviceContext::Map, ID3D11DeviceContext::Unmap을 사용하여 채운다

1번과 2번은 각 프레임당 한번 미만으로 변화가 있는 content일 때 유용하다. 보통, GPU 읽기는 빠르고 CPU 업데이트는 느리다.
3번은 각 프레임당 한 번이 넘게 변화가 있을 때 유용하다. GPU 읽기는 느려지고 CPU 업데이트는 빨라진다.

## How to Create an Index Buffer

- 인덱스 버퍼 초기화
	1. index 정보를 포함하는 버퍼를 생성
	2. D3D11_BUFFER_DESC 구조체를 채움으로서 버퍼 설명을 생성. D3D11_BIND_INDEX_BUFFER flag를 BindFlags 맴버에 전달한다. 그리고 버퍼의 크기를 ByteWidth 맴버에 전달한다. 
	3. D3D11_SUBRESOURCE_DATA 구조체를 채움으로서 subresource 데이터 설명을 생성한다. pSysMem  맴버는 1번에서 생성된 인덱스 버퍼를 직접적으로 가리켜야한다. 
	4. D3D11_BUFFER_DESC 구조체, D3D11_SUBRESOUCE_DATA 구조체, 초기화할 ID3DBuffer 인터페이스 를 가리키는 포인터의 주소를 전달하는 동안 ID3DDevice::CreateBuffer 를 호출한다. 

다음 코드 예시는 어떻게 인덱스 버퍼를 생성하는지 보여준다. 
다음 코드에서 g_pd3dDevice 는 ID3DDevice 객체이고, g_pd3dContext는 ID3D11DeviceContext 객체이다.

```c++
ID3D11Buffer *g_pIndexBuffer = NULL;

// Create indices.
unsigned int indices[] = { 0, 1, 2 };

// Fill in a buffer description.
D3D11_BUFFER_DESC bufferDesc;
bufferDesc.Usage           = D3D11_USAGE_DEFAULT;
bufferDesc.ByteWidth       = sizeof( unsigned int ) * 3;
bufferDesc.BindFlags       = D3D11_BIND_INDEX_BUFFER;
bufferDesc.CPUAccessFlags  = 0;
bufferDesc.MiscFlags       = 0;

// Define the resource data.
D3D11_SUBRESOURCE_DATA InitData;
InitData.pSysMem = indices;
InitData.SysMemPitch = 0;
InitData.SysMemSlicePitch = 0;

// Create the buffer with the device.
hr = g_pd3dDevice->CreateBuffer( &bufferDesc, &InitData, &g_pIndexBuffer );
if( FAILED( hr ) )
    return hr;

// Set the buffer.
g_pd3dContext->IASetIndexBuffer( g_pIndexBuffer, DXGI_FORMAT_R32_UINT, 0 );
```

## How to Create a Constant Buffer

- 상수 버퍼 초기화
	1. vertex shader constant data 를 설명하는 구조체를 정의한다. 
	2. 1번에서 정의한 구조체에 대해 메모리를 할당한다. vertex shader constant data 로 이 버퍼를 채운다. 이 메모리를 할당받는데 malloc 과 new 를 상용할 수 있다. 또한 stack에서  구조체를 위한 메모리를 할당 받을 수 있다. 
	3. D3D11_BUFFER_DESC 구조체를 채움으로서 버퍼 설명을 생성한다. D3D11_BIND_CONSTANT_BUFFER flag 를 BindFlags 맴버에 전달하고 상수 버퍼 설명 구조체의 크리를 ByteWidth 맴버에 전달한다.
	4. D3D11_SUBRESOURCE_DATA 구조체를 채움으로서 subresource 데이터 설명을 생성한다. D3D11_SUBRESOURCE_DATA 구조체의 pSysMem 맴버는 2번에서 생성한 vertex shader constant data 을 직접적으로 가리켜야 한다.
	5. D3D11_BUFFER_DESC 구조체, D3D11_SUBRESOURCE_DATA 구조체, 그리고 초기화할 ID3D11Buffer 인터페이스를 가리키는 포인터의 주소를 전달하는 동안 ID3D11Device::CreateBuffer 를 호출한다. 

다음 코드 예시에서는 어떻게 상수 버퍼를 생성하는지 보여준다. 이 코드네엇 g_pd3dDevice는 ID3DDevice 객체이고 g_pd3dContext 는 ID3D11DeviceContext 객체를 뜻한다.

```c++
ID3D11Buffer*   g_pConstantBuffer11 = NULL;

// Define the constant data used to communicate with shaders.
struct VS_CONSTANT_BUFFER
{
    XMFLOAT4X4 mWorldViewProj;                              
    XMFLOAT4 vSomeVectorThatMayBeNeededByASpecificShader;
    float fSomeFloatThatMayBeNeededByASpecificShader;
    float fTime;                                            
    float fSomeFloatThatMayBeNeededByASpecificShader2;
    float fSomeFloatThatMayBeNeededByASpecificShader3;
} VS_CONSTANT_BUFFER;

// Supply the vertex shader constant data.
VS_CONSTANT_BUFFER VsConstData;
VsConstData.mWorldViewProj = {...};
VsConstData.vSomeVectorThatMayBeNeededByASpecificShader = XMFLOAT4(1,2,3,4);
VsConstData.fSomeFloatThatMayBeNeededByASpecificShader = 3.0f;
VsConstData.fTime = 1.0f;
VsConstData.fSomeFloatThatMayBeNeededByASpecificShader2 = 2.0f;
VsConstData.fSomeFloatThatMayBeNeededByASpecificShader3 = 4.0f;

// Fill in a buffer description.
D3D11_BUFFER_DESC cbDesc;
cbDesc.ByteWidth = sizeof( VS_CONSTANT_BUFFER );
cbDesc.Usage = D3D11_USAGE_DYNAMIC;
cbDesc.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
cbDesc.CPUAccessFlags = D3D11_CPU_ACCESS_WRITE;
cbDesc.MiscFlags = 0;
cbDesc.StructureByteStride = 0;

// Fill in the subresource data.
D3D11_SUBRESOURCE_DATA InitData;
InitData.pSysMem = &VsConstData;
InitData.SysMemPitch = 0;
InitData.SysMemSlicePitch = 0;

// Create the buffer.
hr = g_pd3dDevice->CreateBuffer( &cbDesc, &InitData, 
                                 &g_pConstantBuffer11 );

if( FAILED( hr ) )
   return hr;

// Set the buffer.
g_pd3dContext->VSSetConstantBuffers( 0, 1, &g_pConstantBuffer11 );
```

