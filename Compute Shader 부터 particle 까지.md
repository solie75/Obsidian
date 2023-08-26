## 텍스쳐의 구조 변경

다음 추가
```c++
// CTexture.h
private:
	Microsoft::WRL::ComPtr<ID3D11RenderTargetView> mRTV;
	Microsoft::WRL::ComPtr<ID3D11DepthStencilView> mDSV;
	Microsoft::WRL::ComPtr<ID3D11UnorderedAccessView> mUAV;

public:

Microsoft::WRL::ComPtr<ID3D11RenderTargetView> GetRTV() { return mRTV; }
	void SetRTV(Microsoft::WRL::ComPtr<ID3D11RenderTargetView> rtv) { mRTV = rtv; }

	Microsoft::WRL::ComPtr<ID3D11DepthStencilView> GetDSV() { return  mDSV; }
	void SetDSV(Microsoft::WRL::ComPtr<ID3D11DepthStencilView> dsv) { mDSV = dsv; }

	Microsoft::WRL::ComPtr<ID3D11ShaderResourceView> GetSRV() { return  mSRV; }
	Microsoft::WRL::ComPtr<ID3D11UnorderedAccessView> GetUAV() { return  mUAV; }

	Microsoft::WRL::ComPtr<ID3D11Texture2D> GetTexture() { return mTexture; }
	void SetTexture(Microsoft::WRL::ComPtr<ID3D11Texture2D> texture) { mTexture = texture; }

	bool Create(UINT width, UINT height, DXGI_FORMAT format, UINT bindFlag);

// CTexture.cpp

bool CTexture::Create(UINT width, UINT height, DXGI_FORMAT format, UINT bindFlag)
{
	if (mTexture == nullptr)
	{
		mDesc.BindFlags = bindFlag;
		mDesc.Usage = D3D11_USAGE_DEFAULT;
		mDesc.CPUAccessFlags = 0;
		mDesc.Format = format;
		mDesc.Width = width;
		mDesc.Height = height;
		mDesc.ArraySize = 1;

		mDesc.SampleDesc.Count = 1;
		mDesc.SampleDesc.Quality = 0;

		mDesc.MipLevels = 0;
		mDesc.MiscFlags = 0;

		if (!CDevice::GetInst()->GetDevice()->CreateTexture2D(&mDesc, nullptr, mTexture.GetAddressOf()))
		{
			return false;
		}
	}

	if (bindFlag & (UINT)D3D11_BIND_FLAG::D3D11_BIND_DEPTH_STENCIL) // 비트 and 연산자
	{
		if (!CDevice::GetInst()->CreateDepthStencilView(mTexture.Get(), nullptr, mDSV.GetAddressOf()))
		{
			return false;
		}
	}

	if (bindFlag & (UINT)D3D11_BIND_FLAG::D3D11_BIND_SHADER_RESOURCE)
	{
		D3D11_SHADER_RESOURCE_VIEW_DESC tSRVDesc = {};
		tSRVDesc.Format = mDesc.Format;
		tSRVDesc.Texture2D.MipLevels = 1;
		tSRVDesc.Texture2D.MostDetailedMip = 0;
		tSRVDesc.ViewDimension = D3D11_SRV_DIMENSION::D3D11_SRV_DIMENSION_TEXTURE2D;

		if (!CDevice::GetInst()->CreateShaderResourceView(mTexture.Get(), &tSRVDesc, mSRV.GetAddressOf()))
		{
			return false;
		}
	}

	if (bindFlag & (UINT)D3D11_BIND_FLAG::D3D11_BIND_RENDER_TARGET)
	{
		D3D11_RENDER_TARGET_VIEW_DESC tSRVDesc = {};
		tSRVDesc.Format = mDesc.Format;
		tSRVDesc.Texture2D.MipSlice = 0;
		tSRVDesc.ViewDimension = D3D11_RTV_DIMENSION::D3D11_RTV_DIMENSION_TEXTURE2D;

		if (!CDevice::GetInst()->CreateRenderTargetView(mTexture.Get(), nullptr, mRTV.GetAddressOf()))
		{
			return false;
		}
	}

	if (bindFlag & (UINT)D3D11_BIND_FLAG::D3D11_BIND_UNORDERED_ACCESS)
	{
		D3D11_UNORDERED_ACCESS_VIEW_DESC tUAVDesc = {};
		tUAVDesc.Format = mDesc.Format;
		tUAVDesc.Texture2D.MipSlice = 0;
		tUAVDesc.ViewDimension = D3D11_UAV_DIMENSION::D3D11_UAV_DIMENSION_TEXTURE2D;
	
		if (!CDevice::GetInst()->CreateUnorderedAccessView(mTexture.Get(), &tUAVDesc, mUAV.GetAddressOf()))
		{
			return false;
		}
	}

	return true;
}
```


----
* UnorderedAccessView 란

ID3D11UnorderedAccessView는 DirectX 11 그래픽스 API에서 사용되는 인터페이스 중 하나입니다. 이 인터페이스는 언오더드 액세스 뷰(Unordered Access View, UAV)를 나타내며, 그래픽스 파이프라인의 출력 리소스에 대한 읽기 및 쓰기 액세스를 제공하는 뷰입니다. UAV는 GPU에서 병렬적으로 데이터를 읽고 쓰는 데 사용되며, GPGPU(General-Purpose GPU) 작업이나 계산 작업을 수행하는 데 주로 활용됩니다.

UAV는 다음과 같은 목적으로 사용됩니다:

1. **병렬 계산 및 연산 작업**: UAV는 여러 스레드 또는 스레드 그룹에서 동시에 작업을 수행하도록 설계되어 병렬적인 계산 작업에 유용합니다. 이를 통해 그래픽스 파이프라인의 일부가 아닌 별도의 연산 작업을 수행하거나, 계산 작업을 그래픽 작업과 병렬로 실행할 수 있습니다.

2. **텍스처 작업**: UAV는 텍스처 데이터에 접근하여 텍스처의 값을 읽거나 쓸 수 있습니다. 이를 통해 복잡한 텍스처 연산을 수행하거나 텍스처 데이터를 수정할 수 있습니다.

3. **병렬적인 렌더링 작업**: UAV를 통해 병렬적으로 렌더링 결과를 쓰는 작업을 수행할 수 있습니다. 예를 들어, 계산 쉐이더를 사용하여 파티클 시스템을 업데이트하고 결과를 UAV에 쓸 수 있습니다.

4. **병렬적인 데이터 가공**: UAV는 병렬적인 데이터 변환 및 처리 작업을 수행할 수 있습니다. 예를 들어, 복잡한 데이터 가공 작업을 쉐이더로 처리하고 그 결과를 UAV에 쓸 수 있습니다.

ID3D11UnorderedAccessView 인터페이스는 D3D11 SDK(DirectX 11 Software Development Kit)에서 제공되며, 다양한 유형의 리소스(버퍼, 텍스처 등)에 대한 UAV를 생성하고 사용하는 데 필요한 메서드와 속성을 제공합니다.

----

- ID3D11Device::CreateTexture2D() 란

`ID3D11Device::CreateTexture2D()` 함수는 DirectX 11 그래픽스 API에서 사용되는 함수 중 하나입니다. 이 함수는 2D 텍스처(Texture) 객체를 생성하는 데 사용됩니다. 2D 텍스처는 이미지나 데이터의 2차원 표현을 저장하는 데 사용되며, 주로 그래픽스 작업에서 텍스처 매핑(Texture Mapping)과 렌더링에 활용됩니다.

