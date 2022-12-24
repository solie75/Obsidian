렌더링 파이프 라인에는 텍스쳐를 바인딩할 수 있는 stage 가 여럿 있다. 흔한 용도는  텍스쳐를 렌더 대상으로 묶는 것(Direct3D가 텍스쳐 에 렌더링 하는 경우)과 쉐이더 자원으로서 묶는것(쉐이더 안에서 텍스쳐를 추출하는 경우)이다. 이 두가지 용도의 텍스쳐를 생성할 때에는 다음과 같이 텍스쳐를 묶을 두 파이프라인 단계를 지정한 bind flag를 사용한다. 
D3D11_BIND_RENDER_TARGET | D3D11_BIND_SHADER_RESOURCE
사실 texture resource 가 파이프 라인의 stage에 직접적으로 바인딩 되는 것은 아니다. 실제로는 텍스처가 연관된 resource view를 파이프 라인에 바인딩한다. 어떤 용도이든 Direct3D에서 텍스쳐를 사용하려면 텍스쳐의 초기화 시점에서 그 텍스쳐의 resource view 를 생성해야 한다. 이러한 추가 절차는 기본적으로 효율성을 위한 것으로 "이 방식은 실행 시점 모듈(runtime)과 구동기(드라이버)가 유효성 점검과 매핑을 뷰 생성 시점에서 수행 할 수 있기 때문에 결속 시점에서의 형식 점검이 최소화된다."고 sdk는 말한다. 따라서 Texture를 render target과 쉐이더 자워느올 사용하는 예제를 위해서는 두개의 뷰를 생성할 필요가 있다. 하나는 ID3D11RenderTargetView 이고 하나는 ID3D11ShaderResourceView 이다. resource view 들은 본질적으로 두가지의 일을한다. 1. Direct3D에게 자원의 사용 방식(즉, 파이프라인의 어떤 단계에 바인딩 할 것인지)을 알려준다. 2. 생성 시점에서 typeless 인 resource 타입의 구체적인 형식을 결정하는 것이다. 이때 resource가 typeless 인 경우 텍스쳐 원소를 한 파이프라인 단계에서는 부동 소수점 값으로서 사용하고 다른 단계에서는 정수로서 사용하는 것이 가능함을 뜻한다.

어떤 resource에 대해 특정 뷰를 생성할 수 있으려면 애초에 그 resource 를 생성할 때 해당 바인딩 flag를 지정해야 한다. 예를 들어 D3D11_BIND_DEPTH_STENCIL flag(texture가 깊이, 스텐실 버퍼로서 파이프 라인에 묶일 것을 뜻한다.)를 지정하지 않고 생성한 resource에 대해서는 ID3D11DepthStencilView를 생성할 수 없다. 

리소스는 여러 파이프 라인 스테이지에 공유될 수 있도록 범용 메모리 포맷에 저장될 수 있다. 
<span style="color:Yellow ">파이프 라인 스테이지는 view를 사용하여 리소스 데이터를 해석한다.</span> 
리소스 뷰는 특정 context내에서 사용될 수 있도록 리소스 데이터를 캐스팅하는 것과 개념적으로 비슷하다.

view 는 typeless 리소스와 함께 사용될 수 있다. 
<span style="color:yellow">즉, 리소스는 컴파일 시간에 생성될 수 있고 리소스가 파이프 라인에 바인딩 될 때 데이터 타입은 선언될 수 있다. </span>
typeless 리소스로 생성된 view 는 언제나 각 component(구성요소)마다 동일한 수의 비트를 가진다. 데이터가 해석되는 방식은 포맷이 특정(지정)되는 것에 따라 다르다.
그 지정된 포맷은 리소스가 생성될 때 사용되는 typeless format 과 같이 동일한 family의 출신이어야 한다.(<span style="color: green">format, family는 무엇인가.</span>) 예를 들어, R8G8B8A8_TYPELESS 포맷으로 생성된 리소스는 R32_FLOAT 리소스로서 뷰가 될 수 없다. 심지어 그 두 포맷이 메모리상 같은 크기 일지라도 말이다.

view 는 또한 쉐이더의 back depth/ stencil surface 를 읽는 것, single pass에서 동적 cubemap을 만드는 것 그리고 volume의 여러 조각을 동시에 렌더링 하는 것 과 같은 다른 능력(기능)들을 제공한다.