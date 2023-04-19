<span style="color: yellow">transform engine</span> 은 고정함수인 geometry pipeline 을 거쳐 다음의 작용을 한다.  transform engine 은 model 과 viewer 를 <span style="color: yellow">world space</span> 에 위치시키고, 스크린에 display할 정점을 투영하며, viewport에 정점들을 clip 한다. <span style="color: yellow">transform engine 은 또한 각 정점에 대하여 빛 확산 과 반사 컴포넌트를 결정하는 조명연산을 수행한다.</span>

geometry pipeline 은 입력으로 정점을 취한다(take). transform engine 은 해당 정점에 대해서 world, view projection transform, 결과물에 대한 clip 을 적용시키고 모든 것들을 rasterizer 에 전달한다.

pipeline 의 앞부분에서 하나의 모델에 속한 정점들은 <span style="color: yellow">local 좌표 시스템</span>에 의해 선언된다. 이것은 local origin 이며  orientation 이다. 이 좌표의 orientation 을  <span style="color: yellow">model space</span> 라고 하며, 개별적인 좌표들을 model coordinate 라고 불린다.

geometry pipeline 의 첫번째 단계(stage) 는 local 좌표 시스템의 model 의 정점을 하나의 장면에 모든 오브젝트들이 모여있는 좌표 시스템으로 transform 한다. 정점을 reorienting 하는 과정을 <span style="color: yellow">world transform </span>이라고 부른다. 이 새로운 orientation 은 흔히 <span style="color: yellow">world space </span> 로 불리고 world space 내의 정점들은 world 좌표계로 선언된다.