`CreateTexture2D()` 함수는 다음과 같이 선언됩니다:

```cpp
HRESULT CreateTexture2D(
  const D3D11_TEXTURE2D_DESC *pDesc,
  const D3D11_SUBRESOURCE_DATA *pInitialData,
  ID3D11Texture2D **ppTexture2D
);
```

매개변수 설명:

- `pDesc`: 생성할 텍스처의 속성을 설명하는 `D3D11_TEXTURE2D_DESC` 구조체 포인터입니다. 이 구조체는 텍스처의 차원, 크기, 형식 등을 지정합니다.
- `pInitialData`: 초기 데이터가 포함된 `D3D11_SUBRESOURCE_DATA` 구조체 포인터입니다. 텍스처의 초기 데이터를 제공하려는 경우에 사용됩니다.
- `ppTexture2D`: 생성된 텍스처 객체의 포인터가 저장될 포인터입니다.

`D3D11_TEXTURE2D_DESC` 구조체는 텍스처의 속성을 지정하는 데 사용됩니다. 일부 중요한 속성은 다음과 같습니다:

- `Width`, `Height`: 텍스처의 너비와 높이를 지정합니다.
- `MipLevels`: Mipmap 레벨의 수를 지정합니다.
- `ArraySize`: 텍스처 배열의 크기를 지정합니다.
- `Format`: 텍스처의 픽셀 형식을 지정합니다.
- `Usage`: 텍스처의 사용 용도를 지정합니다(예: 정적, 동적, 스테이징 등).
- `BindFlags`: 텍스처를 어떤 리소스에 바인딩할지 지정합니다(예: 렌더 타겟, 셰이더 리소스 등).

`D3D11_SUBRESOURCE_DATA` 구조체는 텍스처의 초기 데이터를 지정하는 데 사용됩니다. 초기 데이터를 제공하지 않거나, 텍스처를 생성 후에 데이터를 업로드할 경우 이 매개변수는 NULL로 설정할 수 있습니다.

`ID3D11Texture2D` 포인터는 생성된 2D 텍스처 객체를 가리키게 됩니다. 이 객체를 사용하여 그래픽스 파이프라인에서 텍스처를 사용할 수 있습니다.

예시 코드:

```cpp
D3D11_TEXTURE2D_DESC textureDesc = {};
textureDesc.Width = 1024;
textureDesc.Height = 1024;
textureDesc.MipLevels = 1;
textureDesc.ArraySize = 1;
textureDesc.Format = DXGI_FORMAT_R8G8B8A8_UNORM;
textureDesc.SampleDesc.Count = 1;
textureDesc.Usage = D3D11_USAGE_DEFAULT;
textureDesc.BindFlags = D3D11_BIND_SHADER_RESOURCE;

ID3D11Texture2D *pTexture2D = nullptr;
HRESULT hr = pDevice->CreateTexture2D(&textureDesc, nullptr, &pTexture2D);
```

위 예시는 1024x1024 크기의 RGBA 형식 텍스처를 생성하는 코드입니다.

----

- D3D11_BIND_FLAG 란

D3D11_BIND_FLAG는 DirectX 11 그래픽스 API에서 사용되는 열거형 상수입니다. 이 상수는 그래픽스 리소스(텍스처, 버퍼 등)가 어떤 용도로 바인딩될 수 있는지를 지정하기 위해 사용됩니다. 리소스를 특정 용도에 바인딩함으로써 그래픽스 파이프라인의 다양한 단계에서 해당 리소스를 사용할 수 있습니다.

다음은 일부 D3D11_BIND_FLAG 상수와 해당하는 바인딩 용도입니다:

1. `D3D11_BIND_VERTEX_BUFFER`: 정점 버퍼로 바인딩되며, 정점 데이터를 입력으로 사용합니다.
2. `D3D11_BIND_INDEX_BUFFER`: 인덱스 버퍼로 바인딩되며, 그래픽스 파이프라인에서 삼각형 인덱스를 참조하는 데 사용됩니다.
3. `D3D11_BIND_CONSTANT_BUFFER`: 상수 버퍼로 바인딩되며, 쉐이더에 상수 데이터를 전달하는 데 사용됩니다.
4. `D3D11_BIND_SHADER_RESOURCE`: 셰이더 리소스로 바인딩되며, 텍스처나 버퍼 데이터를 셰이더에서 읽을 수 있게 합니다.
5. `D3D11_BIND_RENDER_TARGET`: 렌더 타겟으로 바인딩되며, 렌더링 작업의 결과를 저장하는 데 사용됩니다.
6. `D3D11_BIND_DEPTH_STENCIL`: 깊이-스텐실 버퍼로 바인딩되며, 깊이 정보와 스텐실 정보를 저장하는 데 사용됩니다.
7. `D3D11_BIND_UNORDERED_ACCESS`: 언오더드 액세스 뷰(Unordered Access View, UAV)로 바인딩되며, 병렬적인 읽기와 쓰기 작업에 사용됩니다.

각 바인딩 플래그는 특정한 용도에 해당하는 리소스의 바인딩을 나타냅니다. 리소스를 생성할 때 해당하는 바인딩 플래그를 지정함으로써 리소스가 어떤 용도로 사용될 수 있는지를 정의할 수 있습니다. 예를 들어, 텍스처 리소스를 셰이더에서 읽기 위해 사용할 때는 `D3D11_BIND_SHADER_RESOURCE`를, 렌더 타겟으로 사용할 때는 `D3D11_BIND_RENDER_TARGET`를 지정할 수 있습니다.

----

-  D3D11_SHADER_RESOURCE_VIEW_DESC  란

`D3D11_SHADER_RESOURCE_VIEW_DESC`는 DirectX 11 그래픽스 API에서 사용되는 구조체입니다. 이 구조체는 셰이더 리소스 뷰(Shader Resource View, SRV)를 생성하거나 설정하기 위해 사용됩니다. SRV는 텍스처나 버퍼와 같은 리소스를 셰이더에서 읽을 수 있도록 하는 뷰입니다.

`D3D11_SHADER_RESOURCE_VIEW_DESC` 구조체의 주요 멤버들은 다음과 같습니다:

1. `Format`: SRV로 읽을 데이터의 형식을 지정합니다. 예를 들어, 텍스처의 픽셀 형식을 나타냅니다.
2. `ViewDimension`: SRV의 뷰 차원을 지정합니다. 텍스처인지 버퍼인지 여부를 지정합니다.
3. `Texture1D`, `Texture1DArray`, `Texture2D`, `Texture2DArray`, `Texture3D`, `TextureCube`, `TextureCubeArray`: 각 뷰 차원에 대한 정보를 담고 있는 구조체입니다. 예를 들어, `Texture2D` 멤버는 2D 텍스처에 대한 정보를 포함합니다.
4. `Buffer`: 버퍼에 대한 SRV를 설정할 때 사용되는 구조체입니다.
5. `BufferEx`: 확장된 버퍼 SRV 설정에 사용되는 구조체입니다.

SRV는 다양한 종류의 리소스를 셰이더에서 읽기 위해 사용됩니다. 예를 들어, 텍스처 데이터를 셰이더에서 샘플링하거나, 버퍼 데이터를 셰이더에서 읽어들일 때 SRV를 사용합니다. 이 때, `D3D11_SHADER_RESOURCE_VIEW_DESC` 구조체를 사용하여 SRV를 생성하고 해당하는 리소스와 연결합니다.

