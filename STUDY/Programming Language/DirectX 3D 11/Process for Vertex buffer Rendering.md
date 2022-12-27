## Create CEngine and Manager
1. 작업 영역 크기 조절 및 창 생성 / SetWindowPos(), ShowWindow()
2. Manager 생성 및 초기화 / ManagerInit()

## Create ' Device, Context, SwapChain, RenderTargetView ' in CEngine::EngineInit()

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
		6. CreateSwapChain() 함수로 미리 선언해 둔, IDXGISwapChain 형 <span style="color: yellow">객체에 스왑체인을 저장한다.</span>
	3. CDevice::CreateView( ) 호출
		1. Rendertarget View 생성
			1. 스왑체인 객체에 대해 [[GetBuffer( )]]를 호출한다. 이를 통해서 RenderTargetTexture 용으로 쓰기 위해 미리 선언해둔 객체에 Texture2D 를 생성하여 대입한다.
			2. 디바이스 객체 에 대해 [[CreateRenderTargetView( )]] 를 호출하여 rendertarget  용 <span style="color: yellow">객체에 view 를 생성</span>하여 대입한다.
		2. Depthstencil View 생성
			1. D3D11_TEXTURE2D_DESC 구조체 변수 생성 및 DepthStencil  용도에 맞게 내용 대입
			2. [[CreateTexture2D( )]] 로 <span style="color: yellow">depth Stencil 용 텍스쳐를 생성</span>한다.
			3. [[CreateDepthStencilview( )]]로 <span style="color: yellow">Depth Stencil 용 view 를 생성한다.</span>
	4. 출력 타겟 설정
		1. [[OMSetRenderTargets( )]]  : 해당 메서드로 Output-Merger stage 에 <span style="color:yellow ">렌더타겟을 설정</span>한다. 
	5. viewport 설정
		1. [[Viewport]] 설정
		2. [[RSSetViewports( )]] 메서드로 뷰포트 배열을 파이프 라인의 rasterizer stage에 바인딩한다.
4. 각 매니저 초기화
5. 정점 버퍼 그리기 테스트(이하 Test)의 초기화
	1. TestInit( ) 호출
		1. struct.h 에 <span style="color: yellow">Vertex 구조체(이하 Vtx)를 추가.</span>
		2. 4개의 Vtx를 가지는 배열(arrVtx)을 생성하고 인데스 0부터 2까지 세팅한다.(각각의 <span style="color: yellow">점의 위치와 색 값을 설정</span>정한다.)
		3. 버퍼 Desc 구조체([[D3D11_BUFFER_DESC]]) 생성 및 set.
		4. 버퍼  Subresource 세팅 : D3D11_SUBRESOURCE_DATA 구조체 변수 선언 D3D11_SUBRESOURCE_DATA::pSysMem에 arrVtx 대입.
		5. [[CreateBuffer( )]]호출로 <span style="color: yellow">버퍼 생성</span> 
		6. shader file 생성 및 경로 가져오기 구현
			-  해당 shader  file 에서는
			1. (float3형 위치, float4형 색 정보를 가진)입력 구조체(VS_IN), (float4형 위치 정보를 가진)출력 구조체(VS_OUT)를 생성한다
			2. vertex shader 프로그램(VS_Test) 와 pixel shader 프로그램(PS_Test)을 생성한다 .(<span style="color: green">프로그램이라고 말하는게 맞는걸까</span>) 이 때 pixel shader 프로그램은  vertex shader 프로그램의  출력 값을 입력값으로 받는다.
		7. 