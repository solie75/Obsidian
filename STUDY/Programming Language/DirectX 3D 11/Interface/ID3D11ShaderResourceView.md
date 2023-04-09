Shader resource view interface 로 렌더링 중에 shader 가 접근할 수 있는 subrsource 를 특정한다. shader resource 의 예시는 constant buffer, texture buffer, texture 가 있다.

ID3D11View 로부터 상속 받는다. 

# Methods

- ID3D11ShaderResourceView::GetDesc ^e11af7

shader resource view 의 desc 를 가져와 인자로 받은 변수에 저장한다.

- Syntax
```c++
void GetDesc(
  [out] D3D11_SHADER_RESOURCE_VIEW_DESC *pDesc
);
```
pDesc : D3D11_SHADER_RESOURCE_VIEW_DESC 구조체 변수를 가리키는 포인트로 shader resource view 에 대한 데이터로 채워질 것이다.

# Remark

shader resource view 는 resource 를 shader stage 에 바인딩할 때 사용된다. 바인딩은 다음을 호출하여 실행된다.
[ID3D11DeviceContext::GSSetShaderResources](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-gssetshaderresources), [ID3D11DeviceContext::VSSetShaderResources](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshaderresources) or [ID3D11DeviceContext::PSSetShaderResources](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshaderresources).


