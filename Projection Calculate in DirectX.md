 
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

^2e7101

Directx 에서는 객체를 그리기 위해서 3차원의 객체를 2차원 으로 투영하고 투영된 이미지의 x, y 값 이외에 객체가 화면에 출력될 순서를 알기 위해서 z 값 또한 알고 있어야 한다.

출력되는 이미지의 크기가 제각기이기 때문에 투영 행렬을 ndc 화 하여 저장한다. 하지만 이때 NDC 화를 하는 방법에 대해 여러 블로그가 다르게 알려주고 있다. 

본 글에서는 2차원을 NDC 화 하고 Z 값을 그대로 가지고 있는 방법으로 진행한다. 이 때의 NDC 는 투영 평면의 2차원 이미지를 (-1, -1), (1, 1) 의 좌표 범위를 가진 평면으로 변환한다. 또한 Z 의 값에 대하여 Near plane 를 0, Far plane 을 1로 두고  변환한다. NDC 좌표의 범위가 (-1, -1), (1, 1) 인 이유는 카메라가 위치하는 원점인 (0, 0) 이 소실점을 나타내도록 하기 위해서이다.

기존  투영창의 높이를 1로 기준삼아 비율을 두면 다음의 식이 성립한다.
$$너비 = 종횡비*높이(1) => 너비 = 종횡비$$
NDC 좌표에서는 너비와 높이의 크기가 같기 때문에 너비에 $\frac{1}{종횡비}$ 를 곱하여 기준인  높이와 크기를 같게 만들어 준다. 한마디로 $\frac{1}{종횡비}$ 를 x 값에 곱해줌으로써 투영창의 이미지가 좌우로 압축되어 NDC 화 되었다고 보면된다.



## Homogeneous Coordinate

직교 좌표계상에서 평행한 두 직선은 만나지 않는다. 하지만 원근법에 있어서 평행한 두 직선은 시야에서 멀어질 수록 서로 가까워 지는 것과 같이 보이고 결국에는 하나의 점으로 소실된다. 이때 이 점을 무한 원점 ( point at infinity) 이라고 한다. 이러한 무한 원점이 존재하는 좌표계를 동차 좌표계 (Homogeneous Coordinate) 라고 한다.

따라서 게임의 시야 기준으로 멀리에 있는 물체를 출력할 때 실제보다 작게 출력시키기 위해서는 투영행렬 연산을 동자 좌표계 상에서 실행되어야 한다.

직교 좌표계와 동차 좌표계의 변환의 법칙은 다음과 같다.
1. 직교 -> 동차 : $(x, y, z)$ 좌표에 대하여 임의의 값 w 를 성분 모두에 곱하고 차원을 하나 추가하여 w를 대입한다. -> $(x*w, y*w, z*w, w)$
2. 동차 -> 직교 : $(x, y, z, w)$ 좌표의 성분 모두에 w 를 나누고 차원을 하나 없앤다. -> $(\frac{x}{w},\frac{y}{w},\frac{z}{w})$
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

사진 2에서 투영이 되는 대상의 좌표는 $(X_v\;,Y_v\;, Z_v)$ 이고 NDC의 좌표는 $(X_n\;, Y_n\;, Z_n)$ 이다.

$(X_v\;,Y_v\;, Z_v)$ 의 동차좌표에 투영 연산을 실시하여 $(X_n\;, Y_n\;, Z_n)$ 의 동차좌표를 도출한다. 투영 연산은 $(X_v\;,Y_v\;, Z_v)$ 과 $(X_n\;, Y_n\;, Z_n)$ 를 조건으로 유추한다.

1. 직각삼각형의 비율을 이용하여 다음 변환식을 구한다.

종횡비를 $A$ 라고 할 때

y 값의 투영 연산을 통한 변화의 경우

$Z_n : Y_n = Z_v : Y_v => Y_n = \frac{Z_n}{Z_v}Y_v$

x 값의 투영 연산을 통한 변화의 경우

$Z_n : X_n = Z_v : X_v => X_n = \frac{Z_n}{A*Z_v}X_v$

