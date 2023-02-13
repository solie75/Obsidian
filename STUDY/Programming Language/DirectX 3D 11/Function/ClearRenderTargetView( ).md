ID3D11DeviceContext::ClearRenderTargetView( );

하나의 값으로 render target 의 모든 요소를 세팅한다.

# Syntax

```c++
void ClearRenderTargetView(
  [in] ID3D11RenderTargetView*   pRenderTargetView,
  [in] const FLOAT [4]           ColorRGBA
);
```

1. pRenderTargetView
	render target 을 가리키는 포인터.

2. ColorRGBA
	render target 을 채울 색상을 나타내는 4개 component 배열. 