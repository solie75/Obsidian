buffer 인터페이스는 메모리로 구성되지 않은 buffer resource 에 접근한다. Buffer 는 보통 vertex 나 index data 를 저장한다.

# Inheritance

ID3D11Resource 를 상속 받는다.

## Remark

There are three types of buffers: vertex, index, or a shader-constant buffer. Create a buffer resource by calling [ID3D11Device::CreateBuffer](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer).
buffer 는 accesse 되기 전에 pipeline 에 바인딩 되어야 한다. 
Buffers can be bound to the input-assembler stage by calls to [ID3D11DeviceContext::IASetVertexBuffers](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers) and [ID3D11DeviceContext::IASetIndexBuffer](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer), to the stream-output stage by a call to [ID3D11DeviceContext::SOSetTargets](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-sosettargets), and to a shader stage by calling the appropriate shader method (such as [ID3D11DeviceContext::VSSetConstantBuffers](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetconstantbuffers) for example).

Buffers can be bound to multiple pipeline stages simultaneously for reading. A buffer can also be bound to a single pipeline stage for writing; however, the same buffer cannot be bound for reading and writing simultaneously.