예시 코드:

```cpp
D3D11_SHADER_RESOURCE_VIEW_DESC srvDesc = {};
srvDesc.Format = DXGI_FORMAT_R8G8B8A8_UNORM; // 예시 형식
srvDesc.ViewDimension = D3D11_SRV_DIMENSION_TEXTURE2D; // 2D 텍스처
srvDesc.Texture2D.MostDetailedMip = 0;
srvDesc.Texture2D.MipLevels = -1; // 모든 미하계층 사용

ID3D11ShaderResourceView* pShaderResourceView = nullptr;
HRESULT hr = pDevice->CreateShaderResourceView(pTexture2D, &srvDesc, &pShaderResourceView);
```

위 코드는 2D 텍스처에 대한 SRV를 생성하는 예시입니다. `Texture2D` 멤버를 사용하여 SRV에 필요한 정보를 설정한 후, `CreateShaderResourceView` 함수를 사용하여 실제로 SRV를 생성합니다.

----

- D3D11_SRV_DIMENSION 란

`D3D11_SRV_DIMENSION`은 DirectX 11 그래픽스 API에서 사용되는 열거형 상수입니다. 이 상수는 셰이더 리소스 뷰(Shader Resource View, SRV)의 뷰 차원을 지정합니다. SRV는 텍스처나 버퍼와 같은 그래픽스 리소스를 셰이더에서 읽을 수 있도록 하는 뷰입니다. `D3D11_SRV_DIMENSION`은 SRV의 뷰 차원을 지정하여 어떤 유형의 리소스가 사용되는지를 명시적으로 나타냅니다.

일부 `D3D11_SRV_DIMENSION` 상수 및 해당하는 뷰 차원은 다음과 같습니다:

1. `D3D11_SRV_DIMENSION_BUFFER`: 버퍼에 대한 SRV를 나타냅니다.
2. `D3D11_SRV_DIMENSION_TEXTURE1D`: 1D 텍스처에 대한 SRV를 나타냅니다.
3. `D3D11_SRV_DIMENSION_TEXTURE1DARRAY`: 1D 텍스처 배열에 대한 SRV를 나타냅니다.
4. `D3D11_SRV_DIMENSION_TEXTURE2D`: 2D 텍스처에 대한 SRV를 나타냅니다.
5. `D3D11_SRV_DIMENSION_TEXTURE2DARRAY`: 2D 텍스처 배열에 대한 SRV를 나타냅니다.
6. `D3D11_SRV_DIMENSION_TEXTURE2DMS`: 멀티샘플링 2D 텍스처에 대한 SRV를 나타냅니다.
7. `D3D11_SRV_DIMENSION_TEXTURE2DMSARRAY`: 멀티샘플링 2D 텍스처 배열에 대한 SRV를 나타냅니다.
8. `D3D11_SRV_DIMENSION_TEXTURE3D`: 3D 텍스처에 대한 SRV를 나타냅니다.
9. `D3D11_SRV_DIMENSION_TEXTURECUBE`: 큐브 맵 텍스처에 대한 SRV를 나타냅니다.
10. `D3D11_SRV_DIMENSION_TEXTURECUBEARRAY`: 큐브 맵 텍스처 배열에 대한 SRV를 나타냅니다.

이러한 `D3D11_SRV_DIMENSION` 상수는 각각 특정한 종류의 리소스에 대한 SRV 뷰 차원을 지정하는 데 사용됩니다. 예를 들어, `D3D11_SRV_DIMENSION_TEXTURE2D`는 2D 텍스처에 대한 SRV 뷰 차원을 나타내며, 해당하는 `D3D11_SHADER_RESOURCE_VIEW_DESC` 구조체의 멤버인 `ViewDimension`에 사용됩니다.

SRV 뷰 차원을 올바르게 설정하면 셰이더에서 리소스를 올바르게 읽어올 수 있게 되며, 각 차원에 맞는 설정을 통해 다양한 종류의 리소스를 활용할 수 있습니다.

----

- D3D11_SHADER_RESOURCE_VIEW_DESC 의 요소 중 Texture2D.MostDetailedMip 이란?

`MostDetailedMip`은 DirectX 11 그래픽스 API에서 사용되는 `D3D11_SHADER_RESOURCE_VIEW_DESC` 구조체의 멤버 중 하나입니다. 이 멤버는 셰이더 리소스 뷰(Shader Resource View, SRV)를 생성할 때 텍스처의 어느 Mipmap 레벨부터 사용할 것인지를 지정하는 데 사용됩니다.

Mipmap은 텍스처의 크기를 다양한 해상도로 줄여서 저장하는 기술입니다. 이를 통해 원본 텍스처보다 더 작은 해상도의 텍스처를 사용하여 성능을 향상시키거나 멀리 떨어진 객체에 사용되는 텍스처를 간단한 모양으로 표현하는 데 사용됩니다.

`MostDetailedMip` 멤버는 어느 Mipmap 레벨부터 시작해서 SRV로 읽어올 것인지를 지정합니다. 값은 0부터 시작하며, 0은 가장 상세한 (원본 크기) Mipmap 레벨을 나타냅니다. 낮은 Mipmap 레벨의 값을 사용하면 성능 향상을 가져올 수 있습니다. 예를 들어, 멀리 떨어진 물체에 대한 텍스처 사용 시에는 높은 Mipmap 레벨의 텍스처를 사용하면서 성능을 개선할 수 있습니다.

`MostDetailedMip` 값을 설정할 때, 특정 Mipmap 레벨을 기준으로 그 이상의 레벨도 함께 사용되므로 주의가 필요합니다. 가령, `MostDetailedMip` 값을 1로 설정하면 1번 Mipmap 레벨부터 시작하여 모든 더 낮은 레벨의 텍스처가 사용됩니다.

다음은 `D3D11_SHADER_RESOURCE_VIEW_DESC`를 사용하여 SRV를 생성할 때 `MostDetailedMip` 값을 설정하는 예시 코드입니다:

```cpp
D3D11_SHADER_RESOURCE_VIEW_DESC srvDesc = {};
srvDesc.Format = DXGI_FORMAT_R8G8B8A8_UNORM; // 예시 형식
srvDesc.ViewDimension = D3D11_SRV_DIMENSION_TEXTURE2D; // 2D 텍스처
srvDesc.Texture2D.MostDetailedMip = 0; // 가장 상세한 Mipmap 레벨을 사용

ID3D11ShaderResourceView* pShaderResourceView = nullptr;
HRESULT hr = pDevice->CreateShaderResourceView(pTexture2D, &srvDesc, &pShaderResourceView);
```

위 코드에서 `MostDetailedMip` 값이 0으로 설정되었습니다. 이는 텍스처의 가장 상세한 Mipmap 레벨부터 사용할 것임을 나타냅니다.

----

위의 Texture 클라스 의 변경에 따라 CDevice 에서 관리되던 view 의 형태도 바뀐다.

```c++
기존의 CDevice 의 view 관리

ComPtr<ID3D11Texture2D> mRenderTarget;
ComPtr<ID3D11RenderTargetView> mRenderTargetView;
ComPtr<ID3D11Texture2D> mDepthStencilBuffer;
ComPtr<ID3D11DepthStencilView> mDepthStencilView;

이후의 view 관리

std::shared_ptr<CTexture> mRenderTarget;
std::shared_ptr<CTexture> mDepthStencil;
```

