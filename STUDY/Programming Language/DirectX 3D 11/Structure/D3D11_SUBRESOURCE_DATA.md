## Syntax
```c++
typedef struct D3D11_SUBRESOURCE_DATA
    {
    const void *pSysMem;
    UINT SysMemPitch;
    UINT SysMemSlicePitch;
    } 	D3D11_SUBRESOURCE_DATA;
```

## Members

1. const void *pSysMem : 초기화 데이터 를 가리키는 포인터.
2. UINT SysMemPitch : 텍스쳐의 한 라인의 시작부터 다음 라인까지의 distance(in byte). system-memory pitch 는 다른 resource type 에는 의미가 없기 때문에 2D와 3D 텍스쳐 데이터 에만 사용된다. 3D 텍스쳐의 2D slice 의 첫번째 픽셀부터 SysMemSlicePitch 맴버의 해당 텍스쳐에 있는 다음 2D slice 의 첫번째 픽셀까지의 거리를 지정한다.
3. 