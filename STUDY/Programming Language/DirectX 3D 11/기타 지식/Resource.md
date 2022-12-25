## Define

- <span style="color: yellow">리소스는 파이프 라안에 데이터를 제공하고 scene에 무엇이 렌더 될 지 정의</span>한다. 
- 리소스는 게임 미디어에서 로드 되거나 혹은 런타임중에 동적으로 생성될 수 있다. 
- 일반적으로 <span style="color: yellow">리소스는 텍스쳐 데이터, 버텍스 데이터 그리고 쉐이더 데이터를 포함한다. </span>
- 대부분의 Direct3D 응용 프로그램은 그 수명동안 광범위하게 리소스를 생성하거나 파괴한다. 

Scene이 빌딩이라면 리소스는 하나의 블록이다. 
- 리소스는 Direct3D가 인터프리트 와 Scene의 렌더를 위해 사용하는 데이터의 대부분을 포함한다. 
- 리소스는 <span style="color: yellow">Direct3D 파이프 라인으로 인해 접근이 가능한 메모리 안의 영역</span>이다.
- 리소스는 다음의 데이터 유형을 갖는다.  : Geometry, 텍스쳐, 쉐이더 데이터.

## Feature

1. 리소스는 strongly typed 나 type less으로 리소스를 생성할 수 있다. 
2. 둘 중 어느 리소스로든지 읽기 또는 작성 권한을 갖도록 조정할 수 있다. 
3. 리소스가 CPU에만 연결되거나 GPU 에만 접근할 수 있게 할 수 있으며, 둘 다 접근하는 것 또한 가능하다. 
4. 각 파이프 라인 단계 마다 128개의 리소스를 활성할 수 있다.
5. Direct3D는 바운딩 영역 외에서 접근된 리소스에 대해서 0을 반환한다. (반환을 보증한다.)
## Lifecycle

1. **ID3D11Device** 인터페이스로 생성
2. context 와 **ID3D11DeviceContext** 인터페이스의 메서드의 설정 중 하나 를 사용하여 파이프라인에 리소스를 바인딩한다.
3. 리소스 인터페이스의 **Release** 메서드를 통해서 리소스의 할당을 해제한다.

## Typed

[[Strong and Weak Type]]

## Resource View

[[Resource View]]

## Resource Type

1. Read/Write Buffers and Textures
2. Structured Buffer : structed buffer (구조화된 버퍼)는 동일한 크기의 요소를 포함한 버퍼이다. 이때 하나의 요소(구조체)는 하나 이상의 여러 맴버를 가질 수 있다.
4. Byte Address Buffer
5. Unordered Access Buffer or Texture
6. Append and Consume Buffer