3차원의 좌표는 (x, y, z) 3가지 성분으로 표현된다.

하지만 동차 좌표에서는 여기에 무한 원점이라는 개념을 추가한다.

무한 원점(point at infinity) 이란 직선이나 평면의 끝에 추가하는 가상의 점으로 벡터와 점을 구별하는데 쓰인다. 즉, 3차원에서 어느 한 지점을 표시할 때 이 지점이 벡터인지 점인지를 구별한다. (0일때 벡터, 1일 때 점)

world space 를 기준으로 한 local space 의 각 요소 가 다음과 같을 때
원점의 동자 좌표 $Q_w=(Q_x, Q_y, Q_z,1)$,
x 축의 동자 좌표 $u_w=(u_x, u_y, u_z,0)$,
y 축의 동자 좌표 $V_w=(v_x, v_y, v_z,0)$,
z 축의 동자 좌표 $W_w=(w_x, w_y, w_z,0)$,

local space 를 world space 로 변환하는 좌표 변환 행렬(world matrix)은 다음과 같다.
$$W=\begin{bmatrix}u_x&u_y&u_z&0\\ v_x&v_y&v_z&0\\w_x&w_y&w_z&0\\Q_x&Q_y&Q_z&1\end{bmatrix}$$
이러한 world matrix 를 만들기 위해서는 world space 를 기준으로 local space 원점 및 축들의 좌표를 알아야 한다. 이를 흔한 접근방식으로 표현하면 W 를 일련의 변환들로 정의하는 것이다. 예를 들어 W=SRT와 같이 Scaling Matrix ' S ' (오브젝트를 world matrix 안에서 적절한 크기가 되게 만든다.), Rotation Matrix ' R ' (오브젝트를 world matrix 안에서 적절한 방향이 되게 만든다.), Translation Matrix ' T ' (오브젝트를 worlad matrix 안에서 적절한 위치로 이동시킨다.) 를 곱하는 것이다. 이러한 일련의 변환들을 하나의 좌표 변경 변환 으로 간주할 수 있으며, W = SRT 의 행벡터 들은 world space를 기준으로 한 local space 의 x 축, y 축, z 축, 원점의 동차 좌표를 담는다.

최종적으로 화면에 표시되는 2차원의 이미지를 만들기 위해서는 scene 에 가상의 카메라를 배치해야 한다. 그 카메라에 local space 를 부여한다고 할 때에 이 좌표계가 view space (시야 공간) 으로 eye space (시점 공간) 또는 카메라 공간(camera space) 라고 부르기도 한다. 카메라는 이 공간의 원에 좋여서 양의 z 축을 바라본다. x 축은 카메라의 오른쪽 방향이고 y 축은 카메라의 위쪽 방향이다.
![[Pasted image 20230307171737.png]]
![[Pasted image 20230307134759.png]]

world space 에서 view space 로의 좌표 변경 변환을 view transform 이라고 부르고 해당 변환 행렬을 view matrix 라고 한다.

view space 에서 world space 로의 translation matrix 는 local space 에서 world space 로의 translation matrix 와 같아 다음과 같다.
$$W=\begin{bmatrix}u_x&u_y&u_z&0\\ v_x&v_y&v_z&0\\w_x&w_y&w_z&0\\Q_x&Q_y&Q_z&1\end{bmatrix}$$
world space 에서 view space 로의 변환은 위의 변환의 역행렬이다. 따라서 $W^{-1}$이 world space 에서 view space 로의 translation matrix 이다.

일반적으로 world 좌표계와 view 좌표계는 위치와 방향만 다르므로, W = RT 라고 두어도 무방하다. (즉, 세계 행렬은 회전 다음에 이동을 적용하는 것으로 분해 할 수 있다.) 그 역행렬은 다음과 같다.

