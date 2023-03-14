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

# Remarks

### Using dynamic textures

하나의 format 및 가능하면 각 크기 별 오직 하나의 dynamic texture 를 생성하기가 권장된다. 모든 수준을 mapping하는 추가적인 overhead 때문에 dynamic mipmap, cubes, volumes 를 생성하는 것은 추천하지 않는다. mipmap 의 경우, 오직 top level 에 대해서 D3D11_MAP_WRITE_DISCARD 를 지정할 수 있다. 모든 level은 오직 top level만 mapping 함으로써 discard 된다. 이는 volume 과 cube 도 마찬가지이다. cube 의 경우 top level 과 face 0 은 매핑된다.

### Using dynamic buffers

GPU 가 buffer 를 사용하는 동안 static vertex buffer에 Map 을 호출한 경우 중요한 수행적 패널티가 발생할 수 있다. 이러한 경우, Map 은 응용프로그램에 반환값을 넘기기 전에 GPU 가 vertex 혹은 index data 를 다 읽을 때까지 기다려야 하기에 딜레이가 발생한다. Map을 호출한 다음 프레임당 여러번 static buffer 에서 렌더링 하는 것은 map pointer 를 반환하기 전에 command 를 끝내야 하기 때문에 렌더링 command 를 버퍼링 하지 못한다. 버퍼링된 command 없으면 GPU 는 응용프로그램이 정점 버퍼 또는 인덱스 버퍼를 채우고 렌더링 명령을 실행할 때까지 idle 상태를 유지한다. 


# Reference

https://learn.microsoft.com/en-us/windows/win32/direct3d11/how-to--use-dynamic-resources