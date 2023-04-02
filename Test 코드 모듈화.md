지금까지의 CTest.cpp 의 내용을 각자 모듈화 시키고 그것을 활용하여 Client  프로젝트에서 구현한다.

- 버퍼 생성 부터 실제 출력까지의 코드를 모듈화

1. CResource 내용 추가 
	1. 각 리소스의 고유 이름인 strkey 와 해당 리소스의 주소인 strRelativePath를 맴버 변수로 선언한다.
	2. UpdateResourceData() 를 순수 가상 함수로 선언한다.
	3. 무조건 nullptr 을 반환하는 Clone() 선언 : 리소스는 Clone() 을 구현하지 않는다는 의미.
	4.  iRefCount 를 감소하고 해당 리소스를 삭제하는 함수인 ReleaseResource() 생성

3. 07.Resource 폴더에 4.Shader 폴더 추가 -> 해당 폴더에 CShader class 생성
	1. CShader class는 $ComPtr<ID3DBlob>$ 형의 변수 m_ErrBlob 을 맴버로 갖는다.
	2. 생성자에 RESOURCE_TYPE 을 인자로 주어 생성 시에 부모 클라스인 CResource 의 생성자에 인자로 준다.

4. 4. Shader 폴더에 GraphicsShader 폴더 추가 ->해당 폴더에 CGraphicsShader class 생성
	1. rendering pipeline 의 각 stage 에 대한 shader 인터페이스 변수, 컴파일된 shader 를 담을 blob 형 변수, inputlayout에 대한 변수, topology 에 대한 변수 들을 맴버로 갖는다.
	2. 기존의 CTest.cpp 의 TestInit () 에서 생성하던 생성하는 함수 생성
		1. 생성하려는 shader 의 기본 코드가 작성되어 있는 .fx 파일의 이름과 경로를 인자로 받는다.
		2. shader 파일의 경로를 지역변수 선언하여 저장한다.
		3. 해당 shader 파일을 컴파일하여 그에 맞는 blob 형 맴버 변수에 저장한다.
		4. 컴파일된 객체를 인자로 하는 CreateVertexShader() 와 CreatePixelShader() 를 호출하여 Vertex Shader 와 Pixel shader 를 생성한다.
		5. D3D11_INPUT_ELEMENT_DESC 구조체를 자료형으로 하는 크기 2의 배열을 생성 (이때 각 요소는 'POSITION'과 'COLOR'). -> 해당 배열을 사용하여 InputLayout 을 생성한다.
	3. Topology 맴버 변수에 값을 저장하는 SetTopology() 함수 생성
	4. CResource class에서 순수가상함수 로 UpdateResourceData() 함수를 재정의(Override) -> 다음의 함수들을 호출하여 Create~Shader() 에서 생성된 변수들을 파이프 라인에 바인딩한다.
		1. IASetInputLayout()
		2. IASetPrimitiveTopology()
		3. VSSetShader()
		4. PSSetShader()

4. CMesh 클래스 생성
	1. 하나의 Mesh 를 생성하는데 필요한 다음의 변수들을 맴버로 선언한다.
		1. vertex buffer 와 indexbuffer 의 인터페이스에 대한 변수
		2. 각 종류의 버퍼의 개수를 카운트하는 변수
		3. 각 종류에 따라 모든 버퍼를 요소로 하는 두 배열(~Sys) (이 배열들은 각 버퍼에 필요한 subresource 구조체의 맴버 pSysMem 에 저장된다. 이는 각 버퍼의 초기 값을 의미한다.)
	2. CreateMesh() 함수로 Mesh 를 생성한다.
		매개변수인 버퍼의 초기화 데이터와 그 버퍼를 구성하는 요소(각 정점 또는 인덱스) 를 사용하여 vertex buffer 와 index buffer 를 생성한다.
	1. RenderMesh() 함수로 버퍼를 출력한다.
		1. UpdateResourceData();
			IASetVertexBuffers() 와 IASetIndexBuffer() 를 호출하여 새로운 버퍼를 IA stage 에 바인딩한다.
		2. DrawIndexed() 호출로 인덱스 버퍼 에 따라 정점을 출력한다.

