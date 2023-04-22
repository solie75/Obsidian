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