어떻게 두 개 이상의 렌더링이나 디스플레이에 사용되는 버퍼를 encapsulates(요약)하는 swap chain 을 생성하는가

swapchain은 대체로 display device 에 수여되는 front buffer 와 render target 의 역할을 하는 back buffer 를 포함하고 있다. Immediate context가 back buffer 에 대한 렌더링을 끝낸 후에, swap chain 은 back buffer를 두 버퍼를 스와핑 함으로서 표시한다.
예를 들어 두개의 버퍼를 가지는 swap chain 이 있다고 가정할 때 하나는 front buffer 이고 남은 하나는 back buffer 이다. 이때 back buffer 에 immediate context가 렌더링을 하고 그것이 끝나면 해당 back buffer 를 front buffer로 변경한다. 또한  반대로  기존의 front buffer 는 back buffer 가 된다. 그리고 front buffer 를 출력하는 것이다. ^64e5f9

Swap chain 은 아래의 몇가지 랜더링 특징을 포함하여 정의한다.
1. render area 크기
2. 디스플레이 주사율
3. The display mode
4. The surface format

DXGI_SWAP_CHAIN_DESC 구조체를 채우고 IDXGISwapchian interface 를 초기화 함으로서 swap chain  의 특성을 정의한다.  IDXGIFactory::CreateSwapChain 이나 D3D11CreateDeviceAndSwapChain 을 호출하여 swap chain 을 초기화 한다.

### Create a device and swap chain

 device 와 swap chain 을 초기화 하기 위해서 아래 두 방법 중 하나를 진행
  1.  D3D11CreateDeviceAndSwapChain( ) 함수를 사용하여 swap chain 과 device 를 동시에 초기화 한다. 
  2. IDXGIFactory::CreateSwapChain( ) 을 사용하여 미리  swap chain 을 소유한 상태에서[[D3D11CreateDevice( )]] 함수를 사용한다.

## Syntax

```c++
HRESULT CreateSwapChain(
	[in] IUnknown *pDevice,
	[in] DXGI_SWAP_CHAIN_DESC *pDesc, 
	[out] IDXGISwapChain **ppSwapChain
);
```

## Parameters

1. IUnknown *pDevice : 스왑 체인에 대한 Direct 3D 디바이스를 가리키는 포인터. NULL 일 수 없다.
2. DXGI_SWAP_CHAIN_DESC *pDesc : 스왑체인의 설정을 위한 DXGI_SWAP_CHAIN_DESC 구조체를 가리키는 포인터. NULL일 수 없다.
3. IDXGISwapChain **ppSwapChain : CreateSwapChain() 이 생성하는 swap chain에 대한 IDXGISwapChian 인터페이스 를 가리키는 포인터를 받아들이는 변동가능한 포인터.

## Return value

HRESULT

## Remarks