[[Projection Calculate in DirectX#^2e7101|왜 x 값의 경우 y 값과 다르게 종횡비를 곱해주는 것일까?]]
	기존의 투영 평면을 NDC 화 하는 과정에서 높이를 1값으로 기준사망 너비에도 특정한 값을 곱하여 그 크기가 높이와 같게 만들어준다. 이때 높이를 1이라고 하면 너비 = 종횡비 이므로 너비에 $\frac{1}{종횡비}$를 곱해주어 높이와 너미의 비율을 1 : 1 로 만든다.

위의 두 변환을 행렬 연산으로 나타내면 다음과 같다.
$$\begin{bmatrix}X_v&Y_v&Z_v&1\end{bmatrix}\begin{bmatrix}\frac{Z_n}{A\cdot Z_v}&0&0&0\\0&\frac{Z_n}{Z_v}&0&0\\0&0&m_{33}&1
\\0&0&m_{43}&0\end{bmatrix} = \begin{bmatrix}X'&Y'&Z'&W'\end{bmatrix}$$

3. $Z_v$ 가 $Z_n$ 으로 변화하는데 x 와 y 의 값이 관여하는 바가 없음으로 $m_{13}$과$m_{23}$ 은 0이다.
4. 또한 객체들의 출력 순서를 위해 각 깊이 값을 알고 있어야 한다. 이를 위해 4열 중 $m_{34}$를 1로 하고 나머지는 0으로 하여 $Z_v$값을 온전히 가지고 있는다.  또한 $X_n$과 $Y_n$을 각각 $X_v$ 와 $Y_v$로 표현하면 다음의 행렬 연산이 된다.
$$\begin{bmatrix}X_v&Y_v&Z_v&1\end{bmatrix}\begin{bmatrix}\frac{Z_n}{A\cdot Z_v}&0&0&0\\0&\frac{Z_n}{Z_v}&0&0\\0&0&m_{33}&1
\\0&0&m_{43}&0\end{bmatrix} = \begin{bmatrix}\frac{Z_n}{A*Z_v}X_v&\frac{Z_n}{Z_v}Y_v&Z'&W'\end{bmatrix}$$

위의 식에서 NDC의 x 값과 y값 은 모두 $Z_v$ 를 분모로 가지고 있다. 위에 삼각형의 비율로 구한 것이지만 개념적으로 보면 기존의 기존 좌표의 모든 성분에 Z_v 를 나누어 Z_v 값이 클 수록 NDC 상의 객체의 크기가 작도록 하기 위함이다. $Z_v = W'$ 라고 할 때, $(\frac{Z_n}{A\cdot Z_v},\frac{Z_n}{Z_v}, 1)$ 는 $(\frac{Z_n}{A}, Z_n)$ 의 동자 좌표와 같다. 따라서 동차 좌표계의 개념에서 직교 좌표계의 개념으로 행렬 연산을 바꿔 주기 위해 다음과 같이 연산을 변경한다.
$$\begin{bmatrix}X_v&Y_v&Z_v&1\end{bmatrix}\begin{bmatrix}\frac{Z_n}{A}&0&0&0\\0&Z_n&0&0\\0&0&m_{33}&1
\\0&0&m_{43}&0\end{bmatrix} = \begin{bmatrix}\frac{Z_n}{A}X_v&Z_nY_v&m_{33}Z_v+m_{43}&Z_v\end{bmatrix}$$
5. $m_{33}$과 $m_{43}$ 구하기
$Z_v$가 Far plane 일 때 $Z_n$ 은 1이고 다음과 같다. ($Z_v$ 를 f 라고 표기)
	$m_{33}f+m_{43} = 1, m_{43} = 1-m_{33}f$
$Z_v$가 Near plane 일 때 $Z_n$ 은 0이고 다음과 같다. ($Z_v$ 를 n 라고 표기)
	$m_{33}n+m_{43} = 0$ 에 $m_{43} = 1-m_{33}f$ 를 대입한다.
	$m_{33}n+1-m_{33}f=0$
	$m_{33} = \frac{1}{f-n},\quad m_{43}=\frac{n}{n-f}$

6. 투영행렬
결과적으로 투영행렬을 다음과 같다.
$$\begin{bmatrix}\frac{Z_n}{A}&0&0&0\\0&Z_n&0&0\\0&0&\frac{1}{f-n}&1
\\0&0&\frac{n}{n-f}&0\end{bmatrix}$$

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


![[Pasted image 20230424194905.png]]

위에서 다룬 원근 투영 행렬은 카메라 를 기준으로 멀어짐에 따라 거리에 비례하여 객체가 원래의 크기보다 작아짐을 다루었다. 하지만 게임에서는 UI 와 같이 거리와 상관 없이 항시 사용자에게 같은 크기로 비춰저야 하는 객체들 또한 존재한다. 이러한 객체들에 대하여 객체의 출력 우선 순위에 대한 데이터를 부여하지만 크기의 변화는 없도록 하는 용도로 직교 투영 행렬을 사용한다. 

The **orthographic projection** (also sometimes called oblique projection) is simpler than the other types of projection and learning about it is a good way of apprehending how the perspective projection matrix works. You might think that orthographic projections are of no use today. Indeed, what people strive for whether in films or games, is photorealism for which perspective projection is generally used. Why bother then with learning about parallel projection? Images rendered with an orthographic camera can give a certain look to a video game that is better (sometimes) than the more natural look produced with perspective projection. Famous games such as the Sims or Sim City adopted this look. Orthographic projection can also be used to render shadow maps, or render orthographic views of a 3D model for practical reasons: an architect for example may need to produce blueprints from the 3D model of a house or building designed in CAD software.

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

직교 투영은 z 값에 상관 없이 변환 후에도 x, y 값이 그대로 보존 된다.

The goal of this orthographic projection matrix is to remap all coordinates contained within a certain bounding box in 3D space into the **canonical viewing volume** (we introduced this concept already in chapter 2). If you wonder what that original box is, then just imagine that this is a bounding box surrounding all the objects contained in your scene. Very simply, you can loop over all the objects in your scene, compute their bounding box, and extend the dimensions of the global bounding box so that it contains all the bounding volumes of these objects. Once you have the bounding box for the scene, then the goal of the orthographic matrix is to remap it to a canonical view volume. This volume is a box in whose minimum and maximum extents are respectively (-1, -1, -1) and (1, 1, 1) (or (-1,-1,0) and (1,1,1) depending on the convention you are using). In other words, the x- and y-coordinate of the projected point are remapped from wherever they were before the projection, to the range [-1,1]. The z-coordinate is remapped to the range [-1,1] (or [0,1]). Note that both bounding boxes (the scene bounding box and the canonical view volume) are AABBs (axis-aligned bounding boxes) which simplifies the remapping process a lot.

![[Pasted image 20230424201623.png]]

the orthographic projection matrix remaps the scene bounding volume to the canonical view volume. All points contained in the scene bounding volume have their projected XY coordinates "normalized" (they lie within the range [-1,1]

Once we have computed the scene bounding box, we need to project the minimum and maximum extents of this bounding box onto the image plane of the camera. In the case of an orthographic projection (or parallel projection), this is trivial. The x- and y-coordinates of any point expressed in camera space and the x- and y-coordinates of the same points projected on the image plane are the same. You need to potentially extend the projection of the minimum and maximum extents of the bounding box onto the screen for the screen window itself to be either square or have the same ratio as the image aspect ratio (check the code of the test program below to see how this can be done). Remember also that the canvas or screen is centered around the screen coordinate system origin

![[Pasted image 20230424202157.png]]

위의 screen 에서 좌우 상하를 각각 l, r, t, b 이라고 할때 

We now need to remap the left and right screen coordinates (l, r) to -1 and 1 and do the same for the bottom and right coordinates. Let's see how we can do that. Let's assume that x is any point contained within the range [l,r].

즉,
$$\begin{align}&l\le x\le r \\ &0 \le x-l \le r-l\\ &0\le \frac{x-l}{r-l}\le 1\\&0\le 2\frac{x-l}{r-l}\le 2\\ &-1\le 2\frac{x-l}{r-l}-1\le 1\\ & -1\le \frac{2x}{r-l}-\frac{r+l}{r-l}\le 1\end{align}$$
따라서 $x' = \frac{2x}{r-l}-\frac{r+l}{r-l}$ 를 도출할 수 있다. 이를 행렬로 하면 다음과 같다.
$$\begin{bmatrix}\frac{2}{r-l}&0&0&0\\0&1&0&0\\0&0&1&0\\-\frac{r+l}{r-l}&0&0&1\end{bmatrix}$$
y축에 대해서도 똑같이 했을 때 행렬은 다음과 같다.
$$\begin{bmatrix}\frac{2}{r-l}&0&0&0\\0&\frac{2}{t-b}&0&0\\0&0&1&0\\-\frac{r+l}{r-l}&-\frac{t+b}{t-b}&0&1\end{bmatrix}$$

z 값은 n (near plane)과 f (far plane) 사이에 있어야 한다. 
$$\begin{align}&n\le z\le f \\ &0 \le z-n \le f-n\\ &0\le \frac{z-n}{f-n}\le 1\\&0\le \frac{z}{f-n}-\frac{n}{f-n}\le1\end{align}$$
이에 따라서 직교 투영 행렬은 최종적으로 다음과 같다.
$$\begin{bmatrix}\frac{2}{r-l}&0&0&0\\0&\frac{2}{t-b}&0&0\\0&0&\frac{1}{f-n}&0\\-\frac{r+l}{r-l}&-\frac{t+b}{t-b}&-\frac{n}{f-n}&1\end{bmatrix}$$

# Reference

https://www.scratchapixel.com/lessons/3d-basic-rendering/perspective-and-orthographic-projection-matrix/projection-matrices-what-you-need-to-know-first.html