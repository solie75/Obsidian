 
# View Space

world space 상의 객체를 화면에 출력하기 위해서는 그것을 촬영하는 객체가 존재해야 한다. 이때 촬영을 담당하는 객체를 Camera 혹은 view 라고 하고 Camera 객체에 의해서 출력되는 영역을 view space 라고 한다.

DirectX 는 왼손 좌표계를 기준으로 하기에 카메라 객체의 우측을 x의 + 방향, 카메라의 위쪽을 y의 + 방향, 카메라에서 카메라에 촬영되는 객체로의 방향을 z의 + 방향으로 한다.

## Viewing Frustum

카메라 객체는 일정한 영역을 정하여 출력의 대상으로 삼고 그 범위는 다음의 그림과 같다

![[Pasted image 20230423052943.png]]

위의 그림을 보면 사각 뿔의 형태로 camera의 시각범위가 나타나 있음을 알 수 있다. 이때 카메라와 너무 가까운것 혹은 너무 먼 것은 시각으로 인지하기 힘들기에 이것에 대한 범위를 다음의 그림과 같이 정한다.

![[Pasted image 20230307134759.png]]

카메라에 의해 출력될 공간의 시작을 가까운 평면 (Near Plane) 이라 하고 끝을 먼 평면(Far Plane) 이라 부른다. 카메라와 Near plane 사이의 공간 혹은 카메라 기준 Far plane 너머의 공간은 화면에 출력되지 않는다. 위의 그림처럼 카메라부터 far plane 까지의 사각 뿔에서 Near Plane 이 자르는 역할을 해 잘린 사각 뿔의 형태를 띄는 near plane 에서 far plane 까지의 공간을 Viewing Frustum이라 한다.
# Projection

View space 의 Near Plane 과 Far Plane 사이의 3 차원 객체들은 결국 모니터에 출력될 때에 2차원으로 표현된다. 이와 같이 어떠한 n 차원에서 n-1 차원으로 변환되는 것을 투영이라 한다. 3차원에서 2차원으로 투영되는 경우 한 평면에 3d 객체들이 2d 의 형태로 그려지는데 이 평면을 투영 평면(projection plane)이라 한다.  투영 평면은 back buffer 에 mapping 되고 이는 곧 모니터의 화면에 출력되는 해상도를 가진다. 따라서 가로를 세로로 나누는 종횡비(Aspect Ratio)를 구할 때, 투영 평면을 기준으로 한다.

투영 평면의 이미지를 어느 크기로 출력할 지 모르기 때문에 NDC 화 하여 저장했다가 크기에 맞춰 해상도를 결정한다. 이때의 해상도는 투영  평면의 해상도 비율인 종횡비를 유지한다.