다음의 단계에서 사용자가 의도하는 3D 세상을 설명하는 정점들은 카메라의 측면에서 oriented 된다. 즉, 응용프로그램은 장면으로 내보낼 시점(view)의 한 지점(point)를 선택해야 하고 world space 좌표계는 재위치(replaced)됨과 카메라의 시점 주위를 회전(rotated around the cameta's view)해야 하며 world space 를 <span style="color: yellow">camera space </span> 로 전환해야 한다.

다음 단계는 <span style="color: yellow">projection transform </span> 이다. pipeline 의 이 부분에서 오브젝트는 시점과 가까운 오브젝트들은 거기가 먼 오브젝트들보다 크게 나타나는 등, 보통 장면의 깊이에 대한 시적 착각(illusion)를 주기 위하여 viewer로부터의 거리를 고려하여 크기가 정해(scaled)진다. 간단히 말해 현재 문서는 projection transform (투영 변환) 이후에 존재하는 정점들의 공간을 <span style="color: yellow">projection space </span> 라고 한다. 어떤 그래픽 서적은 projection space 를 post-perspective homogeneous space (원근 후 근질 공간) 라고 하기도 한다. 모든 projection transform 이 장면에 속하는 오브젝트의 크기를 조절하는 것은아니다. 하나의 projection 은 때때로 <span style="color: yellow">afine </span> 또는 <span style="color: yellow">orthogonal projection</span> 이라고 불린다.

pipeline 의 마지막 부분에서는 화면상에 시각화 되지 않은 정점들이 삭제 된다. 따라서 rasterizer 는 절대 보이지 않을 정점에 대하여 색상이나 shading 할 연산에 대해 시간을 잡아먹지 않는다. 이 과정은 <span style="color: yellow">clipping</span> 이라고 불린다. clipping 이후에 남아있는 정점들은 viewport parameter에 따라서 크기가 조절되고 screen 좌표계로 전환된다. 장면이 rasterized 된 때 화면(Screen) 상으로 보이는 결과로 나온 정점들은 <span style="color: yellow">Screen space</span>에 존재한다.

# Matrix Transforms

3D 그래픽을 작업하는 응용프로그램에서 사용자는 다음의 과정을 따라 geometrical transform를 사용할 수 있다.
- 다른 오브젝트와의 연관하여 오브젝트의 위치를 표현
- 오브젝트의 회전과 크기
- 시점에 따른 위치, 방향, 원근법

다금과 같이 4x4 matrix 를 사용하여 임의의 점 $(x, y, z)$ 를 다른 점 $(x',y',z')$ 으로 변환한다.

![[Pasted image 20230304212119.png]]

$(x, y,z)$ 에 대하여 다음의 등식을 수행하고 matrix 는 점 $(x', y', z')$ 을 생성한다. 
$$\begin{align}&x'=(xM_{11})+(yM_{21})+(zM_{31})=(1\cdot M_{41})\\&y'=(xM_{12})+(yM_{22})+(zM_{32})=(1\cdot M_{42})\\&z'=(zM_{13})+(zM_{23})+(zM_{33})=(1\cdot M_{43})\end{align}$$
가장 흔히 쓰이는 transform 들을 translation, rotation, scaling 이 있다. 이러한 효과를 생성하는 행렬을 single matrix로 결합하여 한번에 여러 transform을 계산할 수 있다. 예를 들어 일련의 점들을 translate 하고 ratate 하기 위해서 single matrix 를 만들 수 있다.

matrix들은 row-column 순서 (행-열 순서) 로 작성된다. uniform scaling 으로 불리는 각축을 따라 균등하게 스케일링 되는 matrix 는 수학적 표기법에 따라 다음과 같다. 

$\begin{bmatrix}s&0&0&0\\0&s&0&0\\0&0&s&0\\0&0&0&1\end{bmatrix}$

c++ 에서 Direct3D 는 D3DMATRIX 구조체를 사용하여 2 차원 배열로써 선언한다. 다음의 예는 uniform scaling matrix 로 쓰일 수 있는 D3DMATRIX 구조체를 초기화하는 것을 보여준다.

```c++
// 이 예시에서 s 는 float 자료형의 변수이다.
D3DMATRIX scale = {
	s, 0.0f, 0.0f, 0.0f,
	0.0f, s, 0.0f, 0.0f,
	0.0f, 0.0f, s, 0.0f,
	0.0f, 0.0f, 0.0f, 1,
}
```

## Translate

다음의 등식은 점 $(x, y, z)$ 를 점 $(x', y', z')$ 으로 translate 한다. 

$\begin{bmatrix}x'&y'&z'&1\end{bmatrix}=\begin{bmatrix}x&y&z&1\end{bmatrix}\begin{bmatrix}1&0&0&0\\0&1&0&0\\0&0&1&0\\T_x&T_y&T_z&1\end{bmatrix}$

C++ 에서 사용자는 수동적으로 translation matrix 를 생성할 수 있다. 다음의 예시는 정점을 translate 하기 위한 matrix 를 생성하는 기늘을 위한 source code를 보여준다.

```c++
D3DXMATRIX traslate(const float dx, const float dy, const float dz){

	D3DXMATRIX ret;

	D3DXMatrixIdentity(&ret);
	ret(3,0)=dx;
	ret(3,1)=dy;
	ret(3,2)=dz;
	return ret;
} // End of Translate;
```

## Scale

다음의 등식은 x, y, z 방향의 임의의 값으로 점 $(x, y, z)$ 를 새 점 $(x',y',z')$ 로 확장한다.

$\begin{bmatrix}x'&y'&z'&1\end{bmatrix}=\begin{bmatrix}x&y&z&1\end{bmatrix}\begin{bmatrix}S_x&0&0&0\\0&S_y&0&0\\0&0&S_z&0\\0&0&0&1\end{bmatrix}$

### Rotate

지금 부터 설명되는 것들은 [왼손 좌표계](https://hellowoori.tistory.com/38) 를따른다. 

다음의 등식은 x 축을 기준으로 회전 시켜 새 점 $(x', y', z')$ 을 만든다.

$\begin{bmatrix}x'&y'&z'&1\end{bmatrix}=\begin{bmatrix}x&y&z&1\end{bmatrix}\begin{bmatrix}1&0&0&0\\0&cos\theta&sin\theta&0\\0&-sin\theta&cos\theta&0\\0&0&0&1\end{bmatrix}$

다음의 등식은 y 축을 기준으로 회전시킨다.

$\begin{bmatrix}x'&y'&z'&1\end{bmatrix}=\begin{bmatrix}x&y&z&1\end{bmatrix}\begin{bmatrix}cos\theta&0&-sin\theta&0\\0&1&0&0\\ sin\theta&0&cos\theta&0\\0&0&0&1\end{bmatrix}$

다음의 등식은 z 축을 기준으로 회전시킨다.

$\begin{bmatrix}x'&y'&z'&1\end{bmatrix}=\begin{bmatrix}x&y&z&1\end{bmatrix}\begin{bmatrix}cos\theta&sin\theta&0&0\\-sin\theta&cos\theta&0&0\\0&0&1&0\\0&0&0&1\end{bmatrix}$

이러한 예시 matrix 들에서 $\theta$ 는 회전 각도를 나타낸다. 회전축을 따라 원점을 바라볼 때 각도는 시계 방향으로 측정된다.
![[Pasted image 20230304224806.png]]

C++ 응용프로그램에서는 [**D3DXMatrixRotationX**](https://learn.microsoft.com/en-us/windows/win32/direct3d9/d3dxmatrixrotationx), [**D3DXMatrixRotationY**](https://learn.microsoft.com/en-us/windows/win32/direct3d9/d3dxmatrixrotationy), 그리고 [**D3DXMatrixRotationZ**](https://learn.microsoft.com/en-us/windows/win32/direct3d9/d3dxmatrixrotationz) 함수를 사용하여 rotation matrix 들을 생성한다. ekdmadml 코드는 D3DXMatrixRotationX 함수이다.
```c++
D3DXMATRIX* WINAPI D3DXMatrixRotationX
    ( D3DXMATRIX *pOut, float angle )
{
#if DBG
    if(!pOut)
        return NULL;
#endif

    float sin, cos;
    sincosf(angle, &sin, &cos);  // Determine sin and cos of angle

    pOut->_11 = 1.0f; pOut->_12 =  0.0f;   pOut->_13 = 0.0f; pOut->_14 = 0.0f;
    pOut->_21 = 0.0f; pOut->_22 =  cos;    pOut->_23 = sin;  pOut->_24 = 0.0f;
    pOut->_31 = 0.0f; pOut->_32 = -sin;    pOut->_33 = cos;  pOut->_34 = 0.0f;
    pOut->_41 = 0.0f; pOut->_42 =  0.0f;   pOut->_43 = 0.0f; pOut->_44 = 1.0f;

    return pOut;
}
```

## Concatenating Matrix

행렬의 장점은 두개 이상의 matrix 들을 곱함으로써 효과를 결합할 수 있다. 이것은 model 을 rotate 한 뒤 다른 위치로 translate 하는데에 두개의 matrix 가 필요하지 않음을 의미한다. 대신에 그 효과를 모두 포함하기 위해서 composite matirx 를 생성하기 위해서 rotation 과 translation matrix 를 곱해야 한다. 이 과정은 matrix concatenation 으로 불리며 다음의 등식과 같이 작성할 수 있다.
$$C=M_1\cdot M_2\cdot M_{n-1}\cdot M_n$$
위의 등식에서 C는 생성되는 composite matrix 이고 $M_1$ 부터 $M_n$ 은 모두 개별적인 matrix 이다. 대부분의 경우, 두개에서 세개의 matrix 가 concatenated 된다. 그러나 여기에 한계가 있는 것은 아니다.

D3DXMatrixMultiply 함수를 사용하여 matrix multiplication 을 수행한다.

matrix multiplication 에서 순서대로 수행되는 것은 매우 중요하다. 좌에서 우로 의 matrix mulriplication 법칙이다. 즉, compowite matrix 를 생성하는데 사용되는 matrix 들의 시각적 효과는 좌 -> 우 순서로 발생한다. 전형적인 world matrix 는 다음의 예시와 같다. 
전형적인 비행 접시를 위해서 world matrix 를 생성했다고 생상해 보자. 사용자는 아마도 그 비행 접시가 model space 의 y 축을 기준으로 회전하고 장면상의 어느 지점에 translate 되기를 원할 것이다. 이러한 효과를 내기 위해서, 다음의 등식과 같이 먼저 rotation matrix 를 생성하고 그 다음 translation matrix 를 multiply 해주어야 한다. $$W=R_y\cdot T_w$$
위의 식에서 $R_y$ 는 y축을 기준으로 회전하는 rotation matrix 이고 $T_w$ 는 world 좌표계의 어느 지점으로 translate 한다.

matrix 들을 multiply 하는 순서는 중요하다. 두 개의 스칼라 값을 곱하는 것과 달리 matrix multiplication 은 교환법칙이 성립하지 않기 때문이다. 반대의 순서로 matrix multiplication 을 실시하면 비행 접시를 world space 에 tranlate 하고 세계 원점(origin) 을 중심으로 회전시키는 시작적 효과가 있을 것이다.
# World Transform
Local Space 에서 정의된 오브젝트를 World Space 에 사용자가 의도한 위치와 방향으로 배치해야 한다. 구체적으로, World Space 좌표계를 기준으로 한 Local Space 좌표계의 원점과 축들의 위치 및 방향을 지정하고, 그에 해당하는 좌표 변환을 수행해야 한다.

이때 local space 기준의 좌표를 world space 기준의 좌표로 변경하는 과정을 World Transform 이라 하고 해당 변환 행렬을 World Matrix 라 부른다. 이에 모든 오브젝트는 각자의 World Matrix 를 갖는다.

world transform 은 model 의 local 원점에 관련되어 정의된 정점인 model space로부터 보통 모든 오브젝트들이 존재하는 장면의 원점에 관련되어 정의된 정점들인 World space 로 좌표계를 전환한다. 본질적으로 world transform 은 model 을 world 에 위치시킨다.

world transfor 은 어떤 translation, rotation, scaling 에 대한 결합이라도 모두 포함 할 수 있다.

### Setting Up a World Matrix

다른 transform 과 마찬가지로 일련의 matrix 를 효과들을 합친 단일 matrix에 concatenating 함으로써 world transform 을 생성한다. 가장 간단한 예를 들면, model 이 world space 의 원점에 있고 local 좌표계의 축이 World space과 같아 질 때, world matrix 는 identity matrix (단위 행렬)이다. world matrix 는 world space 로의 translation 과 필요에 따라 모델을 회전하기 위한 하나 이상의 rotation 의 결합이다.


```c++
/*
8/
* 이 예제를 위해, 다음 변수들은 유효하며 초기화된 것으로 가정한다
* 월드 좌표 내 모델의 위치를 포함하는 변수 : m_xPos, m_yPos, m_zPos
* 모델의 방향을 라디안 단위의 pitch, yaw, roll 각으로 표현하는 변수 :
m_fPitch, m_fYaw, m_fRoll
*/

void C3DModel::MakeWorldMatrix(D3DXMATRIX*  pMatWorld)
{
    D3DXMATRIX  MatTemp; // 회전을 위한 임시 행렬
    D3DXMATRIX  MatRot;  // pMatWorld에 적용되는 최종 회전 행렬

    // 개체의 월드 위치에 대한 이동을 회전보다 먼저 적용하도록,
    // 왼쪽 -> 오른쪽 순으로 행렬을 결합한다
    D3DXMatrixTranslation(pMatWorld, m_xPos, m_yPos, m_zPos);
    D3DXMatrixIdentity(&MatRot);
    // 이제, 월드 행렬에 방향 변수를 적용시킨다
    
    if(m_fPitch || m_fYaw || m_fRoll){
        // 회전 행렬들을 만들고 결합시킨다
        D3DXMatrixRotationX(&MatTemp, m_fPitch);    // Pitch
        D3DXMatrixMultiply(&MatRot, &MatRot, &MatTemp);
        D3DXMatrixRotationY(&MatTemp, m_fYaw);    // Yaw
        D3DXMatrixMultiply(&MatRot, &MatRot, &MatTemp);
        D3DXMatrixRotationZ(&MatTemp, m_fRoll);      // Roll
        D3DXMatrixMultiply(&MatRot, &MatRot, m_fRoll);
        // 월드 행렬을 완성시키기 위해 회전 행렬을 적용시킨다
        D3DXMatrixMultiply(pMatWorld, &MatRot, pMatWorld);
    }

}
```

# View(Camera) Transform

정점들을  camera space 로 변환 시키면서, viewer 를 world space 에 위치시킨다. camera space 에서 camera 또는 viewer 는 +z 방향을 바라보며 원점에 위치한다.  Direct3D 는 왼손 좌표계를 사용하므로 z 는 scene 안쪽으로 + 이다. view matrix 는 world 의 오브젝트들을 카메라의 위치와 원점 주위로 재배치한다.

view matrix 를 생성하는 방법은 많다. 그 모든 경우에서 camera 는 world space 에서 논리적인 위치와 방향을 가지며, scene의 모델들에게 적용시킬 view matrix 를 생성하는 시작점으로 사용된다. view matrix 는 object 를 카메라 자체가 원점이 되는 camera space 에 위치시키기 위해서 translate 하고 rotate 한다. view matrix 를 생성하는 한가지 방법은 각 축에 대한 translation 및 rotation matrix 를 결합하는 것이다. 이러한 방법으로는 다음의 등식이 적용된다.
$$V=T\cdot R_Z\cdot R_y\cdot R_x$$
위의 공식에서, V 는 view matrix, T 는 translation matrix, 그리고 $R_Z, R_y,R_x$ 는 각각 z, y, x 축에 대한 rotation 행렬이다. translation 및 rotation matrix 는 camera 의 world space 내의 논리적인 위치와 방향을 기준으로 한다. 따라서, camera 의 world 내의 논리적인 위치가 <10,20,100> 인 경우, translation matrix 은 객체를 x 축으로  -10, y  축으로 -20, z 축으로 -100 만큼 이동시킨다. 위 공식에서 rotation matrix 들은 camera space 의 축들이 world space 에 대해 회전한 양으로 환산한 카메라의 방향을 기준으로 한다. 예를 들어 위에서 언급한 camera가 바로 아래를 가리킨다면, 그것의 z 축은 아래의 그림과 같이 world space의 z 축에 대해 $\frac{\pi}{2}\theta$ 회전한다.
![[Pasted image 20230306203011.png]]

rotation matrix 들은 scene 의 model 에 크기는 같지만 방향은 반대인 회전을 적용시킨다. dnldml camera 에 대한 view matrix 는 x 축 기준 -90도 회전을 포함하고 있다. rotation matrix 는 translatio matrix 와 결합하여 scene 속 오브젝트의 위치와 원점을 조정하는 view matrix 를 생성하다. 따라서 카메라가 오브젝트의 위쪽에 있는 모양으로 오브젝트들의 위쪽은 카메라를 마주본다.

### Setting up a View Matrix

[**D3DXMatrixLookAtLH**](https://learn.microsoft.com/en-us/windows/win32/direct3d9/d3dxmatrixlookatlh) 와 [**D3DXMatrixLookAtRH**](https://learn.microsoft.com/en-us/windows/win32/direct3d9/d3dxmatrixlookatrh) 헬퍼 함수는 카메라의 위치와 보는 점(look-at point) 를 기준으로 view matrix 를 생성한다.
다음의 예시는 왼손 좌표계에 따라 view matrix 를 생성한 것이다.
```c++
D3DXMATRIX out;
D3DXVECTOR3 eye(2,3,3);
D3DXVECTOR3 at(0,0,0);
D3DXVECTOR3 up(0,1,0);
D3DXMatrixLookAtLH(&out, &eye, &at, &up);
```
Direct3D 는 world matrix, view matrix 를 몇몇의 internal data structure를 환경설정하기 위해 사용한다. 새로운 world matrix 와 view matrix 를 설정할 때마다, 시스템은 연관된 internal structure를 재연산한다. 이러한 matrix 를 자주 설정하는 것은 연산적으로 시간을 잡아먹는다. world matrix 와 view matrix 를 world-view matrix 로 concatenate 한 다음 view matrix 를 identity matrix 로 설정하면 요구되는 연산 수 를 최소화 할 수 있다. 

# Projection Transform

viewing frustum 에서 camera와 viewing transform space 의 원점 사이의 거리가 D 라고 할 때, projection matrix 는 다음과 같다.
$\begin{bmatrix}1&0&0&0\\0&1&0&0\\0&0&1&\frac{1}{D}\\0&0&0&1\end{bmatrix}$ 

viewing matrix 는 z 방향으로 -D 만큼 이동하여 카메라를 원점으로 이동한다. translation matrix 는 다음과 같다.
$\begin{bmatrix}1&0&0&0\\0&1&0&0\\0&0&1&0\\0&0&-D&1\end{bmatrix}$ 

translation matrix 에 projection matrix 를 곱하면 (T*P) 다음과 같이 composite projection matrix 가 된다.
$\begin{bmatrix}1&0&0&0\\0&1&0&0\\0&0&1&\frac{1}{D}\\0&0&-D&1\end{bmatrix}$ 

perspectivetransform 은 viewing frustum 을 새로운 좌표 공간으로 변환시킨다. frustum은 cuboid가 되고 원점은 다음의 그림과 같이 scene의 우측 상단에서 중앙으로 이동한다. 
![[Pasted image 20230307012422.png]]

perspective transform 에서 x, y 방향의 범위는 -1 에서 1 까지이다. z 방향의 front plane 은 0, back plane 은 1이다. 
이 행렬은 camera와 가까운 clipping plane 까지의 거리를 기준으로 오브젝트들의 translate 와 크기 조정을 한다. 그러나 이것은 filed of view(fov) 를 고려한 것은 아니기 때문에 그 거리 안의 오브젝트들의 z 값이 거의 같을 수 있어서 깊이 값 비교가 어렵다. 다음의 matrix는 viewport의 가로세로비 를 고려해 정점들을 조정함으로써 이 문제를 해결하며, 이는 perspective projection에 좋은 방법이다.
$\begin{bmatrix}w&0&0&0\\0&h&0&0\\0&0&q&1\\0&0&-QZ_n&0\end{bmatrix}$

위의 행렬에서 $Z_n$은 가까운 clipping plane 의 z 값이다. $fov_w$ 와 $fov_k$ 는 각각 viewport 의 수평 및 수직 fov 를 라디안 단위로 나타낸 것이다. 변수 w, h, q, 는 아래와 같다.

$$\begin{align}&w=cot\left(\frac{fov_w}{2}\right)\\&h=cot\left(\frac{fov_k}{2}\right)\\&Q=\frac{Z_f}{Z_f-Z_n}\end{align}$$
응용프로그램에서 x 와 y scaling 계수를 정의하기 위해서 field of view 각을 사용하는 것은 viewport 의 (camera space 의)수직과 수평 치수를 사용하는 것 같이 편하지 않을 수 있다. 다음의 w와 h에 대한 두 등식은 viewport 의 치수를 사용한다. 그리고 위의 공식과 동등하다.

$$\begin{align}&w=\frac{2\cdot Z_n}{V_w}\\&h=\frac{2\cdot Z_n}{V_h}\end{align}$$
위의 공식에서 $Z_n$ 은 가까운 clipping plane 의 위치를 나타내고, $V_w$와 $V_h$변수는 camera space내 viewport 의 너비와 높이를 나타낸다.

c++ 어플리 케이션에서 이 두 치수는 D3DVIEWPORT9 구조체의 너비와 높이 맴버에 해당한다.
어떤 공식을 사용하던, 카메라와 아주 가까운 z 값은 다름이 거의 없음으로 $Z_n$은 되도록 큰 값을 설정해야 한다. 이것은 16bit z-버퍼를 사용하는 깊이 비교를 다소 복잡하게 한다.

world 및 view transformation과 마찬가지로 projection transform 을 설정하려면 [**IDirect3DDevice9::SetTransform**](https://learn.microsoft.com/en-us/windows/win32/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-settransform) 메서드를 호출해야 한다.

### Setting Up a Projection Matrix

가음 projection matrix 예시 함수는 front 및 back clipping plane 를 설정할 뿐 아니라 수평 및 수직 fov 각을 설정한다. fov 는 pi 라디안 보다 작아야 한다.
```c++
D3DXMATRIX

ProjectionMatrix(const float near_plane, // 앞쪽 클립면까지의 거리
const float far_plane,  // 뒤쪽 클립면까지의 거리
const float fov_horiz,  // 라디안 단위의 수평 fov
const float fov_vert)  // 라디안 단위의 수직 fov
{
    float h, w, Q;
    
    w = (float)1/tan(fov_horiz * 0.5);  // 1/tan(x) == cot(x)
    h = (float)1/tan(fov_vert * 0.5);
    Q = far_plane/(far_plane - near_plane);
    
    D3DXMATRIX ret;
    ZeroMemory(&ret, sizeof(ret));
    
    ret(0, 0) = w;
    ret(1, 1) = h;
    ret(2, 2) = Q;
    ret(3, 2) = -Q * near_plane;
    ret(2, 3) = 1;
    return ret;
}
```

matrix 생성 후 D3DTS_PROJECTION 를 특정하여  [**IDirect3DDevice9::SetTransform**](https://learn.microsoft.com/en-us/windows/win32/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-settransform) 를 설정한다. 

The D3DX utility library provides the following functions to help you set up your projection matrix.

-   [**D3DXMatrixPerspectiveLH**](https://learn.microsoft.com/en-us/windows/win32/direct3d9/d3dxmatrixperspectivelh)
-   [**D3DXMatrixPerspectiveRH**](https://learn.microsoft.com/en-us/windows/win32/direct3d9/d3dxmatrixperspectiverh)
-   [**D3DXMatrixPerspectiveFovLH**](https://learn.microsoft.com/en-us/windows/win32/direct3d9/d3dxmatrixperspectivefovlh)
-   [**D3DXMatrixPerspectiveFovRH**](https://learn.microsoft.com/en-us/windows/win32/direct3d9/d3dxmatrixperspectivefovrh)
-   [**D3DXMatrixPerspectiveOffCenterLH**](https://learn.microsoft.com/en-us/windows/win32/direct3d9/d3dxmatrixperspectiveoffcenterlh)
-   [**D3DXMatrixPerspectiveOffCenterRH**](https://learn.microsoft.com/en-us/windows/win32/direct3d9/d3dxmatrixperspectiveoffcenterrh)

# W-Friendly Projection Matrix

Direct3D 는 depth buffer 혹은 fog effect 의 depth based 계산을 수행하기 위해 world, view, projection matrix를 이용해 변환된 정점의 w-component를 사용할 수 있다. 이런 계산에서 projection matrix 은 w를 world space 의 z 와 동등하도록 정규화 해야한다. 즉, projection matrix가  1이 아닌 (3, 4) 계수를 포함하고 있는 경우, 적절한 matrix 를 만들기 위해서 (3,4)계수의 역을 사용하여 모든 계수의 크기를 조정해야 한다. compliant matrix를 사용자가 제공하지 않는다면, fog 효과, depth buffering 은 제대로 적용되지 않는다.

다음의 그림은 compliant 되지 않는 projection matrix이고 그와 같은 matrix 가 크기조정된 것이다. 즉, eye relative fog 가 불가능하다.
![[Pasted image 20230307022329.png]]

위의 matrix 에서 모든 변수는 0이 아니다. For more information about eye-relative fog, see [Eye-Relative vs. Z-Based Depth](https://learn.microsoft.com/en-us/windows/win32/direct3d9/pixel-fog). For information about w-based depth buffering, see [Depth Buffers (Direct3D 9)](https://learn.microsoft.com/en-us/windows/win32/direct3d9/depth-buffers).

Direct3D 는 w 기반의 깊이 연산에 projection matrix 를 사용한다. 따라서 응용 프로그램은 변환에 Direct3D 를 사용하지 않더라도 원하는 w 기반의 기능을 얻으려면 compliant projection matrix 를 설정해야 한다.
# reference

- orientation 이란 (방향)
https://gofo-coding.tistory.com/entry/Orientation-Rotation
https://learn.microsoft.com/en-us/windows/win32/wmp/orientation