# RasterizerState

###  ID3D11RasterizerState

```c++ 
ID3D11RasterizerState : public ID3D11DeviceChild
{
public:
	virtual void STDMETHODCALLTYPE GetDesc( 
		/* [annotation] */ 
		_Out_  D3D11_RASTERIZER_DESC *pDesc) = 0;
	
};
```

### D3D11_RASTERIZER_DESC

```c++
typedef struct D3D11_RASTERIZER_DESC
{
	D3D11_FILL_MODE FillMode;
	D3D11_CULL_MODE CullMode;
	BOOL FrontCounterClockwise;
	INT DepthBias;
	FLOAT DepthBiasClamp;
	FLOAT SlopeScaledDepthBias;
	BOOL DepthClipEnable;
	BOOL ScissorEnable;
	BOOL MultisampleEnable;
	BOOL AntialiasedLineEnable;
} D3D11_RASTERIZER_DESC;
```
Direct3D 11 그래픽 API에서 래스터라이저(Rasterizer)의 동작을 설정하는 데 사용되는 구조체.
래스터라이저는 3D 모델의 정점(Vertex)들을 픽셀(Pixel) 그리드로 변환하여 래스터화하는 작업을 수행한다.
또한, 그리드 내의 픽셀에 대한 처리 방법, 컬링(Culling), 깊이(Depth) 설정 등을 결정하는데 중요한 역할을 한다.

- Member
1. FillMode (D3D11_FILL_MODE) : 래스터라이저가 그리드 내부에 픽셀을 어떻게 채울지를 결정하는데 사용되는 값
	1. D3D11_FILL_SOLID : 기본값으로, 래스터라이저가 다각형의 내부를 채워 픽셀을 그린다.
	2. D3D11_FILL_WIREFRAME : 래스터라이저가 다각형의 모서리만 그리고 내부를 비워 놓는다.
2. CullMode (D3D11_CULL_MODE) : 래스터라이저가 어느 면을 그릴지 결정하는 방법을 설정하는 값
	1. D3D11_CULL_NONE : Culling 을 사용하지 않는다. 래스터라이저가 모든 삼각형을 그리며, 모델의 양면이 모두 그려진다.
	2. D3D11_CULL_FRONT : 뒷 면을 그리지 않는다. 즉, 모델의 앞 면 만을 그리고 후 면은 제외한다.
	3. D3D11_CULL_BACK : 전 면을 그리지 않는다. 즉, 모델의 귓 면 만을 그리고 전 면은 제외한다.
3. FrontCounterClockwise (BOOL) : 삼각형의 정점 순서를 시계 방향 또는 반시계 방향으로 지정할지 여부를 나타내는 불리언 값. TRUE로 설정하면 반시계 방향을, FALSE로 설정하면 시계 방향을 사용한다.
4. DepthBias, DepthBiasClamp, SlopeScaledDepthBias (FLOAT) : 깊이(Depth) 값에 오프셋을 적용하여 Z-파이팅과 같은 문제를 해결하는 데 사용되는 값.
5. DepthClipEnable (BOOL) : 깊이 클리핑을 사용할지 여부를 나타내는 불리언 값. TRUE로 설정하면 깊이 클리핑이 활성화된다.
6. ScissorEnable (BOOL) : 시저 테스트(Scissor Test)를 사용할지 여부를 나타내는 불리언 값. TRUE로 설정하면 시저 테스트가 활성화된다.
7. MultisampleEnable (BOOL) : 멀티샘플링을 사용할지 여부를 나타내는 불리언 값. TRUE로 설정하면 멀티샘플링이 활성화된다.
8. AntialiasedLineEnable (BOOL) : 선분의 안티 앨리어싱(Antialiased Line)을 사용할지 여부를 나타내는 불리언 값. TRUE로 설정하면 선분이 부드럽게 그려진다.

- D3D11_RASTERIZER_DESC 구조체를 사용하여 래스터라이저의 동작을 정의하고, D3D11Device의 CreateRasterizerState 함수를 호출하여 래스터라이저 상태 개체를 생성할 수 있다. 생성된 래스터라이저 상태 개체를 D3D11DeviceContext의 RSSetState 함수를 사용하여 래스터라이저의 동작을 설정하고 변경할 수 있다.

# DepthStencilState

### ID3D11DepthStencilState

```c++
ID3D11DepthStencilState : public ID3D11DeviceChild
    {
    public:
        virtual void STDMETHODCALLTYPE GetDesc( 
            /* [annotation] */ 
            _Out_  D3D11_DEPTH_STENCIL_DESC *pDesc) = 0;
        
    };
```

