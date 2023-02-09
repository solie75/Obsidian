## Syntax

```c++
typedef struct DXGI_SWAP_CHAIN_DESC { 
	DXGI_MODE_DESC BufferDesc; 
	DXGI_SAMPLE_DESC SampleDesc; 
	DXGI_USAGE BufferUsage; 
	UINT BufferCount; 
	HWND OutputWindow; 
	BOOL Windowed; 
	DXGI_SWAP_EFFECT SwapEffect; 
	UINT Flags; } DXGI_SWAP_CHAIN_DESC;
```

1. BufferDesc
backbuffer display mode 를 설명하는 [[DXGI_MODE_DESC]] 구조체
2. SampleDesc
멀티 샘플링 매개변수를 설명하는 DXGI_SAMPLE_DESC 구조체
3. BufferUsage
surface 의 용도와 back buffer 에 대한 CPU 접근 설정을 설명하는 DXGI_USAGE  열거형 형식의 맴버.
4. BufferCount
swap chain 내의 버퍼의 개수. full screen swap chain 을 생성하기 위해서 IDXGIFactory::CreateSwapChain 을 호출하면, 보통 이 값에 front buffer 를 포함한다.
5. OutputWindow
출력 윈도우의 HWND 핸들. 절대 NULL 이면 안된다.
6. Windowed
출력이 windowed 모드인지를 특정하는 boolean 값.  True 일때 윈도우는 windowed 모드이다.
7. SwapEffect
surface 표시 이후 표시할 버퍼의 내용을 다루는 것에 대한 설정을 설명하는 DXGI_SWAP_EFFECT 열거형 형식 의 맴버이다.
8. Flags
swap-chain이 수행하는 것에 대한 설정을 설명하는 DXGI_SWAP_CHAIN_FLAG 열거형 형식의 맴버이다.

# Remark

이 구조체는 GetDesc 와 CreateSwapChian 메서드에 의해 사용된다.

Full-screen 모드에서는 전용 front buffer 가 있고 windowed 모드에서는 desktop이 front buffer 이다.

하나의 버퍼로 swap chain 을 생성하는 경우, DXGI_SWAP_EFFECT_SEQUENTIAL 을 특정하는 것이 단일 buffer 가 front buffer 와 스왑되는 것을 야기하지 않는다.

