

Direct3D 11 객체 모델은 리소스 생성과 랜더링 기능을 디바이스나 한개 이상의 context로 분리한다. 이 분리는 멀티 쓰레딩을 위해 디자인 되었다.

Device
: 한 디바이스는 리소스를 생성하거나 display adapter의 capability 열거하는 데에 사용된다. Directx 11 에서 한 디바이스는 하나의 ID3D11Device 로 표현된다.
각 어플리케이션은 반드시 적어도 한개 의 디바이스를 갖고 있어야 하며, 대부분의 어플리게이션은 오직 하나의 디바이스를 생성한다. D3D11CreateDevice 를 호출함으로서  하나의 하드웨어 드라이버에 당신의 기계에 설치할 하나의 디바이스를 생성해라. 각 디바이스는 기능적 요구에 따라서 한개 혹은 그 이상의 device context 를 사용할 수 있다. 

Device Context

하나의 device context 는 디바이스가 사용되는 장소의 설정이나 환경을 포함한다. 더 명확히 말하면, device context 는 pipeline state 설정이다. 디바이스가 소유한 리소스를 사용하겠다는 rendering commands를 생성하는데 사용된다. 