이후로 기존의 view 를 필요로 하는 부분에는 mRenderTarget->GetSRV() 등으로 대체한다.

또한 RenderTargetView, ComputeShader, DepthStencilView, UnorderedAccessView , ShaderResourceView 등을 생성하는 함수를 추가한다.
```c++
// CDevice.h
bool CreateDepthStencilView(ID3D11Resource* pResource, const D3D11_DEPTH_STENCIL_VIEW_DESC* pDesc, ID3D11DepthStencilView** ppDepthStencilView);

bool CreateShaderResourceView(ID3D11Resource* pResource, const D3D11_SHADER_RESOURCE_VIEW_DESC* pDesc, ID3D11ShaderResourceView** ppSRView);
	
bool CreateRenderTargetView(ID3D11Resource* pResource, const D3D11_RENDER_TARGET_VIEW_DESC* pDesc, ID3D11RenderTargetView** ppRTView);
	
bool CreateUnorderedAccessView(ID3D11Resource* pResource, const D3D11_UNORDERED_ACCESS_VIEW_DESC* pDesc, ID3D11UnorderedAccessView** ppUAView);
	
bool CrateComputeShader(const void* pShaderBytecode, SIZE_T BytecodeLength, ID3D11ComputeShader** ppComputeShader);

void BindUnorderedAccess(UINT slot, ID3D11UnorderedAccessView** ppUnorrderedAccessViews, const UINT* pUAVInitialCounts);

void BindComputeShader(ID3D11ComputeShader* pComputeShader);

void Dispatch(UINT ThreadGroupCountX, UINT ThreadGroupCountY, UINT ThreadGroupCountZ);


// CDevice.cpp
bool CDevice::CreateDepthStencilView(ID3D11Resource* pResource, const D3D11_DEPTH_STENCIL_VIEW_DESC* pDesc, ID3D11DepthStencilView** ppDepthStencilView)
{
	if (FAILED(mDevice->CreateDepthStencilView(pResource, pDesc, ppDepthStencilView)))
	{
		return false;
	}

	return true;
}

bool CDevice::CreateShaderResourceView(ID3D11Resource* pResource, const D3D11_SHADER_RESOURCE_VIEW_DESC* pDesc, ID3D11ShaderResourceView** ppSRView)
{
	if (FAILED(mDevice->CreateShaderResourceView(pResource, pDesc, ppSRView)))
	{
		return false;
	}

	return true;
}

bool CDevice::CreateRenderTargetView(ID3D11Resource* pResource, const D3D11_RENDER_TARGET_VIEW_DESC* pDesc, ID3D11RenderTargetView** ppRTView)
{
	if (FAILED(mDevice->CreateRenderTargetView(pResource, pDesc, ppRTView)))
	{
		return false;
	}
	return true;
}

bool CDevice::CreateUnorderedAccessView(ID3D11Resource* pResource, const D3D11_UNORDERED_ACCESS_VIEW_DESC* pDesc, ID3D11UnorderedAccessView** ppUAView)
{
	if (FAILED(mDevice->CreateUnorderedAccessView(pResource, pDesc, ppUAView)))
	{
		return false;
	}

	return true;
}

bool CDevice::CrateComputeShader(const void* pShaderBytecode, SIZE_T BytecodeLength, ID3D11ComputeShader** ppComputeShader)
{
	if (FAILED(mDevice->CreateComputeShader(pShaderBytecode, BytecodeLength, nullptr, ppComputeShader)))
	{
		return false;
	}

	return true;
}

void CDevice::BindUnorderedAccess(UINT slot, ID3D11UnorderedAccessView** ppUnorrderedAccessViews, const UINT* pUAVInitialCounts)
{
	mContext->CSSetUnorderedAccessViews(slot, 1, ppUnorrderedAccessViews, pUAVInitialCounts);
}

void CDevice::BindComputeShader(ID3D11ComputeShader* pComputeShader)
{
	mContext->CSSetShader(pComputeShader, 0, 0);
}

void CDevice::Dispatch(UINT ThreadGroupCountX, UINT ThreadGroupCountY, UINT ThreadGroupCountZ)
{
	mContext->Dispatch(ThreadGroupCountX, ThreadGroupCountY, ThreadGroupCountZ);
}

```

Graphic->GraphicShader 폴더에 ComputeShader 폴더 생성 후 해당 폴더에 CComputeShader 를 추가한다.

```c++
// CComputeShader.h
#pragma once
#include "CResource.h"
class CComputeShader :
    public CResource
{
private:
    Microsoft::WRL::ComPtr<ID3DBlob> mCSBlob;
    Microsoft::WRL::ComPtr<ID3D11ComputeShader> mCS;

public:
    CComputeShader();
    virtual ~CComputeShader();

    bool Create(const std::wstring& name, const std::string& methodName);
    virtual HRESULT Load(const std::wstring& path) { return S_FALSE; };
};
```

```c++
// ComputeShader.cpp
#include "CComputeShader.h"
#include "CDevice.h"

CComputeShader::CComputeShader()
    : CResource(eResourceType::ComputeShader)
{
}

CComputeShader::~CComputeShader()
{
}

bool CComputeShader::Create(const std::wstring& name, const std::string& methodName)
{
    std::filesystem::path shaderPath = std::filesystem::current_path().parent_path();
    shaderPath += L"\\Shader\\";

    std::filesystem::path fullPath(shaderPath.c_str());
    fullPath += name;

    ID3DBlob* errorBlob = nullptr;
    CDevice::GetInst()->CompileFromfile(fullPath, methodName, "cs_5_0", mCSBlob.GetAddressOf());
    CDevice::GetInst()->CrateComputeShader(mCSBlob->GetBufferPointer(), mCSBlob->GetBufferSize(), mCS.GetAddressOf());

    return false;
}
```

ComputeShader 에 대한 hlsl 추가

```c++
// PaintCS.hlsl
RWTexture2D<float4> tex : register(u0);

//https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/sv-dispatchthreadid
// SV_GroupID           : 스레드가 속한 그룹의 좌표
// SV_GoupThreadID      : 그룹 내에서, 스레드의 좌표
// SV_GoupIndex         : 그룹 내에서, 스레드의 인덱스 (1차원)
// SV_DispatchThreadID  : 전체 스레드(모든 그룹 통합) 기준으로, 호출된 스레드의 좌표

[numthreads(32, 32, 1)] // 그룹 당 스레드 개수 (최대 1024 까지 지정 가능)

void main(uint3 DTid : SV_DispatchThreadID)
{
    if (DTid.x >= 1024 || DTid.y >= 1024)
    {
        return;
    }

    tex[DTid.xy] = float4(0.f, 0.f, 1.f, 1.f);
}
```

TestScene 에 적용

```c++
std::shared_ptr<CPaintShader> paintShader = CResourceMgr::GetInst()->Find<CPaintShader>(L"PaintShader"); // paintShader 가 empty 이다.
	std::shared_ptr<CTexture> paintTexture = CResourceMgr::GetInst()->Find<CTexture>(L"PaintTexture");
	paintShader->SetTarget(paintTexture);
	paintShader->OnExcute();

CMonster* Smile = new CMonster();
	AddGameObject(eLayerType::Monster, Smile, L"Smile", Vector3(0.0f, 0.0f, 1.0003f)
		, Vector3(1.0f, 1.0f, 0.0f), true, L"Mesh", L"mt_Smile", false);
	CMeshRender* mr = Smile->GetComponent<CMeshRender>(eComponentType::MeshRender);
	std::shared_ptr<CMaterial> mt = mr->GetMaterial();
	mt->SetTexture(paintTexture);
	int a = 0;
```

