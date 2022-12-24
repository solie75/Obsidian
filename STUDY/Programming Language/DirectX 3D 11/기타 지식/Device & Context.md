

Direct3D 11 객체 모델은 리소스 생성과 랜더링 기능을 디바이스나 한개 이상의 context로 분리한다. 이 분리는 멀티 쓰레딩을 위해 디자인 되었다.

## Device

: 한 디바이스는 리소스를 생성하거나 display adapter의 capability 열거하는 데에 사용된다. Directx 11 에서 한 디바이스는 하나의 ID3D11Device 로 표현된다.
각 어플리케이션은 반드시 적어도 한개 의 디바이스를 갖고 있어야 하며, 대부분의 어플리게이션은 오직 하나의 디바이스를 생성한다. D3D11CreateDevice 를 호출함으로서  하나의 하드웨어 드라이버에 당신의 기계에 설치할 하나의 디바이스를 생성해라. 각 디바이스는 기능적 요구에 따라서 한개 혹은 그 이상의 device context 를 사용할 수 있다. 

## Device Context

하나의 device context 는 디바이스가 사용되는 장소의 설정이나 환경을 포함한다. 더 명확히 말하면, device context 는 파이프 라인 state를 설정하고 디바이스 소유의 리소스를 사용하겠다는 렌더링 명령어를 만들어 낸다.

### - Immediate Context

Immediate context는 직접적으로 드라이버(장치)에 랜더 된다. 각 디바이스는 GPU에서 데이터를 검색할 수 있는 단 하나뿐인 immediate context 를 갖는다. 하나의 immediate context 는 command list 를 즉각적으로 렌더하는데 사용될 수 있다. 

### - Deffered Context

deferred context 는 GPU 명령어를 command list 에 저장한다. deferred context는 주로 멀티 쓰레딩에 사용되고 싱글 쓰레드 응용 프로그램에는 필요하지 않다. deffered context는 일반적으로 메인 렌더링 쓰레드 대신에 worker 쓰레드 에 사용된다. deffered context 를 생성할 때 immediate context 로부터 어떠한 값도 상속 받지 않는다.

- immediate, deffered 두 context 모두 한 번에 하나의 쓰레드에서만 사용되는 한 모든 쓰레드에서 사용 할 수 있다.

## Threading Considerations

모든 ID3D11Device 인터페이스 메서드는 자유 쓰레드 이다.  따라서 여러 스레드가 통시에 함수를 호출하는 것이 안전하다. 
- 모든 ID3D11DeviceChild 파생 인터페이스 는 자유 쓰레드 이다. 
- Direct3D11 은 리소스 생성 및 렌더링을 두 개의 인터페이스로 분할 한다. ID3D11Device 는 작업 순서를 강력하게 정의 하므로 Map, Unmap, Begin, End  및 GetData 는 ID3D11DeviceContext 에서 구현된다.  (ID3D11Resource 및 ID3D11Asynchronous 인터페이스도 자유 스레드 작업을 위한 메서드를 구현한다.)
- 