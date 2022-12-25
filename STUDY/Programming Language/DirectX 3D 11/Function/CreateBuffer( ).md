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
2. [in, optional] const D3D11_SUBRESOURCE_DATA *pInitialData : 데이터 초기화를 설명하는 D3D11_SUBRESOURCE_DATA 구조체를 가리키는 포인터.
3. 