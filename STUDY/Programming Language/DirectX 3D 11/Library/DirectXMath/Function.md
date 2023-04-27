# XMMatrixScaling()

^754474

3차원 벡터 값을 인자로 받아 해당 벡터의 각 축(x, y, z) 에 해당하는 스케일 값을 이용하여 <span style="color: yellow">스케일링 변환 행렬을 생성</span>한다.

```c++
XMMATRIX XM_CALLCONV XMMatrixScaling(
    float ScaleX,
    float ScaleY,
    float ScaleZ
);
```

ScaleX, ScaleY, ScaleZ 는 각 축에 적용할 스케일 값이다. 

- 예시
ScaleX 가 2.0 , ScaleY 가 3.0, ScaleZ 가 4.0 인 경우 생성된 스케일링 변환행렬은 다음과 같다.
```
2 0 0 0
0 3 0 0
0 0 4 0
0 0 0 1
```
이 행렬은 오른손 좌표계에서 x 축을 2배, y 축을 3배, z 축을 4배로 스케일링 하는 변환을 나타낸다. 

# XMMatrixIdentity()

^63eb56

항등 행렬을 생성하여 반환한다.

```c++
XMMATRIX XM_CALLCONV XMMatrixIdentity() noexcept;
```

# XMMatrixRotationX(), XMMatrixRotationY(), XMMatrixRotationZ()

3차원 벡터 값을 인자로 받아 해당 벡터를 X 또는 Y 또는 Z 축을 기준으로 회전시키는 회전 변환 행렬을 생성한다.

```c++
XMMATRIX XMMatrixRotationX(float Angle)
```

위 구문의 Angle 은 회전할 각도를 뜻한다. 

- 예
Angle 이 $\frac{\pi}{2}$ (90도) 라면, 생성된 회전 변환 행렬은 다음과 같다.
```
1 0           0          0
0 cos(theta) -sin(theta) 0
0 sin(theta)  cos(theta) 0
0 0           0          1
```

이 행렬은 오른손 좌표계에서 x 축을 기준으로 Angle 만큼 회전하는 변환을 나타낸다. 위의 회전 변환 행렬에서 $cos(\theta)$와 $sin(\theta)$ 는 Angle 에 대한 코사인값과 사인값이다.
또한 회전 행렬은 중첩이 가능하다.


# XMMatrixTranslation()

4x4 행렬을 생성하여 주어진 x, y, z 값으로 이동 변환을 적용하는 함수.

```c++
XMMATRIX XM_CALLCONV XMMatrixTranslation(
  [in] float OffsetX,
  [in] float OffsetY,
  [in] float OffsetZ
) noexcept;

```

XMFLOAT3 또는 XMVECTOR 형태의 위치 벡터를 인자로 받아 4x4 이동 변환 행렬을 생성하여 반환한다. 

```c++
// 반환 행렬의 형태
1       0       0       0
0       1       0       0
0       0       1       0
OffsetX OffsetY OffsetZ 1
```

# XMMatrixOrthographicLH()

^11ffd0

왼손 좌표계용 직교 투영 행렬을 작성한다.

### Syntax
```c++
XMMATRIX XM_CALLCONV XMMatrixOrthographicLH(
  [in] float ViewWidth,
  [in] float ViewHeight,
  [in] float NearZ,
  [in] float FarZ
) noexcept;
```
1. ViewWidth : 가까운 clipping plane 에서 frustum 의 너비.
2. ViewHeight  : 가까운 clipping plane 에서 frustum 의 높이.
3. NearZ : near Clipping plane 까지의 거리
4. FarZ : far Clipping plane 까지의 거리.

### Return value

직교 투영 행렬(orthogonal projection)을 반환한다.

# XMMatrixPerspectiveFovLH()

왼손 좌표계용 원근 투영 행렬을 작성한다.

## Syntax
```c++
XMMATRIX XM_CALLCONV XMMatrixPerspectiveFovLH(
  [in] float FovAngleY,
  [in] float AspectRatio,
  [in] float NearZ,
  [in] float FarZ
) noexcept;
```
1. FovAngleY : 세로 시야각
2. AspectRatio : 종횡비 (가로/세로)
3. NearZ : 시야에서 근 평면 까지의 거리
4. FarZ : 시야에서 원 평면 까지의 거리

### Return value

원근 투영 행렬(perspective projection)을 반환한다.


# XMVector3TransformNormal()

^c4c1f0


