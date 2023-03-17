subresource data에 대한 접근을 제공

# Syntax

```c++
typedef struct D3D11_MAPPED_SUBRESOURCE {
  void *pData;
  UINT RowPitch;
  UINT DepthPitch;
} D3D11_MAPPED_SUBRESOURCE;
```

# Members

1. pData
data 를 가리키는 포인터. [[Map( )|ID3D11DeviceContext::Map]] 이  포인터를 제공할 때, runtime 은 포인터가 다음의 feature level 에 따라 특정한 정렬을 갖게 한다.
- D3D_FEATURE_LEVEL_10_0 이상 의 경우 포인터는 16바이트로 정렬된다.
- D3D_FEATURE_LEVEL_10_0 미만의 경우 포인터는 4바이트로 정렬된다.

2. RowPitch
row pitch or width, or physical size (in byte) of the data

3. DepthPitch
The depth pitch, or with, or physical size (in bytes) of the data.


# Remarks

이 구조체는  [[Map( )|ID3D11DeviceContext::Map]] 의 호출에 사용된다.

이러한 맴버의 값들은 볼 수 있는 데이터의 양을 알려준다.

- pData : row 0 과 depth slice 0 을 가리킨다.
- RowPitch : 행에서 행으로 이동하기 위해서 runtime 이 pData 에 추가하는 값을 포함하며 , 여기에서 각 행에는 여러 픽셀이 포함된다.
- DepthPitch : depth slice 에서 depth slice 로 이동하기 위해서 runtime 이 pData 에 추가하는 값을 포함하며 여기에서 각 depth slice 는 여러 행을 포함한다.

RowPitch와 DepthPitch 가 resource type 에 대해 적절하지 않은 경우, runtime 은 그 값들을 0으로 설정할 것이다. 따라서, row 및 depth  를 반복하는 것 외에는 이러한 값들을 사용하지 말아야 한다.

예시
- Buffer 와 Texture1D 의 경우, runtime 는 0이 아닌 값을 RowPitch 와 DetphPitch 에 할당한다. 예를 들어, buffer 가 8byte 를 포함한 경우, runtime 은 8 이상의 값을 RowPitch와 DepthPitch 에 할당한다.
- Texture2D 의 경우, runtime 은 해당 field 가 쓰이지 않는다고 가정하고 여전히 0 이외의 값을 DepthPitch 에 할당한다.