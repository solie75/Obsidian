디스플레이 어댑터를 나타내는 디바이스를 만든다.
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
4. UINT Flags : 활설화 할 런타임 레이어. 값은 OR 연산으로 함께 사용할 수 있다.
5. const D3D_FEATURE_LEVEL *pFeatureLevels : D3D_FEATURE_LEVEL의 배열을 가리키는 포인터. 작성하려고 하는 [[Feature Level]] 의 순서를 셜정한다. pFeatureLevels 가 NULL 로 설정된 경우 다음의 기능 수준 배열을 사용한다.
{  
    D3D_FEATURE_LEVEL_11_0,  
    D3D_FEATURE_LEVEL_10_1,  
    D3D_FEATURE_LEVEL_10_0,  
    D3D_FEATURE_LEVEL_9_3,  
    D3D_FEATURE_LEVEL_9_2,  
    D3D_FEATURE_LEVEL_9_1,  
};

6. UINT FeatureLevels : pFeatureLevels 의 element 수
7. UINT SDKVersion : SDK의 버전. D3D11_SDK_VERSION 사용
8. ID3D11Device **ppDevice : <span style="color:yellow">생성된 Device를 나타내는 ID3D11Device 객체에 대한 포인터의 주소를 반환.</span> 이 매개 변수가 NULL 이면 ID3D11Device는 반환되지 않는다.
9. D3D_FEATURE_LEVEL *pFeatureLevel : 성공했을 경우, 성공한 pFeatureLevels 배열로부터 최초의 D3D_FEATURE_LEVEL을 돌려준다. 지원되는 기능 레벨을 판별 할 필요가 없는 경우 입력으로 NULL 제공할 것.
10. ID3D11DeviceContext **ppImmediateContext : <span style="color:yellow">Device context 를 나타내는 ID3D11DeviceContext 객체에 대한 포인터의 주소를 반환한다.</span> 이 매게변수가 NULL 인 경우, ID3D11DeviceContext 는 반환되지 않는다.

## Return value

Type : HRESULT

이 메서드는 Direct3D 11 반환 코드 중 하나를 반환할 수 있다. 
pAdapter 매개 변수를 NULL이 아닌 값으로 설정하고 DriverType 매개 변수를 D3D_DRIVER_TYPE_HARDWARE 값으로 설정하면 이 메서드는 E_INVALIDARG를 반환한다.
Flag 가 D3D11_CREATE_DEVICE_DEBUG 를 지정하면, 이 메소드는 DXGI_ERROR_SDK_COMPONENT_MISSING을 반환하며, 부정확한 버젼 의 디버그 층이 컴퓨터에 인스톨 된다. 올바른 버전을 얻고 싶은 경우  최신 SDK 를 설치할 것.

