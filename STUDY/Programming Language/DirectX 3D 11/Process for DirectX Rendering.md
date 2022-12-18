## Create CEngine and Manager
1. 작업 영역 크기 조절 및 창 생성 / SetWindowPos(), ShowWindow()
2. Manager 생성 및 초기화 / ManagerInit()

## Create ' Device, Context, SwapChain, RenderTargetView '

1. Create CDevice class
2. Device, Context, SwapChain, Rendertarget Texture, Rendertarget View, DepthStencil Texture, DepthStencil View 에 대한 변수 선언.
3. DeviceInit( )
	1. [[D3D11CreateDevice( )]] 로 Device와 Context 생성
	2. CDevice::CreateSwapChain( ) 호출
		1. DXGI_SWAP_CHAIN_DESC 구조체 설정
		2. D3D11CreateDevice( )에서 생성된 디바이스인 m_Device 에 대해 QueryInterface( )호출하여 IDXGIDevice형 객체(DXGI)의 포인터를 가져온다.
		3. IDXGIDevice 형 객체에 대하여 GetParent( )를 호출해, IDXGIAdapter 형 객체(adapter)의 포인터를 가져온다.
		4. IDXGIAdapter형 객체에 대하여 GetParent( )를 호출해, IDXGIFactory 형 객체의 포인터를 가져온다.
		5. IDXGIFactory형 객체에 대하여 IDXGIFactory::[[CreateSwapChain( )]] 을 호출한다.
		6. CreateSwapChain() 함수로 미리 선언해 둔, IDXGISwapChain 형 객체에 스왑체인을 저장한다.
	3. CDevice::CreateView( ) 호출
		1. Rendertarget View 생성
			1. 스왑체인 객체에 대해 [[GetBuffer( )]]를 호출한다. 이를 통해서 RenderTargetTexture 용으로 쓰기 위해 미리 선언해둔 객체에 Texture2D 를 생성하여 대입한다.
			2. 디바이스 객체 에 대해 [[CreateRenderTargetView( )]] 를 호출하여 rendertarget  용 객체에 view 를 생성하여 대입한다.
		2. Depthstencil View 생성
			1. D3D11_TEXTURE2D_DESC 구조체 변수 생성 및 DepthStencil  용도에 맞게 내용 대입
			2. [[CreateTexture2D( )]] 로 depth Stencil 용 텍스쳐를 생성한다.
			3. [[CreateDepthStencilview( )]]로 Depth Stencil 용 view 를 생성한다.
4. 출력 타겟 설정
	1. 

### Process

1. 
6. [[CreateRenderTargetView( )]]
7. [[CreateDepthStencilview( )]]
8. 