$$\begin{align}&V=W^{-1}=(RT)^{-1}=T^{-1}R^{-1}=T^{-1}R^{-T}\\=&\begin{bmatrix}1&0&0&0\\0&1&0&0\\0&0&1&0\\-Q_x&-Q_y&-Q_z&1\end{bmatrix}\begin{bmatrix}u_x&u_y&u_z&0\\ v_x&v_y&v_z&0\\w_x&w_y&w_z&0\\0&0&0&1\end{bmatrix}=\begin{bmatrix}u_x&u_y&u_z&0\\ v_x&v_y&v_z&0\\w_x&w_y&w_z&0\\-Q_u&-Q_v&-Q_w&1\end{bmatrix}\end{align}$$

따라서 view matrix 는 다음과 같다.
$$V=\begin{bmatrix}u_x&u_y&u_z&0\\ v_x&v_y&v_z&0\\w_x&w_y&w_z&0\\-Q_u&-Q_v&-Q_w&1\end{bmatrix}$$

예시의 상황이 다음과 같다고 할 때
![[Pasted image 20230307175655.png]]

Q 는 카메라의 위치, T 는 카메라가 바라보는 지점, j 는 world space 의 '위쪽' 을 나타내는 단위벡터라 하자
$$w=\frac{T-Q}{||T-Q||}$$
w 벡터는 카메라의 local space 의 z 축에 해당한다.
$$u=\frac{j\times w}{||j\times w||}$$
u 벡터는 w 의 오른쪽을 가리키며 카메라의 local space 의 x 축에 해당한다.
$$v=w\times u$$
v 는 카메라의 local space 의 y 축이다. w 와 u 가 서로 직교인 단위벡터이므로  $w\times u$ 도 반드시 단위벡터이다. 따라서 정규화할 필요가 없다.

이와 같이 카메라의 위치와 대상점 그리고 world space 기준 상향점(j) 만있다면 해당 카메라를 서룻하는 local 좌표계를 알 수 있고 그것을 이용하여 view matrix 를 구할 수 있다.

XNA Math 라이브러리에서 위의 절차에 따라 view matrix 를 셰산하는 다음의 함수를 세공한다.
```c++
XMMATRIX SMMATRIXLOOKATLH(    // 계산된 view matirx v를 출력
	FXMVECTOR Eyeposition,    // 입력 카메라 위치 Q
	FXMVECTOR focusposition,  // 입력 대상 점 T
	FXMVECTOR UpDirection     // 입력 세계 상향 벡터 j
);
```

일반적으로 world space 의 y 축이 위쪽 방향에 해당하므로 상향 벡터는 j = (0, 1, 0) 이다. 

예를 들어 카메라가 world space 의 (5, 3, -10)에 위치하며 world space origin  (0, 0, 0) 을 바라본다고 할 때, 해당 view matrix 를 구하는 코드는 다음과 같다.
```c++
XMVECTOR pos = XMVectorSet(5, 3, -10, 1.0f);
XMVECTOR target = XMVectorZero();
XMVECTOR up = XMVectorSet(0.0f, 1.0f, 0.0f, 0.0f);

XMMATRIX V = XMMatrixLookAtLH(pos, target, up);
```

# projection & frustum space

![[Pasted image 20230307202639.png]]

3차원의 기하구조를 2차원으로 projection 한다. 평행선들이 하나의 소실점으로 수렴하는 방식으로 이루어지면 이로 인해서 오브젝트의 3차원 깊이 값에 따라 투영괸 경과 의 크기가 조정된다. perspective projection 이 이 방식의 투영이다. 정점에서 시점으로의 직선을 line of projection (투영선) 라고 한다.
![[Pasted image 20230307203019.png]]
위의 그림에서 3차원 정점 v 를 2차원 투영창 과 만나는 지점 v' 로 변환시키는 것을 perspective projection translation 라고 한다. 이 때 v' 를 v 의 투영이라고 부른다.

# frustum

view space 에서 projection의 중심을 원점에 두고 양의 z 축을 바라보는 view frustum 은 다음의 네 가지 수량을 가지고 정의할 수 있다. 
1. 원점과 가까운 평면 사이의 거리 n
2. 원점과 먼 평면 사이의 거리 f
3. 수직 시야각 a
4. 종횡비 r
![[Pasted image 20230307203438.png]]

