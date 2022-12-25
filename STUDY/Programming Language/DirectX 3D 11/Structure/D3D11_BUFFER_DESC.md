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

1. UINT ByteWidth : 버퍼의 크기.
2. D3D11_USAGE Usage : CPU 에 어떻게 접근할 것인가에 대한 방법.
3. UINT BindFlags : 버퍼의 용도. 버퍼가 파이프라인에 어떻게 바인딩 될 것인지에 대해 확인한다. 버퍼는 텍스쳐와 달리 정해져있는 형체는 없는 메모리 덩어리에 불과하나 그 용도는 명확히 해주어야한다. D3D11_BIND_FLAG.
4. UINT CPUAccessFlags : D3D11_CPU_ACCESS_FLAG 또는 CPU access 가 필요하지 않은 경우는 0.
5. UINT MiscFlags : 여러종류의 flag (D3D11_RESOURCE_MISC_FLAG), 사용하지 않으면 0.
6. UINT StructureByteStride : 버퍼가 구조화된 버퍼로 나타내어질 때 그 버퍼 구조 내의 각 요소의 크기 값.

```note
텍스쳐의 경우 GPU 상에서 색상을 색칠하고 그곳에 그림을 그리기 때문에 cpu 에서 개입하여 값을 조작할 일이 없었다. 하지만 정점 버퍼의 경우 GPU 강에 정점 버퍼 메모리를 할당하기는 하지만 그 정점의 위치를 옮기고 싶으면 버퍼의 내용을 사용자가 수정해야 한다.

CPUAccessFlags 와 Usage를 조합하면 사용자가 CPU 에서 접근하여 버처를 조작할 수 있게 된다.
```


## Remarks

이 구조체는 버퍼 리소스를 생성하기 위해서 ID3D11Device::CreateBuffer 에 사용된다.

이 구조체에 더하여, 또한 D3D11.h 에 정의 되어 있고 상속된 클래스처럼 행동할 수 있는 파생된 구조체인 CD3D11_BUFFER_DESC 을 buffer description 을 생성한는데 도움을 주기 위해 사용할 수 있다.

만약  bind flag 가 D3D11_BIND_CONSTANT_BUFFER 이면, ByteWidth 값을 16의 배수로 설정해야 하고 D3D11_REQ_CONSTANT_BUFFER_ELEMENT_COUNT보다 작거나 같아야 한다.

