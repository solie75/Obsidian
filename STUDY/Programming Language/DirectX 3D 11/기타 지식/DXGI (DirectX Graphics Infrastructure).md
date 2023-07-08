DXGI (Microsoft Directx Graphics Infrastructure) 는 그래픽 어댑터 열거, 디스플레이 모드 열거, 버퍼 형식 선택, 프로세스 간(예 : 응용 프로그램과 데스크탑 창 관리자(DWM)간 )자원 공유, 렌더링 된 프레임(장면)을 창 또는 모니터에 나타내는 작업을 처리한다.

대부분 그래픽 프로그래밍이 Direct3D 에서 수행되지만, DXGI 를 사용하여 최종 구성(<span style="color:green">여기에서 말하는 최종 구성이란...?</span>)(eventual composition)및 디스플레이를 위해 창, 모니터 또는 기타 그래픽 구성 요소에 프레임(장면)을 나타낼 수 있다. 또한 DXGI 를 사용하여 모니터에 나타나는 콘텐츠를 읽을 수 있다.

DXGI는 Direct3D와 함쎄 쓰이는 API 로 DirectX 크래픽 런타임에 독립적인 저수준(Low-Level)의 작업을 관리한다. 또한 DirectX 그래픽을 위한 기본적이고 공통적인 프레임워크를 제공한다. 

![[Pasted image 20221212211004.png]]

예를 들어 2차원 랜더링 api 에도 3차원 렌더링 api 처럼 스왑 체인과 페이지 전환이 필요하다. 이 때문에, 스왑체인을 대표하는 인터페이스인 IDXGISwapChain은 실제로 DXGI API 의 일부이다.

다음 코드는 시스템에 있는 모든 어댑터를 열거하는 방법이다.
```c++
// 어뎁터에 접근할 index 변수
UINT i = 0;

IDXGIDevice* pDXGIDevice = nullptr;
// Adapter 를 담아둘 포인터 변수
IDXGIAdapter* padapte = nullptr;
IDXGIFACTORY* pFactory = nullptr;

// D3D11CreateDevice 로 생성된 디바이스를 m_Device 라고 할 때
m_Device->QueryInterface(__uuidof(IDXGIDevice), (void**)&pDXGIDevice);
pDXGIDevice->GetParent(__uuidof(IDXGIAdapter), (void**)&pAdapter);
pAdapter->GetParent(__uuidof(IDXGIFactory), (void**)&pFactory);

// Adapter들을 저장할 vector 컨테이너
std::vector<IDXGIAdapter*> adapterList;

// IDSGIFactory 객체를 통해 어뎁터들을 열거한다. i를 index로 활용
// 만약 index가 시스템에 존재하는 어뎁터의 갯수와 같거나 더 크다면 while문을 종료한다.
while(pFactory->EnumAdapters(i, &pAdapter != DXGI_ERROR_NOT_FOUNT)
{
	DXGI_ADAPTER_DESC desc;
	pAdapter->GetDesc(&desc);
	wstring pdes = desc.Description;
	adapterList.push_back(pAdapter.Get());
	++i;
}
```

다음 코드를 실행하면 pdes 는 adapter를 순서대로 가지고 있는다.
```
pdes 결과 예시
NVIDIA GeForce RTX 2070
Microsoft Basic Render Driver
(Microsoft Basic Render Driver 는 window 8 이상에 포함된 소프트웨어 디스플레이 어뎁터 이다.)
```

- DXGIDevice
	이 인터페이스는 주로 그래픽 처리 장치(GPU)와 상호 작용하는 데 사용된다. IDXGIDevice를 사용하여 그래픽 디바이스의 속성과 기능을 확인하고, 디바이스와의 통신을 관리한다. DXGI 디바이스를 생성하고, DirectX 그래픽 애플리케이션과 그래픽 디바이스 간의 상호 작용을 조정하는 데 사용된다.