3차원 벡터를 변환하는 함수로, 주어진 벡터를 4x4 행렬로 변환하고, 결과 벡터를 반홚나다. 이 때 주어진 벡터는 법선(normal) 벡터로 취급된다. 법선 벡터는 크기가 1인 벡터로 정규화된 상태를 가지며, 벡터가 가리키는 방향은 변환에 영향을 미치지 않는다.

## Syntax
```c++
XMVECTOR XM_CALLCONV XMVector3TransformNormal(
  [in] FXMVECTOR V,
  [in] FXMMATRIX M
) noexcept;
```
1. V : 변환할 법선 벡터
2. M : 변환 행렬

## Return value

 변형된 벡터를 반환한다.

## Remarks

해당 함수는 주어진 벡터를 4차원 벡터 '(x, y, z, 0)' 으로 변환한 뒤, 이를 주어진 행렬 ' M' 으로 곱하여 결과 벡터를 반환한다. 이 때 반환된 벡터는 마지막 요소가 0인 4차 벡터이르모, 법선 벡터로 취급된다.
따라서 M 이 이동 변환이면 어차피 4차원 벡터의 w 가0 이기 때문에 원하는 데로 연산되지 않는다.

# XMLoadFloat3()

^539d69

입력된 XMVECTOR 형 값을 XMVECTOR 로 로드하여 반환한다.

## Syntax
```c++
XMVECTOR XM_CALLCONV XMLoadFloat3(
  [in] const XMFLOAT3 *pSource
) noexcept;
```

## Parameters

1. pSource
로드할 XMFLOAT3 구조체의 주소

## Return Value

pSource 매개변수의 데이터로 로드 된 XMVECTOR 를 반환한다.

## Remarks

XMFLOAT3 의 x, y, z는 그에 맞게 XMVECTOR 의 요소에 로드된다. 리턴된 XMVECTOR의 맴버 w 는 0으로 초기화 된다.

# XMStoreFloat4x4

XMFLOAT4X4 에 XMMATRIX 를 저장한다.

## Syntax

```c++
void XM_CALLCONV XMStoreFloat4x4(
  [out] XMFLOAT4X4 *pDestination,
  [in]  FXMMATRIX  M
) noexcept;
```

## Parameters

1. pDestination
데이터를 저장할 주소

2. M
저장될 데이터를 포함하는 Matrix

## Return Value

반환값이 없다.

## Remarks

XMFLOAT4X4 는 행 중심의 matrix 형식이다. 열 중심으로 작성하려면 저장 함수를 호출하기 전에 XMMatrixTranspose 로 XMMATRIX 를 치환해야 한다. 
해당 함수는 matrix 를 사용하여 16개의 single-precusion floating-point 값을  주어진 주소에 작성한다.
다음의 함수의 작동을 의사 코드로 표현하면 다음과 같다.

```c++
pDestination->_11 = M[0].x; // 4 bytes to address (uint8_t*)pDestination
pDestination->_12 = M[0].y; // 4 bytes to address (uint8_t*)pDestination + 4
pDestination->_13 = M[0].z; // 4 bytes to address (uint8_t*)pDestination + 8
pDestination->_14 = M[0].w; // 4 bytes to address (uint8_t*)pDestination + 12

pDestination->_21 = M[1].x; // 4 bytes to address (uint8_t*)pDestination + 16
pDestination->_22 = M[1].y; // 4 bytes to address (uint8_t*)pDestination + 20
pDestination->_23 = M[1].z; // 4 bytes to address (uint8_t*)pDestination + 24
pDestination->_24 = M[1].w; // 4 bytes to address (uint8_t*)pDestination + 28

pDestination->_31 = M[2].x; // 4 bytes to address (uint8_t*)pDestination + 32
pDestination->_32 = M[2].y; // 4 bytes to address (uint8_t*)pDestination + 36
pDestination->_33 = M[2].z; // 4 bytes to address (uint8_t*)pDestination + 40
pDestination->_34 = M[2].w; // 4 bytes to address (uint8_t*)pDestination + 44

pDestination->_41 = M[3].x; // 4 bytes to address (uint8_t*)pDestination + 48
pDestination->_42 = M[3].y; // 4 bytes to address (uint8_t*)pDestination + 52
pDestination->_43 = M[3].z; // 4 bytes to address (uint8_t*)pDestination + 56
pDestination->_44 = M[3].w; // 4 bytes to address (uint8_t*)pDestination + 60
```

