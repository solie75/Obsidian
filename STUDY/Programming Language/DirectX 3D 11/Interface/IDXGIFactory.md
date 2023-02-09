IDXGIFactory 인터페이스는 전체 화면으로 전환에 대한 조정 과 같은 DXGI object 들을 generate 하기 위한 메서드를 시행한다.

IDXGIFactory 는 IDXGIObject 를 상속받는다.

# Methods

### 1. CreateSoftwareAdaptor
[IDXGIFactory::CreateSoftwareAdapter](https://learn.microsoft.com/en-us/windows/win32/api/dxgi/nf-dxgi-idxgifactory-createsoftwareadapter)

### 2. CreateSwapChain

#### Syntax
```c++
HRESULT CreateSwapChain( 
	[in] IUnknown *pDevice, 
	[in] DXGI_SWAP_CHAIN_DESC *pDesc, 
	[out] IDXGISwapChain **ppSwapChain 
);
```

1. pDevice
DirectX 3D 11 이전에서는 Swap Chain 에 대한 Direct3D device 를 가리키는 포인터 였지만 Direct3D 12 이후로는 queue 명령어를 직접적으로 가리키는 포인터이다. 이 매개변수는 NULL 이 될 수 없다.

2. pDesc
swap chain desc 에 대한 DXGI_SWAP_CHAIN_DESC 구조체를 가리키는 포인터.

3. ppSwapChain
생성된 swap chain 이 저장될 실제 주소.

### Remark

전체 화면이 비활성화 상태에서 전체 화면으로 swap chain 을 생성하려고 시도하는 경우 windowed 모드로 생성되고 DXGI_STATUS_OCCLUDED 가 반환된다.

buffer 의 너비 혹은 높이가 0 인 경우, swap chain desc 의 window size 로부터 추론된 사이즈로 생성된다.

swap chain 이 생성될 때 target output 이 명시적으로 결정되지 않기 때문에 full screen swap chain 으로 생성하지 않는 것이 좋다. 이것은 swap chain 의 크기와 출력되는 window의 사이즈가 같지 않을 때 창을 출력하는 것이 제거 될 수 있다.

사이즈를 맞추는 두 가지 방법
	1. windowed 모드로 swap chain 을 생성하고 IDXGISwapChain::SetFullscreenState 를 사용하여 전체 화면으로 설정한다.
	2. swap chain 이 생성되자마자 그 포인터를 저장하고 WM_SIZE event 가 진행되는 동안 출력 window 크기를 가져온다. 그 다음, windowed 에서 full screen 으로 전환되는 동안 ( [IDXGISwapChain::ResizeBuffers](https://learn.microsoft.com/en-us/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers) 을 사용하여) swap chain buffer 의 크기를 바꾼다. 

swap chain 이 full screen 모드인 경우, 그것을 release 하기 전에 windowed 모드로 바꾸기 위해서는 반드시 [SetFullscreenState](https://learn.microsoft.com/en-us/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-setfullscreenstate) 를 사용해야 한다. For more information about releasing a swap chain, see the "Destroying a Swap Chain" section of [DXGI Overview](https://learn.microsoft.com/en-us/windows/desktop/direct3ddxgi/d3d10-graphics-programming-guide-dxgi).

런타임이 처음 프레임을 full screen 으로 렌더링 한 후에, 런타임은 [IDXGISwapChain::Present](https://learn.microsoft.com/en-us/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) 를 호출하는 동안 예상치 못하게 full screen 을 종료 할 수 있다. 이를 방지하기 위해서 다음의 코드를 CreateSwapChain 바로 다음에 실행하는 것이 추천된다. 
```c++
// Detect if newly created full-screen swap chain isn't actually full screen. 
IDXGIOutput* pTarget; 
BOOL bFullscreen; 

if (SUCCEEDED(pSwapChain->GetFullscreenState(&bFullscreen, &pTarget))) 
{ 
	pTarget->Release(); 
} else 
{
	bFullscreen = FALSE;
}
 // If not full screen, enable full screen again. 
if (!bFullscreen) 
{ 
	ShowWindow(hWnd, SW_MINIMIZE); 
	ShowWindow(hWnd, SW_RESTORE); 
	pSwapChain->SetFullscreenState(TRUE, NULL); 
}
```

You can specify [DXGI_SWAP_EFFECT](https://learn.microsoft.com/en-us/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_effect) and [DXGI_SWAP_CHAIN_FLAG](https://learn.microsoft.com/en-us/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_chain_flag) values in the swap-chain description that _pDesc_ points to. These values allow you to use features like flip-model presentation and content protection by using pre-Windows 8 APIs.

However, to use stereo presentation and to change resize behavior for the flip model, applications must use the [IDXGIFactory2::CreateSwapChainForHwnd](https://learn.microsoft.com/en-us/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforhwnd) method. Otherwise, the back-buffer contents implicitly scale to fit the presentation target size; that is, you can't turn off scaling.

※ Starting with Direct3D 11.1, we recommend not to use **CreateSwapChain** anymore to create a swap chain. Instead, use [CreateSwapChainForHwnd](https://learn.microsoft.com/en-us/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforhwnd), [CreateSwapChainForCoreWindow](https://learn.microsoft.com/en-us/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow), or [CreateSwapChainForComposition](https://learn.microsoft.com/en-us/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcomposition) depending on how you want to create the swap chain.

### 3. EnumAdapters
[IDXGIFactory::EnumAdapters](https://learn.microsoft.com/en-us/windows/win32/api/dxgi/nf-dxgi-idxgifactory-enumadapters)

### 4. GetWindowAssociation
[IDXGIFactory::GetWindowAssociation](https://learn.microsoft.com/en-us/windows/win32/api/dxgi/nf-dxgi-idxgifactory-getwindowassociation)

### 5. MakeWindowAssociation
[IDXGIFactory::MakeWindowAssociation](https://learn.microsoft.com/en-us/windows/win32/api/dxgi/nf-dxgi-idxgifactory-makewindowassociation)