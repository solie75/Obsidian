```c++
typedef struct D3D11_BUFFER_DESC
    {
    UINT ByteWidth;
    D3D11_USAGE Usage;
    UINT BindFlags;
    UINT CPUAccessFlags;
    UINT MiscFlags;
    UINT StructureByteStride;
    } 	D3D11_BUFFER_DESC;
```

## Members

1. UINT ByteWidth 
	버퍼의 크기.

2. D3D11_USAGE Usage 
	버퍼가 쓰이는 방식(버퍼가 CPU 에 접슨하는 방식)
	- D3D11_USAGE_DEFAULT
		GPU 가 buffer 의 resource 를 읽고 사용하고자 할 때 설정한다. CPU 는 해당 용도를 가진 resource 를 ID3D11DeviceContext::Map 을 통해서 읽거나 쓸 수 없다. ( 대신 ID3D11DeviceContext::UpdataSubresource 를 사용하는 것은 가능하다.)
	- D3D11_USAGE_IMMUTABLE
		버퍼를 생성한 후에 변경하지 않을 때 설정한다. 버퍼가 GPU 의 읽기 전용이 됨으로 최적화의 여지가 있다.
		생성 시점에서의 resource 초기화 때를 제외 하면, CPU 는 해당 용도를 설정한 resource 에 data 를 기록하지 못한다. CPU 는 IMMUTABLE rsource 를 읽지 못하여, 매핑과 갱신 메서드에 사용할 수 없다.
	- D3D11_USAGE_DYNAMIC
		응용 프로그램(CPU)이 resource 의 data를 자주 수정하는 경우( 예로들어, 매 프레임마다) 설정한다. 이 용도의 resource 는 GPU 가 읽을 수 있고 CPU는 Map() 메서드를 통해서 자료를 기록할 수 있다.
		※ 새 자료를 CPU 메모리(즉 시스템 RAM) 에서 GPU 메모리 (즉 비디오 RAM)로 전송하기 때문에 성능상의 피해가 생긴다. 따라서 되도록이면 해당 용도는 피한다.
	- D3D11_USAGE_STAGING
		응용 프로그램(CPU)이 resource 의 복사본을 읽어야 한다면 (즉, resource 가 data를 비디오 RAM 에서 시스템 RAM 으로 전송할 수 있어야 한다면) 이 용도를 설정한다.
		GPU 에서 CPU 로의 자료 복사는 느린 연산으로 되도록 피한다.
		자원을 복사하는 메서드는 CopuResource( ) 와 CopySubresourceRegion( )이 있다.

3. UINT BindFlags 
	버퍼의 용도. 버퍼가 파이프라인에 어떻게 바인딩 될 것인지에 대해 확인한다. 버퍼는 텍스쳐와 달리 정해져있는 형체는 없는 메모리 덩어리에 불과하나 그 용도는 명확히 해주어야한다. D3D11_BIND_FLAG.

4. UINT CPUAccessFlags 
	CPU 가 buffer 에 접근하는 방식을 결정하는 flag 들을 지정한다.
	D3D11_CPU_ACCESS_FLAG 또는 CPU access 가 필요하지 않은 경우 (버퍼 생성 이후 CPU 가 버퍼를 읽거나 쓰지 않는 경우)는 0 을 설정.

```
일반적으로 Direct3D 자원을 CPU 에서 읽는 것은 느린 연산이다.
-> GPU 는 파이프 라인을 통해서 자료를 넣는데에는 최적화 되었지만 다시 읽는데는 그렇지 않다.
-> GPU 처리가 일시적으로 멈추는 현상이 일어날 수 있음. (CPU가 data 를 다 읽을 때까지 GPU 가 다음 작업으로 넘어가지 못할 수 있기 때문)
-> CPU가 자료를 다시 비디오 메모리로 전송하는 것도 추가 부담이다.
※ 따라서 자원을 비디오 메모리 안에 유지하고 GPU만이 읽고 쓰게 하는 것이 좋다.
```

5. UINT MiscFlags 
	여러종류의 flag (D3D11_RESOURCE_MISC_FLAG), vertex buffer 의 경우 0.

6. UINT StructureByteStride 
	버퍼가 구조화된 버퍼로 나타내어질 때 그 버퍼 구조 내의 각 요소의 크기 값.
	구조화된 버퍼는 같은 크기의 원소들을 담는 버퍼이다.

```note
텍스쳐의 경우 GPU 상에서 색상을 색칠하고 그곳에 그림을 그리기 때문에 cpu 에서 개입하여 값을 조작할 일이 없었다. 하지만 정점 버퍼의 경우 GPU 강에 정점 버퍼 메모리를 할당하기는 하지만 그 정점의 위치를 옮기고 싶으면 버퍼의 내용을 사용자가 수정해야 한다.

CPUAccessFlags 와 Usage를 조합하면 사용자가 CPU 에서 접근하여 버처를 조작할 수 있게 된다.
```


## Remarks

이 구조체는 버퍼 리소스를 생성하기 위해서 ID3D11Device::CreateBuffer 에 사용된다.

이 구조체에 더하여, 또한 D3D11.h 에 정의 되어 있고 상속된 클래스처럼 행동할 수 있는 파생된 구조체인 CD3D11_BUFFER_DESC 을 buffer description 을 생성한는데 도움을 주기 위해 사용할 수 있다.

만약  bind flag 가 D3D11_BIND_CONSTANT_BUFFER 이면, ByteWidth 값을 16의 배수로 설정해야 하고 D3D11_REQ_CONSTANT_BUFFER_ELEMENT_COUNT보다 작거나 같아야 한다.

