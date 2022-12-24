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
3. UINT BindFlags : 버퍼의 용도. 버퍼는 텍스쳐와 달리 정해져있는 형체는 없는 메모리 덩어리에 불과하나 그 용도는 명확히 해주어야한다.
4. UINT CPUAccessFlags : 