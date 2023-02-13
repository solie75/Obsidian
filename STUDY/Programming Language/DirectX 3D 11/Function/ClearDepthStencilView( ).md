ID3D11DeviceContext::ClearDepthStencilView( );

depth-stencil 리소스를 Clear 처리한다.

# Syntax

```c++
void ClearDepthStencilView(
  [in] ID3D11DepthStencilView *pDepthStencilView,
  [in] UINT                   ClearFlags,
  [in] FLOAT                  Depth,
  [in] UINT8                  Stencil
);
```

1. pDepthStencilView
	clear 처리 될 depth stencil 을 가리키는 포인터

2. ClearFlags
	clear 할 data type 을 식별한다.

3. Depth
	이 값으로 depth buffer 를 clear 처리한다. 이 값은 0과 1 사이 에서 고정된다.

4. Stencil
	이 값으로 stencil buffer 를 clear 처리한다.