## Particle 구현


Header.hlsli 에 Paricle 구조체 추가
``` hlsl
// Header.hlsli
struct Particle
{
    float4 position;
    float4 direction;
    
    float endTime;
    float time;
    float speed;
    uint active;
};
```

Particle hlsl 추가

```c++
// ParticleVS.hlsl
#include "Header.hlsli"

struct VSIn
{
    float3 pos : POSITION;
    uint Instance : SV_InstanceID;
};

struct VSOut
{
    float4 pos : SV_Position;
};

VSOut main(VSIn In)
{
    VSOut Out = (VSOut) 0.0f;
    float4 worldPos = mul(float4(In.pos, 1.0f), WorldMatrix);
    worldPos.xyz += particles[In.Instance].position.xyz;
    
    float4 viewPos = mul(worldPos, ViewMatrix);
    Out.pos = mul(viewPos, ProjectionMatrix);

    return Out;
}
```

```c++
// ParticlePS.hlsl

#include "Header.hlsli"

struct VSIn
{
    float3 pos : POSITIONT;
    uint Instance : SV_InstanceID;
};

struct VSOut
{
    float4 pos : SV_POSITION;
};

float4 main(VSOut In) : SV_Target
{
    float4 Out = (float4) 0.0f;
    
    Out = float4(1.0f, 0.0f, 1.0f, 1.0f);
    
    return Out;

}
```

Graphic.h 에 Paricle 구조체 추가

```c++
// Graphic.h
struct Particle
{
	Vector4 position;
	Vector4 direction;

	float endTime;
	float time;
	float speed;
	UINT active;
};

```

RenderMgr.cpp 에서 particle 로 shader 와 material 만들기

```c++
// CRenderMgr.cpp
void CRenderMgr::Init()
{
...
	std::shared_ptr<CPaintShader> paintShader = std::make_shared<CPaintShader>();
	paintShader->Create(L"PaintCS.hlsl", "main");
	CResourceMgr::GetInst()->Insert(L"PaintShader", paintShader);

	std::shared_ptr<CShader> particleShader = std::make_shared<CShader>();
	particleShader->CreateShader(eShaderStage::VS, L"ParticleVS.hlsl", "main");
	particleShader->CreateShader(eShaderStage::PS, L"ParticlePS.hlsl", "main");
	particleShader->SetRSType(eRSType::SolidNone);
	particleShader->SetDSType(eDSType::NoWrite);
	particleShader->SetBSType(eBSType::AlphaBlend);
	particleShader->CreateInputLayout();
	CResourceMgr::GetInst()->Insert(L"ParticleShader", particleShader);

	{
		LoadMaterial(particleShader, L"Particle", eRenderingMode::Transparent);
	}
}

```

CMesh 에서 DrawIndexedInstance 추가

```c++
// CMesh.h
void RenderInstanced(UINT startIndexLocation);

// CMesh.cpp
void Mesh::RenderInstanced(UINT startIndexLocation)
	{
		GetDevice()->DrawIndexedInstanced(mIndexCount, startIndexLocation, 0, 0, 0);
	}
```

----

`DrawIndexedInstanced`는 DirectX 11 그래픽스 API에서 사용되는 함수 중 하나로, 인덱스 버퍼를 이용하여 정점 데이터를 그리는 데 사용되는 함수입니다. 이 함수를 사용하여 삼각형 메쉬와 같은 3D 개체를 그릴 수 있습니다.

`DrawIndexedInstanced` 함수는 다음과 같이 선언됩니다:

```cpp
void DrawIndexedInstanced(
  UINT IndexCountPerInstance,
  UINT InstanceCount,
  UINT StartIndexLocation,
  INT BaseVertexLocation,
  UINT StartInstanceLocation
);
```

매개변수 설명:

- `IndexCountPerInstance`: 각 인스턴스에 그려질 인덱스의 수를 지정합니다. 인덱스 버퍼에서 읽을 인덱스의 개수입니다.
- `InstanceCount`: 그릴 인스턴스 개수를 지정합니다. 동일한 정점 데이터를 여러 개 그릴 때 사용됩니다.
- `StartIndexLocation`: 인덱스 버퍼에서 시작할 인덱스의 위치를 지정합니다.
- `BaseVertexLocation`: 정점 데이터에서 시작할 정점의 위치를 지정합니다.
- `StartInstanceLocation`: 시작할 인스턴스의 위치를 지정합니다. 일반적으로 0부터 시작됩니다.

`DrawIndexedInstanced` 함수는 인덱스 버퍼와 정점 버퍼에 있는 데이터를 기반으로 정점들을 그리는 데 사용됩니다. 각 인덱스는 버텍스 데이터의 위치를 가리키며, `InstanceCount`로 지정한 횟수만큼 동일한 메쉬를 그릴 수 있습니다.

예를 들어, 여러 개의 삼각형을 그릴 때, 각 삼각형을 인덱스 버퍼와 정점 버퍼를 이용하여 그릴 수 있습니다. `DrawIndexedInstanced` 함수를 사용하여 각 삼각형을 그릴 수 있으며, `InstanceCount`를 이용하여 동일한 메쉬를 여러 번 그릴 수 있습니다.

```cpp
// 예시 코드
UINT indexCount = ...;  // 인덱스 개수
UINT instanceCount = ...;  // 그릴 인스턴스 개수

// DrawIndexedInstanced 함수 호출
pDeviceContext->DrawIndexedInstanced(indexCount, instanceCount, 0, 0, 0);
```

위 코드에서 `DrawIndexedInstanced` 함수는 지정한 인덱스 개수와 인스턴스 개수에 맞게 데이터를 그리게 됩니다.

----

Particle 클라스 추가

``` c++
// CParticleSystem.h
#pragma once
#include "CMeshRender.h"


class CParticleSystem :
    public CMeshRender
{
private:
    CStructedBuffer* mBuffer;

    UINT mCount;
    Vector4 mStartSize;
    Vector4 mEndSize;
    Vector4 mStartColor;
    Vector4 mEndColor;
    float mLifeTime;

public:
    CParticleSystem();
    ~CParticleSystem();

    virtual void Initialize() override;
    virtual void Update() override;
    virtual void LateUpdate() override;
    virtual void Render() override;
};


```

