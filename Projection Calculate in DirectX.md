
# View Space

world space 상의 객체를 화면에 출력하기 위해서는 그것을 촬영하는 객체가 존재해야 한다. 이때 촬영을 담당하는 객체를 Camera 혹은 view 라고 하고 Camera 객체에 의해서 출력되는 영역을 view space 라고 한다.

## Frustum

카메라 객체는 일정한 영역을 정하여 출력의 대상으로 삼고 그 범위는 다음의 그림과 같다

![[Pasted image 20230423052943.png]]

위의 그림을 보면 사각 뿔의 형태로 camera의 시각범위가 나타나 있음을 알 수 있다. 이때 카메라와 너무 가까운것 혹은 너무 먼 것은 시각으로 인지하기 힘들기에 이것에 대한 범위를 다음의 그림과 같이 정한다.

![[Pasted image 20230307134759.png]]

카메라에 의해 출력될 공간의 시작을 가까운 평면 (Near Plane) 이라 하고 끝을 먼 평면(Far Plane) 이라 부른다. 카메라와 Near plane 사이의 공간 혹은 카메라 기준 Far plane 너머의 공간은 화면에 출력되지 않는다. 위의 그림처럼 카메라부터 far plane 까지의 사각 뿔에서 Near Plane 이 자르는 역할을 해 잘린 사각 뿔의 형태를 띄는 near plane 에서 far plane 까지의 도형을 절두체(frustum)이라 한다. 이때 Near Plane의 너비를 높이로 나눈 값 을 종횡비(Aspect Ratio) 라고 한다.
# Projection

View space 의 Near Plane 과 Far Plane 사이의 3 차원 객체들은 결국 모니터에 출력될 때에 2차원으로 표현된다. 이와 같이 어떠한 n 차원에서 n-1 차원으로 변환되는 것을 투영이라 한다. 3차원에서 2차원으로 투영되는 경우 한 평면에 3d 객체들이 2d 의 형태로 그려지는데 이 평면을 투영 평면(projection plane) 이라 한다.

Near Plane 과 Far Plane 사이의 점 $(x, y, z)$를 투영한 투영평면 상의 점 $(x', y', z')$ 을 구하기 위해서는 $(x, y, z)$에 투영 행렬 연산을 해주어야 한다.

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

![[Pasted image 20230423215059.png]]
view space 의 단면도가 위와 같을 때

다음 사진에서
![[Pasted image 20230423221230.png]]
단면도의 위쪽 부분이 좌측 도형 view space 의 평면도의 우측 부분이 위 사진의 우측 도형이라고 가정하자. 투영이 되는 대상의 좌표는 $(X_v\;,Y_v\;, Z_v)$ 이고 투영 평면의 좌표를 $(X_n\;, Y_n\;, Z_n)$ 이다.



$(X_v\;,Y_v\;, Z_v)$ 에 원근 투영 행렬을 연산하여 $(X_n\;, Y_n\;, Z_n)$로 변환시켜야 한다.

$(X_v\;,Y_v\;, Z_v)$ 에 차원을 추가하여 동차 좌표계로 만들어 준다. -> $(X_v\;,Y_v\;, Z_v\;, 1)$ 

종횡비를 a 라고 할 때

y 값의 투영 연산을 통한 변화의 경우

$Z_n : Y_n = Z_v : Y_v => Y_n = \frac{Z_n}{Z_v}Y_v$

x 값의 투영 연산을 통한 변화의 경우

$Z_n : X_n = Z_v : X_v => X_n = a\frac{Z_n}{Z_v}X_v$

왜 x 값의 경우 y 값과 다르게 종횡비를 곱해주는 것일까?
	

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