view space 에서 near plane(가까운 평면) 과 far plane(먼 평면) 은 모두 xy 평면과 평행하다.따라서 원점과 두 평면 사이의 거리는 view space 의 z 축상의 거리일 뿐이다.종횡비(aspect ratio) 는 $r=w/h$ 로 정의되는데, 여기서 w 는 투영창의 너비이고 h 는 투영창의 높이이다.
투영 창은 결국 view space 의 안의 장면의 2차원 이미지이다. 이 <span style="color: yellow">이미지가 back buffer 에 mapping 되므로 투영창의 너비와 높이를 back buffer 의 너비와 높이의 비율과 같게 만드는 것이 바람직하다.</span>
따라서 일반적으로 투영 창의 aspect ratio 는 back buffer의 aspect ratio 를 따른다. 예를 들어 back buffer 가 800x600 이면 $r=\frac{800}{600}$ 으로 대략 1.333 이다. 투영창의 aspect ratio 와 back buffer 의 aspect ratio 가 다른 경우 투영창을 back buffer 에 projection 할 대 비균등 비례가 필요하고 이러면 이미지가 왜곡된다.

수평 시야각을 $\beta$ 로 표기하는데, 이 시야각은 수직 시야각 $\alpha$ 와 aspect ratio 인 $r$ 로 구할 수 있다. 

투영창의 높이를 2로 할 때 (투영창을 ndc 변형 할 때에 x, y 둘 중 하나를 기준 삼고 다른 하나는 비율대로 변형해야 하기 때문에 높이를 ndc 의 높이에 따라서 2로 고정하여 x, d 등을 계산한다.)

$r=\frac{w}{2}=\frac{w}{2} => w = 2r$

원점과 두형 창 사이의 거리인 d 는 다음과 같다.

$\tan(\frac{\alpha}{2})=\frac{1}{d} => d = \cot(\frac{\alpha}{2})$

위의 조건(r, d) 이 주어졌을 때 $\beta$ 는 다음과 같다.

$\tan(\frac{\beta}{2})=\frac{r}{d}=\frac{r}{\cot(\frac{\alpha}{2})} = r\cdot tan(\frac{\alpha}{2})$


# 정점의 투영

![[Pasted image 20230309020910.png]]

