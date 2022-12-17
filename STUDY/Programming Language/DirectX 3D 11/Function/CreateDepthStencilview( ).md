ID3D11Device::CreateDepthStencilView  method

## Syntax

```c++
HRESULT CreateDepthStencilView( 
	[in] ID3D11Resource *pResource, 
	[in, optional] const D3D11_DEPTH_STENCIL_VIEW_DESC *pDesc, 
	[out, optional] ID3D11DepthStencilView **ppDepthStencilView 
);
```

## Parameters

1. ID3D11Resource *pResource : <span style="color:Yellow">Depth-Stencil surface 역할을 하는 리소스를 가리키는 포인터</span>. 이 리소스는 반드시 D3D11_BIND_DEPTH_STENCIL flag를 가지고 생성되어야 한다.
2. const D3D11_DEPTH_STENCIL_VIEW_DESC *pDesc : depth-stencil-view 설명 구조를 가리키는 포인터. 모든 리소스의 mipmap level 0 에 접근하는 view 를 생성하기 위해서는 이 매개변수에 null 을 설정한다.
3. ID3D11DepthStencilView **ppDepthStencilView : ID3D11DepthStencilView 를 가리지는 주소. 다른 매개변수의 입력의 유효성을 검사하려면 이 매개변수에 NULL 을 설정한다. (만약 다른 입력된 메서드가 유효성 검사를 통과했다면 이 메서드는 S_FALSE 를 반환한다.)

# Return value

HRESULT

# Remarks

depth-stencil view는 ID3D11DevicecContext::OMSetRenderTargets를 호출함으로서 output-merger stage 에 바인딩 할 수 있다.