### D3D11_DEPTH_STENCIL_DESC

```c++
typedef struct D3D11_DEPTH_STENCIL_DESC
{
	BOOL DepthEnable;
	D3D11_DEPTH_WRITE_MASK DepthWriteMask;
	D3D11_COMPARISON_FUNC DepthFunc;
	BOOL StencilEnable;
	UINT8 StencilReadMask;
	UINT8 StencilWriteMask;
	D3D11_DEPTH_STENCILOP_DESC FrontFace;
	D3D11_DEPTH_STENCILOP_DESC BackFace;
} D3D11_DEPTH_STENCIL_DESC;
```

- Member
1. DepthEnable (BOOL) : 깊이(Depth) 테스트를 사용할지 여부를 나타내는 불리언 값. TRUE로 설정하면 깊이 테스트가 활성화된다.
2. DepthWriteMask (D3D11_DEPTH_WRITE_MASK) : 깊이 값을 깊이 버퍼에 쓸지 결정하는데 사용되는 값. 
	1. D3D11_DEPTH_WRITE_MASK_ZERO : 깊이 값을 쓰지 않는다. 즉, 깊이 테스트만 하고 깊이 버퍼에는 쓰지 않는다.
	2. D3D11_DEPTH_WRITE_MASK_ALL : 깊이 값을 쓴다. 즉, 깊이 테스트를 통과한 픽셀의 깊이 값을 깊이 버퍼에 쓴다.
3. DepthFunc (D3D11_COMPARISON_FUNC) : 깊이 테스트에 사용되는 비교 함수를 설정하는 값. 
	1. D3D11_COMPARISON_NEVER : 테스트를 항상 실패로 간주한다. 즉, 해당 태스트를 통과하지 않는다.
	2. D3D11_COMPARISON_LESS : 테스트를 통과하는 픽셀의 깊이 값이 현재 깊이 값보다 작을 때 테스트를 통과한다.
	3. D3D11_COMPARISON_EQUAL : 테스트를 통과하는 픽셀의 깊이 값이 현재 깊이 값과 같을 때 테스트를 통과한다.
	4. D3D11_COMPARISON_LESS_EQUAL : 테스트를 통과하는 픽셀의 깊이 값이 현재 깊이 값보다 작거나 같을 때 테스트를 통과한다.
	5. D3D11_COMPARISON_GREATER : 테스트를 통과하는 픽셀의 깊이 값이 현재 깊이 값보다 클 때 테스트를 통과한다.
	6. D3D11_COMPARISON_NOT_EQUAL : 테스트를 통과하는 픽셀의 깊이 값이 현재 깊이 값과 같지 않을 때 테스트를 통과한다.
	7. D3D11_COMPARISON_GREATER_EQUAL : 테스트를 통과하는 픽셀의 깊이 값이 현재 깊이 값보다 크거나 같을 때 테스트를 통과한다.
	8. D3D11_COMPARISON_ALWAYS : 테스트를 항상 통과로 간주한다. 즉, 해당 테스트를 항상 통과한다.
	- 예를 들어 D3D11_COMPARISON_LESS 를 깊이 테스트에 사용하면 렌더링 중에 픽셀의 깊이 값이 현재 깊이 값보다 작을 때만 픽셀을 그릴 수 있다. 
4. StencilEnable (BOOL) : 스텐실 테스트와 스텐실 연산을 사용할지 여부를 나타내는 불리언 값. TRUE로 설정하면 스텐실 테스트와 연산이 활성화된다.
5. StencilReadMask (UINT8) : 스텐실 테스트를 수행할 때 읽을 스텐실 값에 적용할 마스크를 설정한다. 이는 8비트 정수 값으로 설정한다.
6. StencilWriteMask (UINT8) : 스텐실 버퍼에 쓸 스텐실 값에 적용할 마스크를 설정한다. 이는 8비트 정수 값으로 설정한다.
	1. D3D11_DEPTH_WRITE_MASK_ZERO : 깊이 값을 쓰지 않는다. 즉, 깊이 테스트만 하고 깊이 버퍼에는 깊이 값을 쓰지 않는다. 이 설정을 사용하면 깊이 테스트를 통과한 픽셀의 깊이 값을 그대로 유지하고, 깊이 버퍼의 값을 변경하지 않는다. 이로 인해 해당 픽셀이 깊이 버퍼의 값을 변경하지 않고 다른 객체들에 가려져서 이후에 그려진 객체들을 가려내는 등의 효과를 얻을 수 있다.
	2. D3D11_DEPTH_WRITE_MASK_ALL : 깊이 값을 쓴다. 즉, 깊이 테스트를 통과한 픽셀의 깊이 값을 깊이 버퍼에 쓴다. 이 설정을 사용하면 깊이 테스트를 통과한 픽셀의 깊이 값을 깊이 버퍼에 기록하여 해당 픽셀이 다른 객체들을 가려내는데 사용될 수 있다.
