다음의 API 가 중요하다.
[[Map( )|ID3D11DeviceContext::Map]]
[[Unmap( )|ID3D11DeviceContext::Unmap]]
[[D3D11_BUFFER_DESC#^8317a2|D3D11_USAGE]]

응용프로그램이 해당 resource 를 바꾸는 것이 필요할 때 dynamic resource 를 생성하고 사용한다. 사용자는 texture와 buffer 를 dynamic usage 로 생성할 수 있다.

# 단계별

### step 1 : dynamic usage 특정

응용프로그램이 리소스를 변경할 수 있게 되기를 원한다면 리소스를 생성할 때 dynamic 이며 작성가능하게  특정해야 한다.

- dynamic usage 를 특정하기 위해서는 
	1. 리소소의 usage 를 dynamic 으로 지정한다. 
		예를 들어 정점 버퍼 혹은 상수 버퍼에 대한 D3D11_BUFFER_DESC 의 맴버인 Usage 에 D3D11_USAGE_DYNAMIC value 를 지정한다. 그리고 2D Texture 에 대한 D3D11_TEXTURE2D_DESC 의 맴버인 Usage에 D3D11_USAGE_DYNAMIC value 를 지정한다.
	2. 리소스를 작성가능으로 지정한다. 
		예를 들어 D3D11_BUFFER_DESC 또는 D3D11_TEXTURE2D_DESC 의 맴버인 CPUACCESSFLAG 에 D3D11_CPU_ACCESS_WRITE value 를 지정한다. 
	3. 리소스 description 을 생성 함수에 전달한다. 
		예를 들어 D3D11_BUFFER_DESC 의 주소를 ID3D11DEVICE::CreateBuffer 에 전달하고 D3D11_TEXTURE2D_DESC 의 주소를 ID3D11Device::CreateTexture2D 에 전달한다.

다음은 dynamic vertex buffer 를 생성하는 예이다.
```c++
// Create a vertex buffer for a triangle.

    float2 triangleVertices[] =
    {
        float2(-0.5f, -0.5f),
        float2(0.0f, 0.5f),
        float2(0.5f, -0.5f),
    };

    D3D11_BUFFER_DESC vertexBufferDesc = { 0 };
    vertexBufferDesc.ByteWidth = sizeof(float2)* ARRAYSIZE(triangleVertices);
    vertexBufferDesc.Usage = D3D11_USAGE_DYNAMIC;
    vertexBufferDesc.BindFlags = D3D11_BIND_VERTEX_BUFFER;
    vertexBufferDesc.CPUAccessFlags = D3D11_CPU_ACCESS_WRITE;
    vertexBufferDesc.MiscFlags = 0;
    vertexBufferDesc.StructureByteStride = 0;

    D3D11_SUBRESOURCE_DATA vertexBufferData;
    vertexBufferData.pSysMem = triangleVertices;
    vertexBufferData.SysMemPitch = 0;
    vertexBufferData.SysMemSlicePitch = 0;

    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(
	        &vertexBufferDesc,
	        &vertexBufferData,
	        &vertexBuffer2
        )
    );
```

### step2 : dynamic resource 의 변경

리소스를 dynamic 과 writable 로 지정하여 생성한 경우 후에 해당 리소스에 대한 변경이 가능하다.ㅏ

- to change data in a dynamic resource
	1. resource 에 대한 새로운 설정 세팅. D3D11_MAPPED_SUBRESOURCE 타입의 변수를 선언하고 0으로 초기화한다. 사용자는 이 변수를 리소스를 변경하는데에 사용할 것이다.
	2. 변경되길 원하는 데이터에 대한 GPU 의 접근을 비활성화 시킨다. 그리고 헤당 데이터에 대한  메모리를 가리키는 포인터를 얻는다. 이 포인터를 얻기 위해서 ID3D11DeviceContext::Map 을 호출할 때 D3D11_MAP_WRITE_DISCARD 를 MapType 매개변수에 전달한다. 이 포인터를 이전 단계의 D3D11_MAPPED_SUBRESOURCE 의 주소로 설정한다.
	3. 메모리에 새로운 데이터를 작성한다.
	4. 새로운 데이터에 대한 작성이 끝나면 ID3D11DeviceContext::Unmap 을 호출하여 해당 데이터에 대한 GPU 접근을 재활성화 시킨다.

다음은 dynamic vertex buffer 의 변경에 대한 예시이다.

```c++
// This might get called as the result of a click event to double the size of the triangle.
void TriangleRenderer::MapDoubleVertices()
{
    D3D11_MAPPED_SUBRESOURCE mappedResource;
    ZeroMemory(&mappedResource, sizeof(D3D11_MAPPED_SUBRESOURCE));
    float2 vertices[] =
    {
        float2(-1.0f, -1.0f),
        float2(0.0f, 1.0f),
        float2(1.0f, -1.0f),
    };
    //  Disable GPU access to the vertex buffer data.
    auto m_d3dContext = m_deviceResources-> GetD3DDeviceContext();
    m_d3dContext->Map(vertexBuffer2.Get(), 0, D3D11_MAP_WRITE_DISCARD, 0, &mappedResource);
    //  Update the vertex buffer here.
    memcpy(mappedResource.pData, vertices, sizeof(vertices));
    //  Reenable GPU access to the vertex buffer data.
    m_d3dContext->Unmap(vertexBuffer2.Get(), 0);
}
```
-> Map() 함수는 메모리를 잠그는 함수이다. 여기서 잠그다는 뜻은 다른 프로세스가 해당 메모리에 접근을 하지 못하게 막는 것을 의미한다. 현재의 운영체제는 멀티프로세스로 작동하기 때문에 같은 메모리 영역의 내용을 여러개의 프로세스 들이 동시에 사용하게 된다. 따라서 단순히 정보를 읽어가기 위한 동시 접근은 일반적으로 문제가 없으나 특정 프로세스가 기록을 하는 행위를 하게 되면 기록이 끝날 때까지 데이터가 계속해서 변경이 되고 있는 상태기 때문에 기록이 끝나지 않은 메모리 부터 데이터를 읽어가는 것과 같은 불안정한 데이터를 가져가는 결과가 된다. 따라서 메머리 데이터를 변경하기 전에 잠가서(lock 를 해서) 타 프로세스의 접근을 제한하고 write 작업이 다 끝나고 나면 잠갔던 것을 해제 (unlock)하게 된다. 따라서 Map() 함수 호출 뒤에는 반드시 Unmap( ) 으로 잠금 상태를 해제해 주어야 한다.
DirectX 에서는 GPU 메모리에 대한 lock, unlock 을 Map(), Unmap() 으로 구현한다.
따라서 과정은 다름과 같다. ^909bc3
```
잠근다 Map()
메모리 작업 (쓰기, 변경, 지우기 등) memcpy 등
잠금을 해제한다. Unmap()
```
# Remarks

### Using dynamic textures

하나의 format 및 가능하면 각 크기 별 오직 하나의 dynamic texture 를 생성하기가 권장된다. 모든 수준을 mapping하는 추가적인 overhead 때문에 dynamic mipmap, cubes, volumes 를 생성하는 것은 추천하지 않는다. mipmap 의 경우, 오직 top level 에 대해서 D3D11_MAP_WRITE_DISCARD 를 지정할 수 있다. 모든 level은 오직 top level만 mapping 함으로써 discard 된다. 이는 volume 과 cube 도 마찬가지이다. cube 의 경우 top level 과 face 0 은 매핑된다.

### Using dynamic buffers

GPU 가 buffer 를 사용하는 동안 static vertex buffer에 Map 을 호출한 경우 중요한 수행적 패널티가 발생할 수 있다. 이러한 경우, Map 은 응용프로그램에 반환값을 넘기기 전에 GPU 가 vertex 혹은 index data 를 다 읽을 때까지 기다려야 하기에 딜레이가 발생한다. Map을 호출한 다음 프레임당 여러번 static buffer 에서 렌더링 하는 것은 map pointer 를 반환하기 전에 command 를 끝내야 하기 때문에 렌더링 command 를 버퍼링 하지 못한다. 버퍼링된 command 없으면 GPU 는 응용프로그램이 정점 버퍼 또는 인덱스 버퍼를 채우고 렌더링 명령을 실행할 때까지 idle 상태를 유지한다. 

vertex 또는  index data가 변하지 않는것이 이상적이지만 언제나 그렇지는 않다. 응용프로그램이 매 프레임 마다. vertex 또는 index data 의 변경을 필요로 하는 상황은 많다. 이러한 상황에서, vertex 또는 index buffer 를 D3D11_USAGE_DYNAMIC 으로 생성하는 것이 추천된다. 이 usage flag 로 인해 runtime 은 잦은 mapping 작업에 맞게 최적화 된다. D3D_USAGE_DYNAMIC 은 오직 buffer 가 자주 mapping 될 때 유용하다. data 를 변함없이 유지하고 싶다면 해당 데이터를 static vertex Ehsms index buffer 에 배치한다.

dynamic vertex buffer 를 사용할 때 수행 향상을 위해서 응용프로그램은 적절한 D3D11_MAP 값으로 Map 을 호출해야 한다.
<span style="color:#ffd33d"> D3D11_MAP_WRITE_DISCARD 는 응용프로그램이 buffer 에 오래된 vertex 또는 index data 를 유지할 필요가 없다는 것을 나타낸다. </span> 이는 GPU 가 응용프로그램이 새로운 버퍼에 데이터를 배치하는 동안 오래된 데이터를 계속 사용하는 것을 허락한다는 것을 나타낸다. 추가적인 메모리 관리가 요구되지 않는다. 오랜된 buffer 는 재사용 되거나 GPU의 사용이 끝나면 자동적으로 파괴된다. 

- note
	buffer를 D3D11_MAP_WRITE_DISCARD 로 매핑하면 , 런타임은 언제나 모든 buffer 를 discard 한다. nonzero offset 또는 제한된 크기의 field 를 특정하는 것으로 buffer 의 매핑되지 않은 영역에 정보를 보존할 수는 없다.

There are cases where the amount of data the app needs to store per map is small, such as adding four vertices to render a sprite. [**D3D11_MAP_WRITE_NO_OVERWRITE**](https://learn.microsoft.com/en-us/windows/desktop/api/D3D11/ne-d3d11-d3d11_map) indicates that the app will not overwrite data already in use in the dynamic buffer. The [**Map**](https://learn.microsoft.com/en-us/windows/desktop/api/D3D11/nf-d3d11-id3d11devicecontext-map) call will return a pointer to the old data, which will allow the app to add new data in unused regions of the vertex or index buffer. The app must not modify vertices or indices that are used in a draw operation as they might still be in use by the GPU. We recommend that the app then use [**D3D11_MAP_WRITE_DISCARD**](https://learn.microsoft.com/en-us/windows/desktop/api/D3D11/ne-d3d11-d3d11_map) after the dynamic buffer is full to receive a new region of memory, which discards the old vertex or index data after the GPU is finished.

The asynchronous query mechanism is useful to determine if vertices are still in use by the GPU. Issue a query of type [**D3D11_QUERY_EVENT**](https://learn.microsoft.com/en-us/windows/desktop/api/D3D11/ne-d3d11-d3d11_query) after the last draw call that uses the vertices. The vertices are no longer in use when [**ID3D11DeviceContext::GetData**](https://learn.microsoft.com/en-us/windows/desktop/api/D3D11/nf-d3d11-id3d11devicecontext-getdata) returns S_OK. When you map a buffer with [**D3D11_MAP_WRITE_DISCARD**](https://learn.microsoft.com/en-us/windows/desktop/api/D3D11/ne-d3d11-d3d11_map) or no map values, you are always guaranteed that the vertices are synchronized properly with the GPU. But, when you map without map values, you will incur the performance penalty described earlier.
# Reference

https://learn.microsoft.com/en-us/windows/win32/direct3d11/how-to--use-dynamic-resources