리소스의 데이터에 접속하기 위한 shader-resource view 를 생성한다.

# Syntax

```c++
HRESULT CreateShaderResourceView(
  [in]            ID3D11Resource  *pResource,
  [in, optional]  const D3D11_SHADER_RESOURCE_VIEW_DESC *pDesc,
  [out, optional] ID3D11ShaderResourceView  **ppSRView
);
```

# Parameters

- pResource
type : ID3D11Resource*

shader 의 입력으로서 사용될 리소스를 가리키는 포인터. 이 리소스는 D3D11_BIND_SHADER_RESOURCE flag 로 생성되어야 한다.

- pDesc
type : const D3D11_SHADER_RESOURCE_VIEW_DESC*

shader-resource view description  을 가리키는 포인터, 이 parameter 를 null 로 설정하여 모든 리소스에 엑서스 할 수 있다. 

- ppSRView
type : ID3D11ShaderResourceView**

ID3D11ShaderResourceView 인터페이스를 가리키는 포인터의 주소. 이 parameter 를 null 처리하여 다른 입력 parameter 를 유효성 검사 한다. (다른 입력 parameter 가 유효성 검사를 통과하면 S_FAIL 을 반환한다.)

# Return value

HRESOULT

# Remarks

리소스는 한개 이상의 subresource 로 만들어진다. view 는 pileline 에 접근하는 것을 허용하는 subresource 를 식별한다.  게다가, 각 리소스는 view 를 사용하여 pipeline 에 바운딩된다. Shader-resource view 는 다음의 API 를 사용하여 Shader stage 에 Buffer 또는 Texture 리소스를 바인딩하기 위해 설계되었다.
[ID3D11DeviceContext::VSSetShaderResources](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshaderresources), [ID3D11DeviceContext::GSSetShaderResources](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-gssetshaderresources) and [ID3D11DeviceContext::PSSetShaderResources](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshaderresources).

Because a view is fully typed, this means that typeless resources become fully typed when bound to the pipeline.

**Note**  To successfully create a shader-resource view from a typeless buffer (for example, [DXGI_FORMAT_R32G32B32A32_TYPELESS](https://learn.microsoft.com/en-us/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)), you must set the [D3D11_RESOURCE_MISC_BUFFER_ALLOW_RAW_VIEWS](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/ne-d3d11-d3d11_resource_misc_flag) flag when you create the buffer.
You can create shader-resource views of video resources so that Direct3D shaders can process those shader-resource views. These video resources are either [Texture2D](https://learn.microsoft.com/en-us/windows/desktop/direct3dhlsl/sm5-object-texture2d) or [Texture2DArray](https://learn.microsoft.com/en-us/windows/desktop/direct3dhlsl/sm5-object-texture2darray). The value in the **ViewDimension** member of the [D3D11_SHADER_RESOURCE_VIEW_DESC](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/ns-d3d11-d3d11_shader_resource_view_desc) structure for a created shader-resource view must match the type of video resource, D3D11_SRV_DIMENSION_TEXTURE2D for Texture2D and D3D11_SRV_DIMENSION_TEXTURE2DARRAY for Texture2DArray. Additionally, the format of the underlying video resource restricts the formats that the view can use. The video resource format values on the [DXGI_FORMAT](https://learn.microsoft.com/en-us/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format) reference page specify the format values that views are restricted to.

The runtime read+write conflict prevention logic (which stops a resource from being bound as an SRV and RTV or UAV at the same time) treats views of different parts of the same video surface as conflicting for simplicity. Therefore, the runtime does not allow an application to read from luma while the application simultaneously renders to chroma in the same surface even though the hardware might allow these simultaneous operations.

# Ref

https://learn.microsoft.com/en-us/windows/win32/api/d3d11/nf-d3d11-id3d11device-createshaderresourceview