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

1. UINT NumViews : 바인딩 할 render targets의 수 (0에서 D3D11_SIMULTANEOUS_RENDER_TARGET_COUNT). 만약 이 매개변수가 0이 아니라면, ppRenderTargetViews의 포인터가 가리키는 배열의 항목의 개수는 이 매개변수의 수와 같아야 한다. 최대 8개의 렌더타겟을 설정할 수 있다
2. ID3D11RenderTargetView * const *ppRenderTargetViews : <span style="color: yellow">디바이스에 바인딩 할  render target을 나타내는 ID3D11RenderTargetView의 배열을 가리키는 포인터</span>이다. 이 매개변수가 NULL 이고 NumViews 가 0이면 render target 은 바인딩 되지 않는다.
3. ID3D11DepthStencilView *pDepthStencilView : <span style="color: yellow">디바이스에 바인딩할 depth_stencil view를 표현하는 ID3D11DepthStencilView를 가리키는 포인터</span>이다.

## Return Value

없음

## Remarks

디바이스가 정해진 시간 내에 활성화(active) 할 수 있는 최대 active render target의 수는 D3D11_SIMULTANEOUS_RENDER_TARGET_COUNT 라는 D3D11.h의 #define 을 호출함으로서 설정된다. 동일한 subresource 를 다수의 render target slots 에 설정하는 것은 유효하지 않다. 이 호출에 의해 정의되지 않는 모든 render target은 NULL 로 설정된다. 
만약 어떤 subresource 가 다른 stage 에서 읽기 위해서 혹은 작성하기 위해서 현재 바인딩 되어있다면 (아마도 파이프 라인의 다른 부분에서), 하나의 렌더링 작업 에서 같은 suvresource가 동시에 읽히거나 작성되는 것을 방지하기 위해서 이러한 바인딩 된 point(지점) 들은 NULL 로 설정된다. 

그 메서드는 전달된 인터페이스에 대한 참조를 가지고 있는다.

If the render-target views were created from an array resource type, all of the render-target views must have the same array size. (<span style="color:green ">array resource type 은 뭘까</span>). 
이 제한은 또한 depth-stencil view 에도 적용된다, 그것의 배열 사이즈는 바인딩 되는 render-target view와 같아야 한다.

픽셀 쉐이더는 적어도 여덟개의 분별된 render target을 동시에 랜더할 수 있어야 한다. 모든 이러한 render-target 은 같은 종류의 리소스에 access 해야 한다. [Buffer](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-buffer), [Texture1D](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture1d), [Texture1DArray](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture1darray), [Texture2D](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2d), [Texture2DArray](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2darray), [Texture3D](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture3d), or [TextureCube](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-to-type). 모든 render target은 모든 dimention(너비, 높이, 3D 에서는 깊이 또는 배열 타입에서는 배열 크기) 에서 같은 크기여야 한다. 만약 render-target이 multisample anti-aliasing을 사용하면, 모든 바인딩 된 render target과 depth buffer는 multisample resource 와 같은 형식이어야 한다. (그말인 즉슨, 그 sample의 수가 같아야 한다는 것이다.) 각 render target은 다른 데이터 포맷을 가질 수 있다. 이러한 render target 포맷은 모든 요소당 비트 수가 동일할 필요가 없다. 

어떤 render target의 8개의 slot들의 조합(combination)은 render target 을 설정할 수도 설정하지 않을 수도 있다.(<span style="color:green ">slot 이란</span>)

같은 resource view 는 동시에 multiple render target slot 에 바인딩 될 수 없다. 그러나, single resource의 multiple non-overlapping resource view를 동시에 multiple render target으로서 설정할 수 있다. 