<span style="color: yellow">transform engine</span> 은 고정함수인 geometry pipeline 을 거쳐 다음의 작용을 한다.  transform engine 은 model 과 viewer 를 <span style="color: yellow">world space</span> 에 위치키고, 스크린에 display할 정점을 투영하며, viewport에 정점들을 clip 한다. <span style="color: yellow">transform engine 은 또한 각 정점에 대하여 빛 확산 과 반사 컴포넌트를 결정하는 조명연산을 수행한다.</span>

geometry pipeline 은 입력으로 정점을 취한다(take). transform engine 은 해당 정점에 대해서 world, view projection transform, 결과물에 대한 clip 을 적용시키고 모든 것들을 rasterizer 에 전달한다.

pipeline 의 앞부분에서 하나의 모델에 속한 정점들은 <span style="color: yellow">local 좌표 시스템</span>에 의해 선언된다. 이것은 local origin 이며  orientation 이다. 이 좌표의 orientation 을  <span style="color: yellow">model space</span> 라고 하며, 개별적인 좌표들을 model coordinate 라고 불린다.

geometry pipeline 의 첫번째 단계(stage) 는 local 좌표 시스템의 model 의 정점을 하나의 장면에 모든 오브젝트들이 모여있는 좌표 시스템으로 transform 한다. 정점을 reorienting 하는 과정을 <span style="color: yellow">world transform </span>이라고 부른다. 이 새로운 orientation 은 흔히 <span style="color: yellow">world space </span> 로 불리고 world space 내의 정점들은 world 좌표계로 선언된다.

다음의 단계에서 사용자가 의도하는 3D 세상을 설명하는 정점들은 카메라의 측면에서 oriented 된다. 즉, 응용프로그램은 장면으로 내보낼 시점(view)의 한 지점(point)를 선택해야 하고 world space 좌표계는 재위치(replaced)됨과 카메라의 시점 주위를 회전(rotated around the cameta's view)해야 하며 world space 를 <span style="color: yellow">camera space </span> 로 전환해야 한다.

다음 단계는 <span style="color: yellow">projection transform </span> 이다. pipeline 의 이 부분에서 오브젝트는 시점과 가까운 오브젝트들은 거기가 먼 오브젝트들보다 크게 나타나는 등, 보통 장면의 깊이에 대한 시적 착각(illusion)를 주기 위하여 viewer로부터의 거리를 고려하여 크기가 정해(scaled)진다. 간단히 말해 현재 문서는 projection transform (투영 변환) 이후에 존재하는 정점들의 공간을 <span style="color: yellow">projection space </span> 라고 한다. 어떤 그래픽 서적은 projection space 를 post-perspective homogeneous space (원근 후 근질 공간) 라고 하기도 한다. 모든 projection transform 이 장면에 속하는 오브젝트의 크기를 조절하는 것은아니다. 하나의 projection 은 때때로 <span style="color: yellow">afine </span> 또는 <span style="color: yellow">orthogonal projection</span> 이라고 불린다.

pipeline 의 마지막 부분에서는 화면상에 시각화 되지 않은 정점들이 삭제 된다. 따라서 rasterizer 는 절대 보이지 않을 정점에 대하여 색상이나 shading 할 연산에 대해 시간을 잡아먹지 않는다. 이 과정은 <span style="color: yellow">clipping</span> 이라고 불린다. clipping 이후에 남아있는 정점들은 viewport parameter에 따라서 크기가 조절되고 screen 좌표계로 전환된다. 장면이 rasterized 된 때 화면(Screen) 상으로 보이는 결과로 나온 정점들은 <span style="color: yellow">Screen space</span>에 존재한다.

# Matrix Transforms

3D 그래픽을 작업하는 응용프로그램에서 사용자는 다음의 과정을 ㅓ따라 geometrical transform를 사용할 수 있다.
- 다른 오브젝트와의 연관하여 오브젝느의 위치를 표현
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
위의 등식에서 C는 생성되는 composite matrix 이고 $M_1$ 
# World Transform
Local Space 에서 정의된 오브젝트를 World Space 에 사용자가 의도한 위치와 방향으로 배치해야 한다. 구체적으로, World Space 좌표계를 기준으로 한 Local Space 좌표계의 원점과 축들의 위치 및 방향을 지정하고, 그에 해당하는 좌표 변환을 수행해야 한다.

이때 local space 기준의 좌표를 world space 기준의 좌표로 변경하는 과정을 World Transform 이라 하고 해당 변환 행렬을 World Matrix 라 부른다. 이에 모든 오브젝트는 각자의 World Matrix 를 갖는다.



https://learn.microsoft.com/en-us/windows/win32/direct3d9/transforms


# Camera Transform

# Projection Transform



### reference

- orientation 이란
https://gofo-coding.tistory.com/entry/Orientation-Rotation
https://learn.microsoft.com/en-us/windows/win32/wmp/orientation