5. CMeshRender 클래스 수정
	1. CMesh* 형과 CGraphicsShader* 형의 변수를 선언
	2. 이 맴버 변수에 대한 Get, Set 함수 추가
	3. BeginRenderingMesh() 함수에서 COMPONENT_TYPE::MESHRENDERING 을 가지는 오브젝트가 Mesh 와 CGraphicsShader 를 가지로 있는지 여부에 따라 둘 다 있는 경우 CMesh::RenderMesh() 를 호출하여 렌더링을 시작한다.

6. 기존의 CTest 클래스의 모듈화 한다.
	1. 선언 : 기존의 선언을 삭제하고 CMesh 클래스와 CGraphicsShader 사용
		1. vertex buffer (g_VB)와 index buffer (g_IB) 선언 삭제.
		2. 컴파일된 쉐이더 객체 저장용 blob 형 변수(g_VSBlob, g_PSBlob, g_ErrBlob) 선언 삭제.
		3. vertex shader (g_VS) 와 index shader (g_PS) 선언 삭제.
		4. InputLayout (g_Layout) 선언 삭제.
		5. 정점(Vtx) 를 자료형으로 하는 배열 (g_arrVtx) 와 인덱스를 나타내는 배열 (g_arrIdx) 선언 삭제.
		6. CMesh.h 를 include 하고 CMesh* 형 변수(g_pRectMesh) 와 CGraphicsShader* 형 변수(g_pShader) 선언
	
	2. TestInit()
		- g_arrVtx 와 g_arrIdx 에 대입하는 코드 삭제
			1. TestInit() 내에 Vtx 를 자료형으로 하는 벡터 (vecVtx)와 UINT 를 자료형으로 하는 벡터 (vecIdx)를 선언하고. Vtx 형 변수를 선언한다.
			2. 이 Vtx 형 변수의 position 과 color 에 특정 값을 대입하고 그 Vtx 형 변수를  vecVtx 에 push_back 한다.
			3. 이와 마찬가지로 vecIdx 에도 값들을 push_back 한다.
		![[Pasted image 20230327031309.png]]
		1. 기존의 버퍼 생성 코드 삭제
			- g_pRectMesh 에 new CMesh 로 Mesh 생성 및  CreateMesh() 호출로 vertex buffer 를 생성한다. 생성된 vertex buffer 는 m_VB 에 담긴다.
		2. 기존의 shader 코드 컴파일 및 shader 생성 코드 삭제
			- g_pShader 에 new CGraphicShader 로 Shader 생성 및 CraeteVertexShader() 과 CreatePixelshader() 호출로 shader  코드를 컴파일하고 shader 를 생성한다.
		1. 게임 오브젝트 생성 
			- 선언된 object 변수에 new CGameObject 로 메모리를 할당하고 AddComponent 로 해당 오브젝트에서 사용될 컴포넌트들을 추가한다.
			- g_Obj 의 MeshRender Component 에 SetMesh(), SetShader() 함수를 호출하여 위에서 설정한 Mesh 와 Shader 를 CMeshRender class의 맴버 변수에 저장한다. (이는 하나의 게임 오브젝트가 자신만의 온전한 Mesh 와 Shader 를 갖게 되었다는 것을 말한다.)

	3. TestRender() 에서
		- 기존의 코드를 삭제하고 p_Obj 에 대해서 GameObjectRender() 함수를 호출한다. 
		- CGameObject::GameObjectRender()->CMeshRender::BeginRenderingMesh()->CMesh::MeshRender() 로 이어진다.
		- CMesh::MeshRender() 에서 호출되는 CGraphicsShader::UpdateResourceData() 와 CMesh::UpdateResourceData() 로 buffer 와 layout, primitive topology 를 IA Stage 에 바인딩하고, VSSetShader 로 정점 쉐이더를 device에 바인딩하며 PSSetShader 로 픽셀 쉐이더를 device 에 바인딩 한다.

	 4. TestTick()
		 - 기존의 코드를 삭제 -> object 로서 출력될 resource에 대한 설계와 object 의 실시간 변경점만을 관리하는 Script Library 설계
		 - 전체적인 흐름은 다음과 같다.
			 - TestTick() 에서 변경될 resource 가져오기 -> 오브젝트의 Tick() 호출 -> 모든 컴포넌트의 Tick() 호출 -> 이에 컴포넌트를 부모로 가지는 모든 자식 컴포넌트의 Tick() 이 차례로 호출. -> 변경점을 가져온 resource 에 적용시킨다.
		1. 정적 라이브러리 'Script' 프로젝트 생성 [[정적 및 동적 라이브러리를 프로젝트에 적용#^620f5d | 정적 라이브러리 생성 과정]]
			1. 솔루션에서 새 프로젝트 추가 (Windows 데스크톱 마법사)
			2. 프로젝트 이름 : Script, 애플리케이션 종류 : 정적 라이브러리, 미리 컴파일된 헤더 체크
			3.  솔루션의 .sln 파일을 메모장으로 열어 Script 프로젝트 경로 설정
			4.  ( External 폴더 -> Include 폴더 와 Library 폴더에 Script 폴더 추가 )
			5.  Default 폴더와 Script 폴더를 생성 -> Default 폴더에 모든 파일을 넣는다.
			6.  정적 라이브러리 Script 의 속성 설정
				- 구성 : 모든 구성, 플랫폼 : 활성 , 일반, 출력 디렉터리 설정 + 구성 형식 : 정적 라이브러리
				- 구성 : 디버깅 , 대상이름에 언더바 d 추가
				- 구성 : 모든 구성, 빌드 전 이벤트 : 명령줄에 다음 스크립트 입력
				![[Pasted image 20230329175654.png]]
				- (다음은 Script Library에서 CEngine 의 헤더를 include 할 수 있게 만드는 설정이다.) 구성 : 모든 구성, VC ++ 디렉터리, 일반, 포함 디렉터리 : $(SolutionDir)External\Include\ , 외부 include 디렉터리 : $(ExternalIncludePath) , 라이브러리 디렉터리 : $(SolutionDir)External\Library\ ; $(LibraryPath)
				- Script Library/ Default 의 pch.h 에 # include <Engine\global.h> 추가
			7. 솔루션 폴더에 ScriptCopy.bat 파일 추가 (이는 Script 프로젝트의 헤더파일들을 External/Include/Script 폴더에 복사한다.)
			8. exclude_list 파일에 적힌 targetver.h, pch.h, framework.h 의 경우 External/Include 폴더에 복사되지 않기 때문에 Script Library 에서 접근할 대상은 위의 헤드파일에서 다른 접근 가틍한 파일로 옮겨야 한다. 때문에 pch.h 파일에 있는 코드 중 Script 에서 사용될 코드들은 global.h 에 옮기고 pch.h 은 global.h 를 include 한다.
		2. Script Library에 기존의 TestTick() 의 코드를 모듈화
			1. Engine Library의 Source/06.Component 에 Script 폴더 생성 -> 해당 폴더에 CScript class 생성. -> 이를 상속 받는 자식 클래스들은 Script Library에서 관리.
			2. CScript class
				1. ComponentFinaltick() 을 final 한다.
				2. 생성자에서 부모 클라스( CComponent )의 생성자에 COMPONENT_TYPE::SCRIPT 를 인자로 준다.
				3. CLONE(CScript) 추가
				4. virtual void ComponentTick() {} 추가
			3. Script Library / Script 폴더에 CPlayerSCript class 생성
				1. <Engine/CScript.h> 를 include한다. (이는 VC++ 디렉터리 부분에서의 설정으로 가능하다.)
				2. CLONE(CPlayerScript) 를 추가
				3. ComponentTick() 을 override 한다. -> CPlayerScript::ComponentTick() 은 플레이어가 되는 객체의 방향키에 따른 위치의 변화 를 구현한다.
		3. 컴포넌트 중 script type은 하나의 object tick 내에서 여러 개의 script 를 호출해야하는 경우가 존재한다. 따라서 m_arrCom 과 별개로 Script를 vector 로 저장한다.
			- CGameObject class에서
			1. vector<CScript*>m_vecScript; 를 선언한다.
			2. AddComponent() 변경
				1. 인자로 받은 Component의 m_pOwner에 CGameObject 를 대입 (this)
				2. 인자로 받은 Component가 Script 인 경우 m_vecScript 에 저장하고 아닌 경우 m_arrCom 에 저장
			3. GameObjectTick() 에서 오브젝트가 가지고 있는 모든 component 와 Script 의 tick 을 호출한다.
		4. CBuffer 선언 및 생성
			1. Engine Library/ Source/ 02.Device 에 ConstBuffer 폴더 생성 -> 해당 폴더에 CConstBuffer class 생성 하고 다음의 변수들 선언
				1. ID3D11Buffer 형 constant buffer 변수 (m_CB)
				2. D3D11_BUFFER_DESC 구조체 변수 (m_Desc)
				3. 생성할 버퍼의 요소 한개의 크기 (m_iElementSize)과 요소들의 갯수 (m_iElementCount)
				4. 쉐이더에 constant buffer 를 바인딩하는 함수 ~SetConstantBuffers() 에서 첫 번째 인자로 받는 슬롯 넘버 로 사용될 UINT 형 변수 (m_iRegisterNum) <span style="color:green">왜 slot num 이 아니라 registernum 이지..?</span>
			2. 생성자에서 _ iRegisterNum 을 인자로 받는다.
			3. CreateConstBuffer(UINT _ iElementSize, UINT _ iElementCount) 함수 추가
				1. 인자로 받은 값을 그에 맞는 맴버 변수에 저장한다
				2. 인자로 받은 두 값을 곱하여 만들 버퍼의 크기를 구하고 m_Desc 를 Constant Buffer 용 으로 설정한다. 
				3. CreateBuffer() 함수를 호출하여 m_CB 에 생성한 버퍼를 저장한다.
			- CDevice.h 에서 CConstBuffer 임의 선언
				1. CDevcie class 에서 CConstBuffer* 형 맴버변수 ( m_ConstBuffer ) 선언
				2. DeviceInit() 함수에서 RSSetViewports() 이후 삼수 버퍼를 생성한다.
				3. m_ConstBuffer 에 new CConstBuffer(0) 로 CConstBuffer 생성
				4. m_ConstBuffer 에 대해 CreateConstBuffer(sizeof(Vec4),1) 호출 -> 이 상수 버퍼는 네개의 정점을 갖는 하나의 사각형을 갖는다.
		5. 오브젝트의 위를 변환하기 위한 Component 인 CTransform 을 수정
			1. 인자가 Vec3 인 경우와 float 3개 인 경우 각각에 대한 Set 함수와 맴버 변수에 대한 Get 함수 생성
			2. UpdateTransformData() 함수 수정
				1. D3D11
		6. Client 프로젝트에 정적 라이브러리 Script 의 적용
			1. Client.cpp 의 기존의 선언 부분에 대해 미리 컴파일 된 헤더 설정을 추가하여 분류하고 Script Library 선언을 추가한다. 이때 pch.h 는 다음과 같다.
			![[Pasted image 20230328220218.png]]


그외. CTransform 클래스에 UpdateTransformData() 를 추가한다. 이는 위치 값을 상수 버퍼에 저장을 수행한다.

## const buffer 순서

CConstBuffer 클라스 생성
	다음 선언
	ComPtr<ID3D11Buffer> m_CB;
	D3D11_BUFFER_DESC m_Desc;
	const UINT m_iRegisterNum;
	UINT m_iElementSize;
	UINT m_iElementCount;

	다음 함수 추가
	CreateConstBuffer -> CreateBuffer (m_CB 에 생성한 상수 버퍼 담김)
	SetConstBufferData -> Map (데이터를 m_CB 에 복사하여 저장)
	UpdateConstBufferData -> VSSetConstBuffers() 로 상수 버퍼 를 정점 쉐이더에 바인딩

CDevice 에서 
	CConstBuffer* 로 변수 (m_ConstBuffer) 선언
	DeviceInit() 에서 new 로 할당하고 , CConstBuffer::CreateConstBuffer() 호출 -> 이는 CreateBuffer로 이어진다. Device 에서 CConstBuffer 형 변수를 선언하지 않으면 그 변수 내에 존재하는 m_CB 또한 존재하지 않는다. 

CTransform 에서 
	ComponentFinaltick() 에서 UpdateTransformData() 호출
	UpdateTransformData () 에서
	-> CDevice의 m_ConstBuffer 에 대하여
	-> CConstBuffer::SetConstBufferData() 호출
		-> D3D11_MAPPED_SUBRESOURCE 선언
		-> Map 호출
	-> CConstBuffer::UpdateConstBufferData() 호출
		-> VSSetConstantBuffers() 호출 -> 상수 버퍼를 정점 쉐이더에 바인딩한다.











## Script Tick 에서의 주인되는 오브젝트의 위치에 대한 접근

CComponent 에서
	GetOwner() 생성

## resource 관리

resource 생성 및 해제 관리
	 Ptr class 생성 (<span style="color: green ">이름이 마음에 들지 않음 리소스에 대해서만 관리할 거면 적어도 ResPtr 이라고 하던가 아니면 Release 와 Addref 함수를 확정짓지 말던가</span>)
	 template<typename T>
		 T* 형 맴버 변수 선언
		 GetResource()
		 operator ->
		 operator =
		 생성자 및 소멸자
	operator = 와 생성자 및 소멸자에서 CResource class 의 private 맴버 함수에 접근해야 하기에 CResource class 에서 Ptr 에 대해 friend class 선언한다.

ResourceMgr class 생성
	Engine Library/ Source / 03.Manager 에 05. ResourceMgr 폴더 생성 -> 해당 폴더에 CResourceMgr class 생성
	wstring과 Ptr<CResource> 를 하나요소로 하는 map 형 맴버 변수 (m_arrRes) 선언
	CresourceMgrInit() 함수 생성
	CreateDefaultMesh() 함수 생성 -> 여기에서 만든 기본 사각형 mesh 가 m_arrRes d에 저장된다.
	CreateDefaultGraphicsShader() 함수 생성
	CResourceMgrInit() 함수에 
		CreateDefaultMesh 와CreateDefaultGraphicsShader 호출
	global.h 에 <typeinfo> 를 include 한다.
	FindResource() 함수 생성


## TestInit()
object 생성
new CGameObject;
AddComponent(CMeshRender, CTransform)
- 리소스 관리 ()
	- 
	- 
- ~~ (ptr.h 생성)
SetMesh()
SetShader()


## constBuffer 부분
TestTick() 에서
	-> GameObjectTick()
		-> ComponentTick()
		-> 모든 컴포넌트의 ComponentTick() 호출
		-> 모든 스크립트의 ComponentTick() 호출
			->CPlayerScript::ComponentTick() 호출
				-> 현재 컴포넌트의 주인되는 오브젝트의 위치를 가져와야 한다. 이를 위해서 CComponent 를 수정해야 한다.
	-> GameObjectFinalTick()
		-> ComponentFinalTick()
		-> CTransform::ComponentFinalTick();
		-> UpdateTransformData ()
			->SetConstBufferData()
				m_CB 에 대한 Map()
			->UpdateConstBufferData()
				-> VSSetConstantBuffers()

CComponent class 수정
	1. include 관계정리
		CGameObject.h 에서
			# include "CComponent" 
			를
			class CComponent; 로 변경
		
		이와 반대로
		
		CComponent.h 에서
			class CGameObject;
			를
			# include "CGameObject.h"
	2. GetOwner() 생성

CPlayerScript class에서 해당 오브젝트의 현재 위치를 알기 위한 CTransform::GetRelativePos() 를 호출하기 위해서는 CGameObject 와 CTransform 에 접근할 수 있어야 한다.
	CScript 에 CGameObject.h 와 CTransform.h 를 include 한다.

# Rendering 과정

TestRender()

GameobjectRender()

BeginRenderingMesh()

m_pShader ->UpdateResourceData()
	IASetInputLayout()
	IASetPrimitiveTopology()
	VSSetshader()
	PSSetShader()
m_pMesh->RenderMesh()
	UpdateResourceData()
		IASetVertexBuffers()
		IASetIndexBuffer()
	DrawIndexed()

