## syntax

```c++
HRESULT D3D11CreateDevice(
	IDXGIAdapter *pAdapter,
	D3D_DRIVER_TYPE DriverType,
	HMODULE Software,
	UINT Flags,
	const D3D_FEATURE_LEVEL *pFeatureLevels,
	UINT FeatureLevels,
	UINT SDKVersion,
	ID3D11Device **ppDevice,
	D3D_FEATURE_LEVEL *pFeatureLevel,
	ID3D11DeviceContext **ppImmediateContext
)

```

## Parameter

1. IDXGIAdapter (*pAdapter)  : device 를 생성하는데 쓰기 위한 video adaptor에 대한 포인터 이다. 기본 어뎁터를 사용하기 위해서는 NULL 값을 전달한다. 
2. D3D_DRIVER_TYPE DriverType : 생성할 드라이버 타입을 나타낸다.
3. HMODULE Software : software rasterizer 를 시행시키는 동적라이브러이에 대한 핸들
4. UINT Flags