7. FrontFace, BackFace (D3D11_DEPTH_STENCILOP_DESC) : 각각 전면, 후면 에 대한 스텐실 연산을 설정하는 구조체(D3D11_DEPTH_STENCILOP_DESC)이다. 이 구조체는 스텐실 테스트를 통과 했을 때와 테스트를 통과하지 못했을 때의 동작 등을 지정할 수 있다.

# BlendState

### ID3D11BlendState

```c++
ID3D11BlendState : public ID3D11DeviceChild
    {
    public:
        virtual void STDMETHODCALLTYPE GetDesc( 
            /* [annotation] */ 
            _Out_  D3D11_BLEND_DESC *pDesc) = 0;
        
    };
```

### D3D11_BLEND_DESC

```c++
typedef struct D3D11_BLEND_DESC
{
	BOOL AlphaToCoverageEnable;
	BOOL IndependentBlendEnable;
	D3D11_RENDER_TARGET_BLEND_DESC RenderTarget[ 8 ];
} D3D11_BLEND_DESC;
```

- Member
1. AlphaToCoverageEnable (BOOL) : Alpha To Coverage 기능을 사용할지 여부를 나타내는 불리언 값. Alpha To Coverage는 멀티샘플링 Anti-aliasing(MSAA)를 지원하는 경우 투명한 객체를 더 정교하게 그리는 데 사용된다.
2. BlendEnable (BOOL) : 블렌딩 기능을 사용할지 여부를 나타내는 불리언 값. TRUE로 설정하면 블렌딩이 활성화된다.
3. D3D11_RENDER_TARGET_BLEND_DESC : 개별 렌더 타겟에 대한 블렌딩 상태를 설정하는 데 사용되는 구조체. 각 렌더 타겟에 대해 별도의 블렌딩 동작을 지정할 수 있다. 이 구조체는 다중 렌더 타겟(multiple render target) 렌더링 시에 각 렌더 타겟마다 다른 블렌딩 동작을 정의하는 데 유용하다.

```c++
typedef struct D3D11_RENDER_TARGET_BLEND_DESC
{
	BOOL BlendEnable;
	D3D11_BLEND SrcBlend;
	D3D11_BLEND DestBlend;
	D3D11_BLEND_OP BlendOp;
	D3D11_BLEND SrcBlendAlpha;
	D3D11_BLEND DestBlendAlpha;
	D3D11_BLEND_OP BlendOpAlpha;
	UINT8 RenderTargetWriteMask;
} D3D11_RENDER_TARGET_BLEND_DESC;
```

1. BlendEnable (BOOL) : 렌더 타겟에 대한 블렌딩 기능을 사용할지 여부를 나타내는 불리언 값. TRUE로 설정하면 해당 렌더 타겟의 블렌딩이 활성화된다.
2. SrcBlend, DestBlend (D3D11_BLEND) : 소스 컬러 및 대상 컬러의 블렌딩에 사용되는 블렌딩 팩터를 설정하는 값. D3D11_BLEND 열거형을 통해 다양한 블렌딩 모드를 선택할 수 있다.
3. BlendOp (D3D11_BLEND_OP) : 블렌딩 작업에 사용되는 연산을 설정하는 값. D3D11_BLEND_OP 열거형을 통해 더하기, 빼기, 최대값, 최소값 등 다양한 블렌딩 연산을 선택할 수 있다.
4. SrcBlendAlpha, DestBlendAlpha (D3D11_BLEND) : 소스 알파 컬러 및 대상 알파 컬러의 블렌딩에 사용되는 블렌딩 팩터를 설정하는 값. 알파 값은 투명도를 나타낸다.
5. BlendOpAlpha (D3D11_BLEND_OP) : 알파 블렌딩 작업에 사용되는 연산을 설정하는 값. D3D11_BLEND_OP 열거형을 통해 더하기, 빼기, 최대값, 최소값 등 다양한 블렌딩 알파 연산을 선택할 수 있다.
6. RenderTargetWriteMask (UINT8) : 렌더 타겟에 어떤 채널을 쓸지 설정하는 비트 마스크. 4비트로 표현되며, 각 비트는 각각 빨강, 초록, 파랑, 알파(투명도) 채널에 대응한다.