IDXGISwapChain::GetBuffer  method

스왑 체인의 백버퍼 중 하나에 접근한다.

## Syntax

```c++
HRESULT GetBuffer( 
	UINT Buffer, 
	[in] REFIID riid, 
	[out] void **ppSurface 
);
```

## Parameters

1. UINT Buffer : 0 에서 부터의 버퍼 인덱스 값. 만약 DXGI_SWAP_CHAIN_DESC::SwapEffect 가 DXGI_SWAP_EFFECT_DISCARD 라면, 이 메서드는 오직 첫번째 버퍼에만 접근할 수 있다; 이러한 경우, 인덱스를 0으로 설정한다. DXGI_SWAP_CHAIN_DESC::SwapEffect 가 DXGI_SWAP_EFFECT_SEQUENTIAL 또는 DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL 이면 스왑 체인의 제로 인덱스 버퍼만 읽고 쓸 수 있다. 인덱스가 0보다 큰 스왑 체인의 버퍼는 읽을 수만 있다. 따라서 이러한 버퍼에 대해 IDXGIResource::GetUsage 메서드를 호출하면 DXGI_USAGE_READ_ONLY flag 가 설정된다.
3. REFIID riid : 버퍼를 조작하는데 사용되는 인터페이스의 타입.
4. void **ppSurface : 백버퍼 인터페이스를 가리키는 포인터.

## Return value

HRESULT

