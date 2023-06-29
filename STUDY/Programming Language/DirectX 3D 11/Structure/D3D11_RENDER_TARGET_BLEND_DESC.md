Describes the blend state for a render target.

```c++
typedef struct D3D11_RENDER_TARGET_BLEND_DESC {
  BOOL           BlendEnable;
  D3D11_BLEND    SrcBlend;
  D3D11_BLEND    DestBlend;
  D3D11_BLEND_OP BlendOp;
  D3D11_BLEND    SrcBlendAlpha;
  D3D11_BLEND    DestBlendAlpha;
  D3D11_BLEND_OP BlendOpAlpha;
  UINT8          RenderTargetWriteMask;
} D3D11_RENDER_TARGET_BLEND_DESC;
```

BOOL           BlendEnable
	: blending 여부

D3D11_BLEND    SrcBlend
	: 픽셀 쉐이더가 출력하는 RGB 값에 대해 수행할 연산을 특정한다.

D3D11_BLEND    DestBlend
	: 렌더 타겟에 현재 RGB 값에 대해 수행할 연산을 특정한다.

 D3D11_BLEND_OP BlendOp
	 : SrcBlend 와 DestBlend 를 결합하는 방식을 정의한다.

D3D11_BLEND    SrcBlendAlpha
	 : 픽셀 쉐이더가 출력하는 알파 값에 대해 수행할 연산을 특정한다.

D3D11_BLEND    DestBlendAlpha
	: 렌더 타겟의 현재 알파 값에 대해 수행할 연산을 특정한다.

D3D11_BLEND_OP BlendOpAlpha
	: SrcBlendAlpha  와 DestBlendAlpha 를 결합할 방법을 정의한다.

UINT8          RenderTargetWriteMask
	: A write mask.

![[Pasted image 20230630004907.png]]