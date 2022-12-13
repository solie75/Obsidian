DXGI (Microsoft Directx Graphics Infrastructure) 는 그래픽 어댑터 열거, 디스플레이 모드 열거, 버퍼 형식 선택, 프로세스 간(예 : 응용 프로그램과 데스크탑 창 관리자(DWM)간 )자원 공유, 렌더링 된 프레임(장면)을 창 또는 모니터에 나타내는 작업을 처리한다.

대부분 그래픽 프로그래밍이 Direct3D 에서 수행되지만, DXGI 를 사용하여 최종 구성(<span style="color:green">여기에서 말하는 최종 구성이란...?</span>)(eventual composition)및 디스플레이를 위해 창, 모니터 또는 기타 그래픽 구성 요소에 프레임(장면)을 나타낼 수 있다. 또한 DXGI 를 사용하여 모니터에 나타나는 콘텐츠를 읽을 수 있다.

DXGI는 Direct3D와 함쎄 쓰이는 API 로 DirectX 크래픽 런타임에 독립적인 저수준(Low-Level)의 작업을 관리한다. 또한 DirectX 그래픽을 위한 기본적이고 공통적인 프레임워크를 제공한다. 

![[Pasted image 20221212211004.png]]

예를 들어 2차원 랜더링 api 에도 3차원 렌더링 api 처럼 스왑 체인과 페이지 전환이 필요하다. 이 때문에, 스왑체인을 대표하는 인터페이스인 IDXGISwapChain은 실제로 DXGI API 의 일부이다.

다음 코드는 시스템에 있는 모든 어댑터를 열거하는 방법이다.
```c++
// 어뎁터에 접근할 index 변수
UINT i = 0;

IDXGIDevice* pDXGIDevice = nullptr;
// Adapter 를 담아둘 포인터 변수
IDXGIAdapter* padapte = nullptr;
IDXGIFACTORY* pFactory = nullptr;
// Adapter들을 저장할 vector 컨테이너
std::vector<IDXGIAdapter*> adapterList;

// IDSGIFactory 객체를 통해 어뎁터들을 열거한다. i를 index로 활용
// 만약 index가 시스템에 존재하는 어뎁터의 갯수와 같거나 더 크다면 while문을 종료한다.
while()
```