```c++
// CParticleSystem.cpp
#include "CParticleSystem.h"

CParticleSystem::CParticleSystem()
	: mCount(0)
	, mStartSize(Vector4::One)
	, mEndSize(Vector4::One)
	, mStartColor(Vector4::Zero)
	, mEndColor(Vector4::Zero)
	, mLifeTime(0.0f)
{
	std::shared_ptr<CMesh> mesh = CResourceMgr::GetInst()->Find<CMesh>(L"Mesh");
	SetMesh(mesh);

	std::shared_ptr<CMaterial> material = CResourceMgr::GetInst()->Find<CMaterial>(L"mt_Particle");
	SetMaterial(material);

	Particle particles[1000] = {};
	for (size_t i = 0; i < 1000; i++)
	{
		Vector4 pos = Vector4::Zero;
		pos.x += rand() % 20;
		pos.y += rand() % 10;

		// 50퍼센트의 확률로 x 와 y 값을 음수 일지 양수 일지 정한다.
		int sign = rand() % 2; // 50% 확률로 0 또는 1이다.
		if (sign == 0)
		{
			pos.x *= -1.0f;
		}
		sign = rand() % 2;
		if (sign == 0)
		{
			pos.y *= -1.0f;
		}

		particles[i].position = pos; 
	}

	mBuffer = new CStructedBuffer();
	mBuffer->CreateStructedBuffer(sizeof(Particle), 1000, eSRVType::None);
	mBuffer->SetData(particles, 1000);
}

CParticleSystem::~CParticleSystem(){}

void CParticleSystem::Initialize(){}

void CParticleSystem::Update(){}

void CParticleSystem::LateUpdate(){}

void CParticleSystem::Render()
{
	CTransform* tr = this->GetOwner()->GetComponent<CTransform>(eComponentType::Transform);
	tr->CreateConstantBuffer();
	CRenderMgr::GetInst()->BindConstantBuffer(eShaderStage::VS, tr->GetTransformCB());

	mBuffer->Bind(eShaderStage::VS, 14);
	mBuffer->Bind(eShaderStage::PS, 14);

	GetMaterial()->Bind();
	GetMesh()->RenderInstanced(1000);
}
```

----

`rand()` 함수는 C 및 C++ 프로그래밍 언어에서 표준 라이브러리(`stdlib.h`)에 포함된 난수 생성 함수입니다. 이 함수를 사용하여 의사 난수(pseudorandom number)를 생성할 수 있습니다. 의사 난수는 실제로는 사전에 정해진 알고리즘을 사용하여 생성되며, 무작위로 보이지만 시작 값인 시드(seed)에 의해 결정됩니다.

`rand()` 함수는 호출될 때마다 이전에 생성된 의사 난수를 기반으로 새로운 의사 난수를 생성합니다. 따라서 같은 시드를 가지고 여러 번 호출하면 항상 같은 순서의 난수가 생성됩니다. 의사 난수 생성기를 초기화하려면 `srand()` 함수를 사용하여 시드 값을 설정해야 합니다.

다음은 `rand()` 함수의 사용 예시입니다:

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main() {
    // 시드 값을 현재 시간으로 초기화
    srand(time(NULL));

    for (int i = 0; i < 10; i++) {
        int randomNum = rand();  // 0부터 RAND_MAX 사이의 의사 난수 생성
        printf("%d\n", randomNum);
    }

    return 0;
}
```

위 코드에서 `srand(time(NULL))`를 사용하여 시드 값을 현재 시간으로 설정한 후, `rand()` 함수를 사용하여 0부터 `RAND_MAX` 사이의 의사 난수를 생성합니다. `RAND_MAX`는 `stdlib.h`에 정의된 상수로, 생성될 수 있는 최대 의사 난수 값입니다.

하지만 주의할 점은 `rand()` 함수의 의사 난수는 특정 알고리즘에 기반한 것이기 때문에 보안적으로 강력한 난수가 필요한 경우에는 다른 방법을 고려해야 합니다. 보안 용도로 사용되는 경우에는 보다 안전한 난수 생성기를 사용하는 것이 좋습니다.

의사 난수 란 : 난수는 아니나 난수로 취급이 가능한 수열을 지칭한다.

----


TestScene 에 적용

```c++
// TestScene.cpp
CGameObject* particle = new CGameObject();
	particle->SetName(L"Particle");
	AddGameObject(eLayerType::Monster, particle, L"Particle", Vector3(0.0f, 0.0f, 1.0f), Vector3(0.2f, 0.2f, 0.2f), false, L"", L"", false);
	CParticleSystem* pts = particle->AddComponent<CParticleSystem>();
```

결과

![[Pasted image 20230826141950.png]]


## geometry shader 추가

기존의 ParticleVS.hlsl 과 ParticlePS.hlsl 을 Geometry 에 맞게 변경

```hlsl
// ParticleVS.hlsl

#include "Header.hlsli"

struct GSOut
{
    float4 Pos : SV_Position;
    float2 UV : TEXCOORD;
};

//float4 main(VSOut In) : SV_Target
float4 main(GSOut In) : SV_TARGET
{
    float4 Out = (float4) 0.0f;
    
    Out = float4(1.0f, 0.0f, 1.0f, 1.0f);
    
    return Out;

}

// ParticlePS.hlsl

#include "Header.hlsli"

struct GSOut
{
    float4 Pos : SV_Position;
    float2 UV : TEXCOORD;
};

//float4 main(VSOut In) : SV_Target
float4 main(GSOut In) : SV_TARGET
{
    float4 Out = (float4) 0.0f;
    
    Out = float4(1.0f, 0.0f, 1.0f, 1.0f);
    
    return Out;

}
```

ParticleGS.hlsl 추가

```c++
// ParticleGS.hlsl

#include "Header.hlsli"

struct VSOut
{
    float4 LocalPos : SV_Position;
    uint Instance : SV_InstanceID;
};

struct GSOut
{
    float4 Pos : SV_Position;
    float2 UV : TEXCOORD;
    // uint Instance : SV_InstanceID;
};

[maxvertexcount(6)]
void main(point VSOut In[1], inout TriangleStream<GSOut> output)
{
    GSOut Out[4] = { (GSOut) 0.0f, (GSOut) 0.0f, (GSOut) 0.0f, (GSOut) 0.0f };
    
    if (particles[In[0].Instance].active == 0)
    {
        return;
    }
    
    float3 worldPos = (In[0].LocalPos.xyz) + particles[In[0].Instance].position.xyz;
    
    float3 viewPos = mul(float4(worldPos, 1.0f), ViewMatrix).xyz;
    
    float3 NewPos[4] =
    {
        viewPos - float3(-0.5f, 0.5f, 0.f) * float3(0.2f, 0.2f, 1.f),
        viewPos - float3(0.5f, 0.5f, 0.f) * float3(0.2f, 0.2f, 1.f),
        viewPos - float3(0.5f, -0.5f, 0.f) * float3(0.2f, 0.2f, 1.f),
        viewPos - float3(-0.5f, -0.5f, 0.f) * float3(0.2f, 0.2f, 1.f)
    };
    
    for (int i = 0; i < 4; ++i)
    {
        Out[i].Pos = mul(float4(NewPos[i], 1.0f), ProjectionMatrix);
    }

    Out[0].UV = float2(0.0f, 0.0f);
    Out[1].UV = float2(1.0f, 0.0f);
    Out[2].UV = float2(1.0f, 1.0f);
    Out[3].UV = float2(0.0f, 1.0f);
    
    // 0 -- 1
    // | \  |
    // 3 -- 2
    output.Append(Out[0]);
    output.Append(Out[1]);
    output.Append(Out[2]);
    output.RestartStrip();
    
    output.Append(Out[0]);
    output.Append(Out[2]);
    output.Append(Out[3]);
    output.RestartStrip();
}
```

CTransform 에서 TransformCB 를 여러 shaderStage 에 바인딩한다.

```c++
// CTransform.cpp

기존의 CreateConstantBuffer();

