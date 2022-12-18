하나 혹은 그 이상의 render targets 과 depth-stencil buffer 을 output-nerger stage 에 원자적으로(atomically, 프로그램이 중간에 끼어들 수 없음) 바인딩 한다. 

## Syntax

```c++
void OMSetRenderTargets( 
	[in] UINT NumViews, 
	[in, optional] ID3D11RenderTargetView * const *ppRenderTargetViews, 
	[in, optional] ID3D11DepthStencilView *pDepthStencilView 
);
```

## Parameters

1. UINT NumViews : 바인딩 할 render targets의 수 (0에서 D3D11_SIMULTANEOUS_RENDER_TARGET_COUNT). 만약 이 매개변수가 0이 아니라면, 