점 (x, y, z)가 주어졌을 때 그것을 z = d 의 투영 평면 에 투영한 점 (x', y', z') 를 구하고자 할 때, 다음과 같다.
$$\frac{x'}{d}=\frac{x}{z}\quad=>\quad x'=\frac{xd}{z}=\frac{x\cdot cot(\frac{\alpha}{2})}{z}=\frac{x}{z\cdot tan(\frac{\alpha}{2})}$$
$$\frac{y'}{d}=\frac{y}{z}\quad=>\quad y'=\frac{yd}{z}=\frac{y\cdot cot(\frac{\alpha}{2})}{z}=\frac{y}{z\cdot tan(\frac{\alpha}{2})}$$

# 정규화된 장치 좌표 (NDC)

지금 까지는 투영창의 너비를 주어지는 종횡비를 이용하여 계산했다. 하지만 이는 하드웨어가 투영 창의 크기가 관여하는 연산(투영 창을 back buffer에 mapping하는 등의) 을 수행하기 위해서는 하드웨어에게 aspect ratio 를 알려 주어야 한다는 것을 말한다. 이러한 aspect ratio 에 대한 의존성을 없애기 위해서  [-r,r] 구간 (여기에서 r 은 aspect ratio) 가 아닌 [-1, 1] 로 비례시킨다.
$$\begin{align}&-r\le x'\le r\\&-1\le \frac{x'}{r}\le 1\end{align}$$
이러한 mapping 을 수행한 뒤의 x, y 좌표를 NDC (Normalized device coordinates, 정규화된 장치 좌표) 라고 부른다.

이에 따라 점 $(x,y, z)$ 는
$$\begin{align}&-1\le \frac{x'}{r}\le 1\\&-1\le y'\le 1\\&\quad n\le z \le f\end{align}$$
를 만족해야 frustum 안에 있다고 할 수 있다.

view space 에서 NDC 공간으로의 변환을 일종의 단위 변환(unit conversion) 으로 볼 때, x 축에서 한 단위는 시야 공간의 aspect ratio 의 단위와 같다. (즉, 1ndc = r vs). 따라서 시야 공간의 x 단위를 ndc의 단위로 변환한다면 다음의 공식을 사용한다.
$$x\;vs\cdot\frac{1\;ndc}{r\;vs}=\frac{x}{r}ndc$$
# projection translation 의 matrix 표현

3차원의 장면을 2차원으로 변환하기 위해서 사전 작업으로 z 축의 범위를 0~1로 한정한다. 해당 z 값의 변화에 따라 나머지 x 나 y 값도 변화하면서 카메라로부터의 거리에 따라 크기 변화를 유도하는 것이다. (z 축 기준 0 은 near plane 1은 far plane 이다.) 

따라서 x 와 y 는 -1 ~ 1 의 범위로 z 는 0 ~ 1의 범위로 변환된다. (이 때 x, y 가 -1 부터 1 이 범위인 이유는 그래야 중앙에 원점을 고정하여 소실되지 않기 때문이다.) 

이전 정점의 투영의 식에서 투영창과 실제 오브젝트의 좌표에 대해 비율을 사용하여 정리한 결과. 기준의 x, y 의 좌표에 $\frac{1}{tan(\frac{\alpha}{2})}$ 를 곱한뒤 x 좌표는 $\frac{1}{r}$ 을 곱한 다음. 점의 모든 성분(x, y, z, 동차) 에 z를 나누어 카메라로부터 먼 오브젝트는 그 크기가 전체적으로 작아지게 한다. 결과적으로 projection matrix 를 거친 정점은 원근법이 적용된 2차원의 좌표계 내의 정점으로 변환되는 것이다. (이러한 z로 나누기를 원근 나누기(perspective divide) 또는 동차 나누기( homogeneous divide)라고 부르기도 한다.) 마지막 과정으로 정점의 모든 성분에 z 를 나누기 위하여 z 값을 행렬이 알고 있어야 하기에 4행 3열을 1로 설정하고 4행4열을 0으로 설정한다.

이에 따라 projection matrix 는 다음과 같다.
$$P=\begin{bmatrix}\frac{1}{r\cdot tan(\frac{\alpha}{2})}&0&0&0\\0&\frac{1}{tan(\frac{\alpha)}{2}}&0&0\\0&0&A&1
\\0&0&B&0\end{bmatrix}$$
projection matrix에 임의의 점 (x, y, z, 1)에 위의 행렬을 곱하면 다음과 같다.
$$\begin{bmatrix}x,y,z,1\end{bmatrix}\begin{bmatrix}\frac{1}{r\cdot tan(\frac{\alpha}{2})}&0&0&0\\0&\frac{1}{tan(\frac{\alpha)}{2}}&0&0\\0&0&A&1
\\0&0&B&0\end{bmatrix}=\begin{bmatrix}\frac{x}{r\cdot tan(\frac{\alpha}{2})},\frac{y}{tan(\frac{\alpha}{2})},AZ+B, z\end{bmatrix}$$

다음으로 정점의 모든 성분에 z 를 나눈다.
$$\begin{bmatrix}\frac{x}{rz\cdot tan(\frac{\alpha}{2})},\frac{y}{z\cdot tan(\frac{\alpha}{2})},A+\frac{B}{z}, 1\end{bmatrix}$$

# 정규화된 깊이 값

투영된 모든 정점들은 2차원의 투영창에 놓인 상태로 깊이 값이 없어도 될 것 같지만. [[Depth Buffering]] 알고리즘을 위해서는 3차원 깊이에 대한 정보가 필요하다. Direct3D 는 투영된 x, y 좌표가 정규화된 범위이길 요구하는 것과 마찬가지로 깊이 성분 또한 정규화된 [0,1]이길 요구한다. 따라서 [n, f]를 [0, 1]로 mapping 하는 순서 보존 함수 g(z)를 만들 필요가 있다.

순서 보존 함수란 $z_1. z_2 \in [n,f]$ 이고 $z_1 <z_2$ 이면 $g(z_1)<g(z_2)$가 성립한다는 것으로 값들의 상대적 관계의 유지 (카메라부터의 거리 순서)를 말한다. (depth buffering 알고리즘에 필요한 것은 정점 사이의 깊이 값의 상대적 관계 뿐임을 주의할 것.)

위의 projectino matrix 를 보면 z 는 다음의 변환을 거친다.
$$g(z)=A+\frac{B}{z}$$
이 z 값에 대하여 ' [n, f]의 [0, 1]로의 mapping ' 이 성립하려면 A와 B는 다음의 두 조건을 만족하여야 한다. 
$$\begin{align}&조건\;1\;:\;g(n)=A+\frac{B}{n}=0\\&조건\;2\;:\;g(f)=A+\frac{B}{f}=1\end{align}$$
조건 1을 B 에 대해 정리하면 $B=-A\cdot n$ 이다. 이를 조건 2에 대입하면 다음과 같다.
$$\begin{align}&A+\frac{-An}{f}=1\\&\frac{Af-An}{f}=1\\&Af-An=f\\&A=\frac{f}{f-n}\end{align}$$
따라서
$$g(z)=\frac{f}{f-n}-\frac{fn}{(f-n)z}$$
이다.

![[Pasted image 20230309213855.png]]
위의 그래프는 strictly increasing (순증가, 즉 순서 보존) 인 g의 그래프로 비선형이다. 치역의 대부분이 near plane 과 가까이 있기에 depth buffer 에서 정밀도 문제가 발생할 수 있다. (여기에서 정밀도 문제란 유한한 수치의 표현 때문에 변환된 두 깊이 값이 약간만 달라 컴퓨터가 그 둘을 구분하지 못하는 것이다.)(이 정밀도 문제에 대한 해결중 하나는 near plane 과 far plane을 최대한 가깝게 해서 깊이 정밀도 문제 자체를 최소화 하는 것이다.)

n, f 를 이용하여 나타낸 A, B 를 위의 projection matrix 에 대입한 perspective projection matrix의 최종형태는 다음과 같다.
$$P=\begin{bmatrix}\frac{1}{r\cdot tan(\frac{\alpha}{2})}&0&0&0\\0&\frac{1}{tan(\frac{\alpha}{2})}&0&0\\0&0&\frac{f}{f-n}&1\\0&0&\frac{-nf}{f-n}&0\end{bmatrix}$$

위의 projectioni matrix 를 곱한 후, 하지만 원근 나누기는 이전 인 때의 기하 구조를 가리켜서 동차 절단 공간(homogeneous clip space) 또는 projection space 라고 한다. 
또한 원근 나누기 까지 끝나친 후의 기하구조를 가리켜 NDC space 에 있다고 말한다.

# XMMatrixPerspectiveFovLH 함수

XNA Math 라이브러리에서 perspective projection matrix 를 구축하는 다음의 함수를 제공한다.
```c++
XMMATRIX XMMatrixPerspectiveFovLH( // 투영 행렬 반환
	FLOAT FovAngleY,     // 수직 시야각(라디안단위)
	FLOAT AspectRatio,   // 종횡비
	FLOAT NearZ,         // near plane 까지의 거리
	FLOAT FarZ           // far plane 까지의 거리
)
```

## 사용 예시

```c++
XMMATRIX P = XMMatrixPerspectiveFovLH(
	0.25f*MathX::Pi,
	AspectRatio(),
	1.0f,
	1000.0f
);
```

위의 코드는 수직 시야각을 45도로 설정하고 near plane 은 z = 1 에 far plane 은 z = 1000에 둔다. (view space 기준)

이때 aspect ratio 계산은 다음과 같다.
```c++
float D3DApp::AspectRatio()const
{
	return static_cast<float>(mClientWidth)/(mClientHeight);
}
```