void CTransform::CreateConstantBuffer()
{
	TransformCB trCB = {};
	trCB.mWorld = mWorld;
	trCB.mView = CCamera::GetStaticViewMatrix();
	trCB.mProjection = CCamera::GetStaticProjectionMatrix();

	mTransformCB->InitConstantBuffer(sizeof(TransformCB), eCBType::Transform, &trCB);
	CRenderMgr::GetInst()->BindConstantBuffer(eShaderStage::VS, mTransformCB);
}


변경 후의 CreateConstantBuffer();

void CTransform::CreateConstantBuffer()
{
	TransformCB trCB = {};
	trCB.mWorld = mWorld;
	trCB.mView = CCamera::GetStaticViewMatrix();
	trCB.mProjection = CCamera::GetStaticProjectionMatrix();

	mTransformCB->InitConstantBuffer(sizeof(TransformCB), eCBType::Transform, &trCB);
	CRenderMgr::GetInst()->BindConstantBuffer(eShaderStage::VS, mTransformCB);
	CRenderMgr::GetInst()->BindConstantBuffer(eShaderStage::HS, mTransformCB);
	CRenderMgr::GetInst()->BindConstantBuffer(eShaderStage::DS, mTransformCB);
	CRenderMgr::GetInst()->BindConstantBuffer(eShaderStage::GS, mTransformCB);
	CRenderMgr::GetInst()->BindConstantBuffer(eShaderStage::PS, mTransformCB);
}

```

CDevice class 에서 Bind Shader 함수 추가

```c++
// CDevice.h

	void BindHullShader(ID3D11HullShader* pHullShader);
	void BindDomainShader(ID3D11DomainShader* pDomainShader);
	void BindGeometryShader(ID3D11GeometryShader* pGeometryShader);

// CDevice.cpp

void CDevice::BindHullShader(ID3D11HullShader* pHullShader)
{
	mContext->HSSetShader(pHullShader, 0, 0);
}

void CDevice::BindDomainShader(ID3D11DomainShader* pDomainShader)
{
	mContext->DSSetShader(pDomainShader, 0, 0);
}

void CDevice::BindGeometryShader(ID3D11GeometryShader* pGeometryShader)
{
	mContext->GSSetShader(pGeometryShader, 0, 0);
}
```

CDevice class 에서 Geometry shader 생성 함수 추가
```c++
// CDevice.h

bool CreateGeometryShader(const void* pShaderBytecode, SIZE_T BytecodeLength, ID3D11GeometryShader** ppGeometryShader);

// CDevice.cpp

bool CDevice::CreateGeometryShader(const void* pShaderBytecode, SIZE_T BytecodeLength, ID3D11GeometryShader** ppGeometryShader)
{
	if (FAILED(mDevice->CreateGeometryShader(pShaderBytecode, BytecodeLength, nullptr, ppGeometryShader)))
	{
		return false;
	}

	return true;
}
```

CShader 에서 CreateShader 에서 GS 경우 추가

```c++
// CShader.cpp

CShader::CreateShader()
{
	...
	else if (shaderStage == eShaderStage::GS)
	{
		std::filesystem::path psPath = shaderPath;
		psPath += shaderName;

		HRESULT hr = D3DCompileFromFile(psPath.c_str(), nullptr, D3D_COMPILE_STANDARD_FILE_INCLUDE,
			entrypointName.c_str(), "gs_5_0", 0, 0
			, mGSBlob.GetAddressOf(), mErrorBlob.GetAddressOf());

		if (mErrorBlob)
		{
			OutputDebugStringA((char*)mErrorBlob->GetBufferPointer());
			mErrorBlob->Release();
		}

		CDevice::GetInst()->GetDevice()->CreateGeometryShader(mGSBlob->GetBufferPointer()
			, mGSBlob->GetBufferSize(), nullptr, mGS.GetAddressOf());
	}
	...
}
```

CShadfer 에서 Shader Binds 부분

```c++
// CShader.cpp
void CShader::BindsShader()
{
	...
	CDevice::GetInst()->GetContext()->GSSetShader(mGS.Get(), 0, 0);
}
```

CRenderMgr 에서 ParticleShader 생성 부분 수정

```c++
// CRenderMgr::Init();
	std::shared_ptr<CShader> particleShader = std::make_shared<CShader>();
	particleShader->CreateShader(eShaderStage::VS, L"ParticleVS.hlsl", "main");
	particleShader->CreateShader(eShaderStage::PS, L"ParticlePS.hlsl", "main");
	particleShader->CreateShader(eShaderStage::GS, L"ParticleGS.hlsl", "main");
	particleShader->SetRSType(eRSType::SolidNone);
	particleShader->SetDSType(eDSType::NoWrite);
	particleShader->SetBSType(eBSType::AlphaBlend);
	particleShader->SetTopology(D3D11_PRIMITIVE_TOPOLOGY::D3D10_PRIMITIVE_TOPOLOGY_POINTLIST);
	particleShader->CreateInputLayout();
	CResourceMgr::GetInst()->Insert(L"ParticleShader", particleShader);
```

위에 Topology 세팅을 위해 CShader 에서 Topology 관련 함수를 추가하고 코드를 수정한다.

```c++
// CShader.h
private:
	D3D11_PRIMITIVE_TOPOLOGY mTopology;
	...
public:
	void SetTopology(D3D11_PRIMITIVE_TOPOLOGY topology) { mTopology = topology; }
	...

// CShader.cpp
CShader::CShader()
{
	mTopology = D3D11_PRIMITIVE_TOPOLOGY::D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST;
}

void CShader::BindInputLayout()
{
	CDevice::GetInst()->GetContext()->IASetInputLayout(mInputLayout.Get());
	CDevice::GetInst()->GetContext()->IASetPrimitiveTopology(mTopology);
}
```

CParticleSystem 에 GS 요소 추가

```c++
// CParticleSystem.cpp

#include "CParticleSystem.h"

CParticleSystem::CParticleSystem()
	: mCount(0)
	, mStartSize(Vector4::One)
	, mEndSize(Vector4::One)
	, mStartColor(Vector4::Zero)
	, mEndColor(Vector4::Zero)
	, mLifeTime(0.0f)
{
	std::shared_ptr<CMesh> mesh = CResourceMgr::GetInst()->Find<CMesh>(L"PointMesh");
	SetMesh(mesh);

	std::shared_ptr<CMaterial> material = CResourceMgr::GetInst()->Find<CMaterial>(L"mt_Particle");
	SetMaterial(material);

	Particle particles[1000] = {};
	for (size_t i = 0; i < 1000; i++)
	{
		Vector4 pos = Vector4::Zero;
		pos.x += rand() % 20;
		pos.y += rand() % 10;

		// 50퍼센트의 확률로 x 와 y 값을 음수 일지 양수 일지 정한다.
		int sign = rand() % 2; // 50% 확률로 0 또는 1이다.
		if (sign == 0)
		{
			pos.x *= -1.0f;
		}
		sign = rand() % 2;
		if (sign == 0)
		{
			pos.y *= -1.0f;
		}

		particles[i].position = pos; 
		particles[i].active = 1;
	}

	mBuffer = new CStructedBuffer();
	mBuffer->CreateStructedBuffer(sizeof(Particle), 1000, eSRVType::None);
	mBuffer->SetData(particles, 1000);
}

CParticleSystem::~CParticleSystem(){}
void CParticleSystem::Initialize(){}
void CParticleSystem::Update(){}
void CParticleSystem::LateUpdate(){}

