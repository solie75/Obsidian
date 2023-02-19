ID3D11DeviceContext::IASetIndexBuffer( );

index buffer 를 input assembler stage 에 바인딩한다.

```c++
void IASetIndexBuffer(
  [in, optional] ID3D11Buffer* pIndexBuffer,
  [in]           DXGI_FORMAT  Format,
  [in]           UINT         Offset
);
```

1. pIndexBuffer
	[[ID3D11Buffer]]객체를 가리키는 포인터로 indices 를 포함한다. index buffer 는 반드시D3D11_BIND_INDEX_BUFFER flag로 생성되어야 한다.

2. Format
	index buffer 내의 data 의 형식를 특정하는 [DXGI_FORMAT](https://learn.microsoft.com/en-us/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format) .

3. Offset
	index buffer 의 시작부터 사용할 첫 번째 인덱스 까지의 offset.


# Remark

Calling this method using a buffer that is currently bound for writing (i.e. bound to the stream output pipeline stage) will effectively bind **NULL** instead because a buffer cannot be bound as both an input and an output at the same time.

The debug layer will generate a warning whenever a resource is prevented from being bound simultaneously as an input and an output, but this will not prevent invalid data from being used by the runtime.