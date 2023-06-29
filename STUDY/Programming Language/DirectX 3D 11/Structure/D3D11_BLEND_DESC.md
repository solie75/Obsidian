[ID3D11Device::CreateBlendState](https://learn.microsoft.com/ko-kr/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createblendstate)를 호출하여 혼합 상태 개체를 만드는 데 사용하는 혼합 상태를 설명합니다.

```c++
typedef struct D3D11_BLEND_DESC { 
	BOOL AlphaToCoverageEnable;
	BOOL IndependentBlendEnable;
	D3D11_RENDER_TARGET_BLEND_DESC RenderTarget[8]; 
} D3D11_BLEND_DESC;
```

BOOL AlphaToCoverageEnable : Specifies whether to use alpha-to-coverage as a multisampling technique when setting a pixel to a render target

BOOL IndependentBlendEnable : Specifies whether to enable independent blending in simultaneous render targets. Set to **TRUE** to enable independent blending. If set to **FALSE**, only the RenderTarget[0] members are used

D3D11_RENDER_TARGET_BLEND_DESC RenderTarget[8] : 렌더링 대상의 블렌드 상태를 설명하는 [D3D11_RENDER_TARGET_BLEND_DESC](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/ns-d3d11-d3d11_render_target_blend_desc) 구조 의 배열입니다 . [이는 출력 병합기 단계](https://learn.microsoft.com/en-us/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage) 에 한 번 에 바인딩할 수 있는 8개의 렌더링 대상에 해당한다.