void CParticleSystem::Render()
{
	CTransform* tr = this->GetOwner()->GetComponent<CTransform>(eComponentType::Transform);
	tr->CreateConstantBuffer();
	CRenderMgr::GetInst()->BindConstantBuffer(eShaderStage::VS, tr->GetTransformCB());

	mBuffer->Bind(eShaderStage::VS, 14);
	mBuffer->Bind(eShaderStage::GS, 14);
	mBuffer->Bind(eShaderStage::PS, 14);

	GetMaterial()->Bind();
	GetMesh()->RenderInstanced(1000);
}

```

----

Geometry Shader 의 역할

DirectX의 렌더링 파이프라인(Rendering Pipeline)에서 Geometry Shader는 그래픽스 프로그래밍의 한 단계로서 기하 데이터의 조작과 생성을 담당하는 컴퓨트 셰이더 유형 중 하나입니다. Geometry Shader는 정점 데이터를 입력으로 받아서 이를 조작하거나 새로운 정점을 생성하여 변형시키는 역할을 합니다.

Geometry Shader는 렌더링 파이프라인의 여러 단계 중에서 다음과 같은 위치에 위치하게 됩니다:

1. **Input Assembler (IA) Stage**: 이 단계에서는 기하 데이터(정점 데이터)를 읽어 들입니다. 이 데이터는 정점 버퍼에서 가져온 것이며, 각 정점은 위치, 색상, 텍스처 좌표 등의 속성을 가지고 있습니다.

2. **Vertex Shader (VS) Stage**: 기하 데이터가 Vertex Shader로 전달됩니다. 이 단계에서는 기하 데이터를 변형하여 정점의 최종 위치를 계산합니다. 변형, 뷰 변환, 투영 변환 등의 작업이 이루어집니다.

3. **Geometry Shader (GS) Stage**: 이 단계에서 Geometry Shader가 작동합니다. GS는 변형된 정점 데이터를 입력으로 받아서 이를 추가적으로 처리하고, 새로운 정점을 생성하거나 기존의 정점을 수정하는 작업을 수행합니다. 이 단계에서 생성되는 정점들은 기하 도형을 수정하거나 확장하는 데 사용될 수 있습니다.

4. **Rasterizer (RS) Stage**: Geometry Shader에서 생성된 정점들은 클리핑과 래스터화 과정을 거쳐서 화면 공간의 픽셀로 매핑됩니다.

Geometry Shader의 주요 역할은 다음과 같습니다:

1. **기하 도형 변환**: 기하 도형의 위치, 크기 및 회전과 같은 변환을 적용하여 화면 공간에 맞게 변형합니다.
2. **기하 데이터의 생성 및 삭제**: 새로운 정점을 생성하거나 기존의 정점을 삭제하여 기하 도형을 수정하거나 확장할 수 있습니다.
3. **좀 더 복잡한 도형 생성**: 입력으로 들어온 정점들을 이용하여 더 복잡한 기하 도형을 생성할 수 있습니다.
4. **쉐도우 볼륨 생성**: 쉐도우 맵 기능을 구현하기 위해, 라이트의 시야에 들어오는 오브젝트들을 검출하여 그림자의 영역을 계산하고 생성할 수 있습니다.

Geometry Shader는 주로 파티클 시스템, 빛 볼륨 계산, 텔레메트리 데이터 시각화 등 다양한 시나리오에서 활용됩니다.

----

## Particle Compute Shader
( 파티클이 랜덤한 위치에 생성, 각자의 속도를 가지고 지정된 방향으로 이동한다.)

Header.hlsli 에 ParticleSystem cbuffer 추가

```hlsli
cbuffer ParticleSystem : register(b5)
{
    uint elementCout;
    float elapsedTime;
    int padd;
    int padd2;
}

```

ParticleCS.hlsl 추가
```hlsli
#include "Header.hlsli"
RWStructuredBuffer<Particle> ParticleBuffer : register(u0);

[numthreads(128, 1, 1)]
void main( uint3 DTid : SV_DispatchThreadID )
{
    if (elementCout <= DTid.x)
    {
        return;
    }

    ParticleBuffer[DTid.x].position += /*float4(1.0f, 0.0f, 0.0f, 1.0f)*/
    ParticleBuffer[DTid.x].direction * ParticleBuffer[DTid.x].speed * elapsedTime;
}
```

Graphic.h 에 ParticleCB 추가

```c++
// Graphic.h
CBUFFER(ParticleCB, CBSLOT_PARTICLE)
{
	UINT elementCount;
	float elpasedTime;
	int padd;
	int padd2;
};
```

eSRVType 을 eViewType 으로 변경

```c++
// Graphic.h
//enum class eSRVType
//{
//	None,
//	End,
//};

enum class eViewType
{
	None,
	SRV,
	UAV,
	End,
};
```

CParticleShader 추가

```c++
// CParticleShader.h
#pragma once
#include "CComputeShader.h"
#include "CStructedBuffer.h"
#include "CConstantBuffer.h"

class CParticleShader :
    public CComputeShader
{
private:
    CStructedBuffer* mParticleBuffer;
    CConstantBuffer* mParticleCB;

public:
    CParticleShader();
    ~CParticleShader();

    virtual void Binds() override;
    virtual void Clear() override;

    void SetParticleBuffer(CStructedBuffer* particleBuffer);
};

// CParticleShader.cpp
#include "CParticleShader.h"
#include "CTimeMgr.h"
#include "CRenderMgr.h"


CParticleShader::CParticleShader()
	: CComputeShader(128, 1, 1)
	, mParticleBuffer(nullptr)
{
}

CParticleShader::~CParticleShader()
{
}

void CParticleShader::Binds()
{
	mParticleBuffer->BindUAV(0);

	mGroupX = mParticleBuffer->GetStride() / mThreadGroupCountX + 1;
	mGroupY = 1;
	mGroupZ = 1;
}

void CParticleShader::Clear()
{
	mParticleBuffer->Clear();
}

void CParticleShader::SetParticleBuffer(CStructedBuffer* particleBuffer)
{
	mParticleBuffer = particleBuffer;

	static float elapsedTime = 0.0f; // 이 변수가 하는 역할이 뭐길래  static 이지? (elapsed : 경과)
	elapsedTime += CTimeMgr::GetInst()->GetDeltaTime();

	ParticleCB cbData = {};
	cbData.elementCount = mParticleBuffer->GetStride();
	cbData.elpasedTime = CTimeMgr::GetInst()->GetDeltaTime(); // 

	mParticleCB->InitConstantBuffer(sizeof(mParticleCB), eCBType::Particle, &cbData);
	CRenderMgr::GetInst()->BindConstantBuffer(eShaderStage::CS, mParticleCB);
}
```

ComputeShader 의 생성자 추가

```c++
// CComputeShader.h
public:
	CComputeShader(int x, int y, int z);

// CComputeShader.cpp
CComputeShader::CComputeShader(int x, int y, int z)
    : CResource(eResourceType::ComputeShader)
{
    mThreadGroupCountX = x;
    mThreadGroupCountY = y;
    mThreadGroupCountZ = z;
}
```

CRenderMgr 에서 ParticleShader 생성

```c++
// CRenderMgr.cpp
	std::shared_ptr<ParticleShader> psSystemShader = std::make_shared<ParticleShader>();
	psSystemShader->Create(L"ParticleCS.hlsl", "main");
	ya::Resources::Insert(L"ParticleSystemShader", psSystemShader);
```

