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
2. const D3D11_RENDER_TARGET_VIEW_DESC *pDesc : render target view 의 설정 구조체인 D3D11_RENDER-TARGET_VIEW_DESC을 가리키는 포인터이다. <span style="color: yellow">레벨 0의 밉맵의 모든 subresource  를 접근하는 view 를 만들기 위해서는  이 매개변수가 NULL 로 설정</span>한다.
3. ID3D11RenderTargetView **ppRTView : ID3D11RenderTargetView 를 가리키는 포인터의 주소이다. 다른 입력 매개변수를 유효성 검사 하기 위한 경우 이 매개변수를 NULL 로 설정해야 한다..(만약 다른 매개변수가 유효성 검사를 통과 하면 이 메서드는 S_FALSE를 반환한다.)

## Return value

HRESULT

## Remark

A render-target view can be bound to the output-merger stage by calling [ID3D11DeviceContext::OMSetRenderTargets](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets).

The Direct3D 11.1 runtime, which is available starting with Windows 8, allows you to use **CreateRenderTargetView** for the following new purpose.

You can create render-target views of video resources so that Direct3D shaders can process those render-target views. These video resources are either [Texture2D](https://learn.microsoft.com/en-us/windows/desktop/direct3dhlsl/sm5-object-texture2d) or [Texture2DArray](https://learn.microsoft.com/en-us/windows/desktop/direct3dhlsl/sm5-object-texture2darray). The value in the **ViewDimension** member of the [D3D11_RENDER_TARGET_VIEW_DESC](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/ns-d3d11-d3d11_render_target_view_desc) structure for a created render-target view must match the type of video resource, D3D11_RTV_DIMENSION_TEXTURE2D for Texture2D and D3D11_RTV_DIMENSION_TEXTURE2DARRAY for Texture2DArray. Additionally, the format of the underlying video resource restricts the formats that the view can use. The video resource format values on the [DXGI_FORMAT](https://learn.microsoft.com/en-us/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format) reference page specify the format values that views are restricted to.

The runtime read+write conflict prevention logic (which stops a resource from being bound as an SRV and RTV or UAV at the same time) treats views of different parts of the same video surface as conflicting for simplicity. Therefore, the runtime does not allow an application to read from luma while the application simultaneously renders to chroma in the same surface even though the hardware might allow these simultaneous operations.
