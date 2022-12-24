하나 혹은 그 이상의 render targets 과 depth-stencil buffer 을 output-merger stage 에 원자적으로(atomically, 프로그램이 중간에 끼어들 수 없음) 바인딩 한다. 

## Syntax

```c++
void OMSetRenderTargets( 
	[in] UINT NumViews, 
	[in, optional] ID3D11RenderTargetView * const *ppRenderTargetViews, 
	[in, optional] ID3D11DepthStencilView *pDepthStencilView 
);
```

## Parameters

1. UINT NumViews : 바인딩 할 render targets의 수 (0에서 D3D11_SIMULTANEOUS_RENDER_TARGET_COUNT). 만약 이 매개변수가 0이 아니라면, ppRenderTargetViews의 포인터가 가리키는 배열의 항목의 개수는 이 매개변수의 수와 같아야 한다.
2. ID3D11RenderTargetView * const *ppRenderTargetViews : 디바이스에 바인딩 할  render target을 나타내는 ID3D11RenderTargetView의 배열을 가리키는 포인터이다. 이 매개변수가 NULL 이고 NumViews 가 0이면 render target 은 바인딩 되지 않는다.
3. ID3D11DepthStencilView *pDepthStencilView : 디바이스에 바인딩할 depth_stencil view를 표현하는 ID3D11DepthStencilView를 가리키는 포인터이다.

## Return Value

없음

## Remarks

디바이스가 정해진 시간 내에 활성화(active) 할 수 있는 최대 active render target의 수는 D3D11_SIMULTANEOUS_RENDER_TARGET_COUNT 라는 D3D11.h의 #define 을 호출함으로서 설정된다. 동일한 subresource 를 다수의 render target slots 에 설정하는 것은 유효하지 않다. 이 호출에 의해 정의되지 않는 모든 render target은 NULL 로 설정된다. 
만약 어떤 subresource 가 다른 stage 에서 읽기 위해서 혹은 작성하기 위해서 현재 바인딩 되어있다면 (아마도 파이프 라인의 다른 부분에서), 하나의 렌더링 작업 에서 같은 suvresource가 동시에 읽히거나 작성되는 것을 방지하기 위해서 이러한 바인딩 된 point(지점) 들은 NULL 로 설정된다. 

그 메서드는 전달된 인터페이스에 대한 참조를 가지고 있는다.

만약 render target view 가 array resoure type 으로 생성된다면 모든 render target view 는 모두 같은 array size (배열 크기)를 가져야 한다. 

-> <span style="color:green ">array resource type 은 뭘까</span>


https://learn.microsoft.com/en-us/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets