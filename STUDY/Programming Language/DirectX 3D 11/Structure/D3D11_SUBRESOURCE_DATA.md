subresource를 초기화 하기 위해 데이터를 지정한다.

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
3. UINT SysMemSlicePitch : 하나의 depth level의 시작부터  그 다음 까지의 Distance. System-memory-slice pitch는 다른 resource type 에대해서는 의미를 갖지 않기 때문에 오직 3D 텍스쳐 데이터 에 대해서만 사용된다. 

## Remarks

이 구조체는 CreateBuffer, CreateTexture1D, CreateTexture2D, CreateTexture3D 등 버퍼나 텍스쳐를 생성하는 호출에 사용된다. 만약 system-memory pitch 혹은 system-memory-slice pitch 가 필요하지 않는 리소스를 생성했다면,  해당 맴버를 사용하여 리소스 생성 문제를 디버깅할 때 도움이 될 수 있는 크기 정보를 전달 할 수 있다.

subresource 는 single mipmap-level surface 이다. <span style="color: yellow">subresources 배열은 리소스를 생성하기 위해 이전 메서드 중 하나에 전달된다. </span> 하나의 subresource 는 1D, 2D, 3D 가 될 수 있다. 어떻게 D3D11_SUBRESOURCE_DATA의 맴버를 설정하는지는 subresource가 1D, 2D, 3D중 뭐인지에 달려있다.

x, y, 그리고 d 값은 0-based 인덱스들이고 BytePerPixel 은 pixel format에 따른다. 밉맵핑된 3D 표면(mipmapped 3D surfaces) 의 경우, 각 level 안의 depth slices 의 수는 그 전 레벨 에서의 수의 절반이며 2로 나눈 결과 정수가 아닌 경우 내림된다.


## Note

An application must not rely on **SysMemPitch** being exactly equal to the number of texels in a line times the size of a texel. In some cases, **SysMemPitch** will include padding to skip past additional data in a line. This could be padding for alignment or the texture could be a subsection of a larger texture. For example, the **D3D11_SUBRESOURCE_DATA** structure could represent a 32 by 32 subsection of a 128 by 128 texture. The value for **SysMemSlicePitch** will reflect any padding included in **SysMemPitch**.