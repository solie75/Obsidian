ID3D11Texture2D interface

텍셀 데이터(texel data)를 관리하는 2D 텍스쳐 인터페이스로 메모리로 구성되어 있다.

## Inheritance

 ID3D11Texture2D 인터페이스는 ID3D11Resource 를 상속받는다.

## Remarks

하나의 빈 Texture2D recource 를 생성하기 위해서  ID3D11Device::CreateTexture2D를 호출해야 한다. 
texture는 파이프 라인에 직접적으로 다인딩 될 수 없다. 대신에, view 는 반드시 생성되고 바인딩 되어야 한다. view 를 사용해서, 텍스쳐 데이터는 확실한 제한 내에서 런타임 때에 해석 될 수 있다. 텍스쳐를 render target이나 depth_stencil 리소스 로 사용하기 위해 ID3D11Device::CreateRenderTargetView 그리고 ID3D11Device::CreateDepthStencilView를 각자 호출해야 한다. 쉐이더에 대한 입력으로서 텍스쳐를 사용하기 위해서는 ID3D11Device::CreateShaderResourceView 를 호출하여 생성해 주어야 한다.