```c++
IDXGIDevice : public IDXGIObject
    {
    public:
        virtual HRESULT STDMETHODCALLTYPE GetAdapter( 
            /* [annotation][out] */ 
            _COM_Outptr_  IDXGIAdapter **pAdapter) = 0;
        
        virtual HRESULT STDMETHODCALLTYPE CreateSurface( 
            /* [annotation][in] */ 
            _In_  const DXGI_SURFACE_DESC *pDesc,
            /* [in] */ UINT NumSurfaces,
            /* [in] */ DXGI_USAGE Usage,
            /* [annotation][in] */ 
            _In_opt_  const DXGI_SHARED_RESOURCE *pSharedResource,
            /* [annotation][out] */ 
            _COM_Outptr_  IDXGISurface **ppSurface) = 0;
        
        virtual HRESULT STDMETHODCALLTYPE QueryResourceResidency( 
            /* [annotation][size_is][in] */ 
            _In_reads_(NumResources)  IUnknown *const *ppResources,
            /* [annotation][size_is][out] */ 
            _Out_writes_(NumResources)  DXGI_RESIDENCY *pResidencyStatus,
            /* [in] */ UINT NumResources) = 0;
        
        virtual HRESULT STDMETHODCALLTYPE SetGPUThreadPriority( 
            /* [in] */ INT Priority) = 0;
        
        virtual HRESULT STDMETHODCALLTYPE GetGPUThreadPriority( 
            /* [annotation][retval][out] */ 
            _Out_  INT *pPriority) = 0;
        
    };
```

- DXGIAdaptor
	그래픽 어댑터를 나타내며 이는 시스템에 설치된 그래픽 카드를 나타낸다. 해당 인터페이스로 시스템에 설치된 그래픽 목록을 가져오고, 각 어댑터에 대한 정보를 얻을 수 있다. 또한 DXGI 디바이스를 생성할 때 특정 어댑터를 선택할 수 있고 이는 어플이 특정 그래픽 카드를 사용하도록 지정하는데 영향을 끼친다.
```c++
IDXGIAdapter : public IDXGIObject
    {
    public:
        virtual HRESULT STDMETHODCALLTYPE EnumOutputs( 
            /* [in] */ UINT Output,
            /* [annotation][out][in] */ 
            _COM_Outptr_  IDXGIOutput **ppOutput) = 0;
        
        virtual HRESULT STDMETHODCALLTYPE GetDesc( 
            /* [annotation][out] */ 
            _Out_  DXGI_ADAPTER_DESC *pDesc) = 0;
        
        virtual HRESULT STDMETHODCALLTYPE CheckInterfaceSupport( 
            /* [annotation][in] */ 
            _In_  REFGUID InterfaceName,
            /* [annotation][out] */ 
            _Out_  LARGE_INTEGER *pUMDVersion) = 0;
        
    };
```

- DXGIFactory
	DXGI 객체를 생성한다. 해당 인터페이스를 활용하여 DXGI 디바이스, DXGI 스왑 체인 등을 생성할 수 있다. 
```c++
 IDXGIFactory : public IDXGIObject
    {
    public:
        virtual HRESULT STDMETHODCALLTYPE EnumAdapters( 
            /* [in] */ UINT Adapter,
            /* [annotation][out] */ 
            _COM_Outptr_  IDXGIAdapter **ppAdapter) = 0;
        
        virtual HRESULT STDMETHODCALLTYPE MakeWindowAssociation( 
            HWND WindowHandle,
            UINT Flags) = 0;
        
        virtual HRESULT STDMETHODCALLTYPE GetWindowAssociation( 
            /* [annotation][out] */ 
            _Out_  HWND *pWindowHandle) = 0;
        
        virtual HRESULT STDMETHODCALLTYPE CreateSwapChain( 
            /* [annotation][in] */ 
            _In_  IUnknown *pDevice,
            /* [annotation][in] */ 
            _In_  DXGI_SWAP_CHAIN_DESC *pDesc,
            /* [annotation][out] */ 
            _COM_Outptr_  IDXGISwapChain **ppSwapChain) = 0;
        
        virtual HRESULT STDMETHODCALLTYPE CreateSoftwareAdapter( 
            /* [in] */ HMODULE Module,
            /* [annotation][out] */ 
            _COM_Outptr_  IDXGIAdapter **ppAdapter) = 0;
        
    };
```