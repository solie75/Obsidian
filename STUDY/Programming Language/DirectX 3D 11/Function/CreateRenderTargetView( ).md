ID3D11Device::CreateRenderTargetView  method\

## Syntax

```c++
HRESULT CreateRenderTargetView( 
	[in] ID3D11Resource *pResource, 
	[in, optional] const D3D11_RENDER_TARGET_VIEW_DESC *pDesc, 
	[out, optional] ID3D11RenderTargetView **ppRTView );
```

## Parameters

1. ID3D11Resource *pResource : render target 을 표편할 ID3D11Resource 를 가리키는 포인터이다. 이 리소스는 D3D11_BIND_RENDER_TARGET flag 를 사용하여 생성되어야만 한다. 
2. const D3D11_RENDER_TARGET_VIEW_DESC *pDesc : render target view 의 설정 구조체인 D3D11_RENDER-TARGET_VIEW_DESC을 가리키는 포인터이다. 레벨 0의 밉맵의 모든 subresource  를 접근하는 view 를 만들기 위해서는  이 매개변수가 NULL 로 설정한다.
3. ID3D11RenderTargetView **ppRTView : ID3D11RenderTargetView 를 가리키는 포인터의 주소이다. 다른 입력 매개변수를 유효성을 검사 하기 위해서 이 매개변수를 NULL 로 설정한다.(만약 다른 매개변수가 유효성 검사를 통과 하면 이 메서드는 S_FALSE를 반환한다.)

## Return value

HRESULT