Near Plane 과 Far Plane 사이의 점 $(x, y, z)$를 투영한 투영평면 상의 점 $(x', y', z')$ 을 구하기 위해서는 $(x, y, z)$에 투영 행렬 연산을 해주어야 한다.

### NDC

Directx 에서는 객체를 그리기 위해서 3차원의 객체를 2차원 으로 투영하고 투영된 이미지의 x, y 값 이외에 객체가 화면에 출력될 순서를 알기 위해서 z 값 또한 알고 있어야 한다.

출력되는 이미지의 크기가 제각기이기 때문에 투영 행렬을 ndc 화 하여 저장한다. 하지만 이때 NDC 화를 하는 방법에 대해 여러 블로그가 다르게 알려주고 있다. 

본 글에서는 2차원을 NDC 화 하고 Z 값을 그대로 가지고 있는 방법으로 진행한다. 이 때의 NDC 는 투영 평면의 2차원 이미지를 (-1, -1), (1, 1) 의 좌표 범위를 가진 평면으로 변환한다. 또한 Z 의 값에 대하여 Near plane 를 0, Far plane 을 1로 두고  변환한다. NDC 좌표의 범위가 (-1, -1), (1, 1) 인 이유는 카메라가 위치하는 원점인 (0, 0) 이 소실점을 나타내도록 하기 위해서이다.

## Homogeneous Coordinate

직교 좌표계상에서 평행한 두 직선은 만나지 않는다. 하지만 원근법에 있어서 평행한 두 직선은 시야에서 멀어질 수록 서로 가까워 지는 것과 같이 보이고 결국에는 하나의 점으로 소실된다. 이때 이 점을 무한 원점 ( point at infinity) 이라고 한다. 이러한 무한 원점이 존재하는 좌표계를 동차 좌표계 (Homogeneous Coordinate) 라고 한다.

따라서 게임의 시야 기준으로 멀리에 있는 물체를 출력할 때 실제보다 작게 출력시키기 위해서는 투영행렬 연산을 동자 좌표계 상에서 실행되어야 한다.

직교 좌표계와 동차 좌표계의 변환의 법칙은 다음과 같다.
1. 직교 -> 동차 : $(x, y, z)$ 좌표에 대하여 임의의 값 w 를 성분 모두에 곱하고 차원을 하나 추가하여 w를 대입한다. -> $(x*w, y*w, z*w, w)$
2. 동차 -> 직교 : $(x, y, z, w)$ 좌표의 성분 모두에 w 를 나누고 차원을 하나 없앤다. -> $(\frac{x}{w},\frac{y}{w},\frac{z}{w}, 1)$
이 법칙은 동차 좌표계의 $(1, 1, 1, 1)$ 과 $(2, 2, 2, 2)$ 모두 직교좌표의 $(1, 1, 1)$ 로 변환됨을 의미한다. 
위의 두 좌표을 일반화 하면 $(1*w, 1*w, 1*w, w)$ 는 직교 좌표계 (1, 1, 1)로 변환된다. 즉, 좌표계를 직교 좌표계로 변환하는 것은 동차 좌표계 위의 한 직선에 있는 모든 점들을 직교 좌표계의 한 점으로 몰아넣는 것과 같다는 말이다.

2차원의 두 직선에 대하여 다음이 성립한다. 
$$\begin{cases}Ax+By+C = 0\\Ax+By+D =0\end{cases}\quad(C\ne D)$$
위의 두 직선은 서로 평행하여 교차하지 않는다.
직교 좌표계 위의 점 $(x, y)$를 동차 좌표계 위의 점 $(X, Y, W)$ 로 대응 된다고 보고 $(X, Y, W)$를 직교 좌표계로 변환하는 과정은 다음과 같다.
$$(\frac{X}{W},\frac{Y}{W},1)\quad -> \quad(x, y)$$
이 때 동차 좌표계의 직교좌표계로의 변환 법칙에 따라서 1 은 없애기 때문에 $x = \frac{X}{W}, y = \frac{Y}{W}$ 이다. 이것을 두 직선의 방적식에 대입하면
$$\begin{cases}A\frac{X}{W}+B\frac{Y}{W}+C = 0\\A\frac{X}{W}+B\frac{Y}{W}+D =0\end{cases}\quad(C\ne D)$$
두 식 모두의 양변에 W 를 곱하면
$$\begin{cases}AX+BY+CW = 0\\AX+BY+DW =0\end{cases}\quad(C\ne D)$$
위와 같이 모든 항의 차수가 같은 방정식이 도출된다.
이 때 두 직선은 $(A, B, 0)$에서 서로 만나며 W 가 0인 좌표가 바로 무한 원점 이다.

( ! ) $W = 0$의 의미
$W$가 $0$인 점들을 다시 유클리드 공간에 직교 좌표계로 나타내기 위해서 동차 좌표의 모든 성분에 $W$ 즉, $0$을 나누게 되는데 이는 (2차원 직교좌표계의 경우) $X$와 $Y$는 $\infty$가 되므로 직교좌표 상에서 무한을 표현할 수 있게 된다.

## Why use Homogeneous coordinate

위치 변환과 투영 변환은 단순 선형 변환이 아니기 때문에 3x3 행렬로 표현할 수 없다. 이에 렌더링 파이프 라인의 과정이 복잡해지기 때문에 행렬로 표현하기 위해서 동차 좌표 개념을 사용한다. 

1. 위치변환
선형 변환은 벡터 공간들 사이의 변환을 다루는데, 벡터는 크기와 방향 성분만 가질 뿐, 위치에 대한 정보가 없기 때문에 위치의 이동을 선형 변환으로 표현할 수 없다. 따라서 한 차원을 추가하고 아핀 변환하여 위치를 이동시킨다.

2. 투영변환
3차원의 좌표계를 2차원으로 투영시킨다. 

# Projection Matrix

### Create Perspective Projection Matrix

다음의 함수를 호출하여 원근 투영 변환 행렬을 생성한다.

```c++
XMMATRIX XM_CALLCONV XMMatrixPerspectiveFovLH(
  [in] float FovAngleY,
  [in] float AspectRatio,
  [in] float NearZ,
  [in] float FarZ
) noexcept;
```

### Process of Creating Perspective Projection Matrix

view space 가 다음의 두 사진과 같다고 가정하자.

![[Pasted image 20230307203438.png]]
<사진 1>
![[Pasted image 20230423221230.png]]
<사진 2>

사진 2에서 투영이 되는 대상의 좌표는 $(X_v\;,Y_v\;, Z_v)$ 이고 투영 평면의 좌표를 $(X_n\;, Y_n\;, Z_n)$ 이다.

$(X_v\;,Y_v\;, Z_v)$ 에 원근 투영 행렬을 연산하여 $(X_n\;, Y_n\;, Z_n)$로 변환시켜야 한다.

1. $(X_v\;,Y_v\;, Z_v)$ 에 차원을 추가하여 동차 좌표계로 만들어 준다. -> $(X_v\;,Y_v\;, Z_v\;, 1)$ 

종횡비를 $A$ 라고 할 때

y 값의 투영 연산을 통한 변화의 경우

$Z_n : Y_n = Z_v : Y_v => Y_n = \frac{Z_n}{Z_v}Y_v$

x 값의 투영 연산을 통한 변화의 경우

$Z_n : X_n = Z_v : X_v => X_n = \frac{Z_n}{A*Z_v}X_v$

왜 x 값의 경우 y 값과 다르게 종횡비를 곱해주는 것일까?
	출력 되는 이미지의 너비가 높이보다 상대적으로 큼에도 불구하고 투영 평면을 NDC 화 할 때, 너비와 높이 모두 -1 부터 1 까지로 동일하게 변화시기키 때문이다. 따라서 $\frac{W(투영 평면 너비)}{H(투영 평면 높이)}=A$ 에서 H 가 1인 경우 W = A 이다. 이 W 가 1이 되기 위해서는 W에 $\frac{1}{A}$ 를 곱해줘야 하고 이는 곧$X_v$를 $X_n$으로 변환시킬 때 $X_v$에 $\frac{1}{A}$ 를 곱해줘야 함을 의미란다 따라서 x 값의 변화의 경우 y 값의 변화와 다르게 종횡비의 역수를 곱해주는 것이다. 

위의 두 변환을 행렬 연산으로 나타내면 다음과 같다.
$$\begin{bmatrix}X_v&Y_v&Z_v&1\end{bmatrix}\begin{bmatrix}\frac{Z_n}{A\cdot Z_v}&0&m_{13}&m_{14}\\0&\frac{Z_n}{Z_v}&m_{23}&m_{24}\\0&0&m_{33}&m_{34}
\\0&0&m_{43}&m_{44}\end{bmatrix} = \begin{bmatrix}X_n&Y_n&Z_n&1\end{bmatrix}$$
 
### Create Orthographic Projection Matrix

다음의 함수를 호출하여 직교 투영 행렬을 생성한다.

```c++
XMMATRIX XM_CALLCONV XMMatrixOrthographicLH(
  [in] float ViewWidth,
  [in] float ViewHeight,
  [in] float NearZ,
  [in] float FarZ
) noexcept;
```

### Process of Creating Orthographics Projection Matrix