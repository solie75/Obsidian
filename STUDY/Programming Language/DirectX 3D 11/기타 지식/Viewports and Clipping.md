개념적으로 viewport 는 3D scene이 투영된 2 차원 사각형이다. Direct3D 에서 이 사각형은 시스템이 render target 으로 사용하는 Direct3D 표면 내의 좌표로 존재한다. projection 변환은 정점들을 viewport 에서 사용되는 좌표계로 변환한다. viewport 는 또한 장면이 렌더링 될 render target 표면의 depth value 의 범위를 특정하는데 사용된다. (일반적으로 0.0 ~ 1.0) 
# Viewing Frustum

view frustum 은 viewport 의 카메라를 기준으로 배치된 scene 내의 3D volume 이다. volume 의 shape는 model 이 camera space 로부터 화면에 투영되는 방식에 영향을 미친다. 가장 흔한 투영 유형인 perspective projection 은 가까이 있는 오브젝트가 멀리 있는 오브젝트보다 크게 보이게 한다. perspective viewing 에 대해서 viewing frustum 은 다음의 그림과 같디 피라미드의 형태로 보일 수 있다. 
![[Pasted image 20230122042330.png]]
위의 그림에서 front Cliping plane 과 back clipping plane 사이의 volume 이 viewing frustum 이다. 오브젝트들은 이 영역 안에 있어야 시각화 된다.

view frustum 은 다음의 그림과 같이 field of view (fov)과  z 좌표로 특정 되는 앞과 뒤 클립핑 평면 사이의 거리 로 정의된다. 
![[Pasted image 20230306231930.png]]
위의 그림에서 D 는 카메라와 geometry pipeline 의 마지막 부분에 정의되는 공간의 원점과의 거리이다. 이 D 값은 [[STUDY/Programming Language/DirectX 3D 11/기타 지식/Transform#Projection Transform|projection matrix]]를 생성하는데 사용된다.

CreatePerspectiveFiledOfView 메서드를 사용하여 perspective fov 를 설정한다. 

# Viewport Rectangle

viewport rectangle 은 c++ 에서 D3DVIEWPRT9 구조체를 사용하여 정의한다. D3DVIEWPORT9 구조체는 다음의 viewport를 다루는 메서드에 사용된다.
-   [**IDirect3DDevice9::GetViewport**](https://learn.microsoft.com/en-us/windows/win32/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-getviewport)
-   [**IDirect3DDevice9::SetViewport**](https://learn.microsoft.com/en-us/windows/win32/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-setviewport)

D3DVIEWPORT9 structure는 render target surface의 영역을 정의하는 X, Y, Width, Height 총 4개의 맴버를 포함한다. 이 값들은 다음의 그림과 같이 목표로 하는 rectangle 또는 viewport rectangle 에 해당한다.
![[Pasted image 20230306232909.png]]
X, Y, 너비, 높이 멤버에 대해 지정하는 값은 렌더링 대상 표면의 왼쪽 위 모서리에 상대적인 화면 좌표이다. 이 구조체는 장면이 렌더링될 depth range를 나타내는 두 개의 추가 멤버(MinZ 및 MaxZ)를 정의한다.

Direct3D 는 vieport clipping volume 의 범위를 X, Y 대해 각각 -1.0 ~1.0 으로 추정한다. 사용자는  projection 변환을 사용하여 clipping 하기 전에 viewport 의 가로 세로의 비를 조정할 수 있다.

D3DVIEWPORT9의 맴버들에 사용되는 치수는 render target surface 위의 viewport 의 위치와 치수를 정의한다. 이러한 값들은 surface 의 좌상단 모서리를 기준으로 화면 좌표에 있다.

Direct3D 는 viewport 의 위치와 치수를 사용하여 target surface 상의 적절한 위치에 랜더링될 scene에 맞게  정점을 크기 조정을 한다. 내부적으로 Direct3D 는 이 값들을 각 정점에 적용될 다음의 matrix 에 삽입한다.
![[Pasted image 20230306235332.png]]
위의 matrix 는 viewport 치수 및 원하는 깊이 범위에 따라서 정점을 크기 조정하고 render target surface 의 적절한 위치로 변환한다. matrix는 또한 y가 아래쪽으로 증가하면서 왼쪽 상단 모서리에 화면 원점을 반영하도록 y 좌표를 뒤집는다. 이 matrix 가 적용된 후에도 정점은 여전히 homogeneous 이다. (즉,여전히 [x, y, z, w]정점 으로 존재한다는 뜻). 그리고 rasterizer 에 보내지기 전에 non-homogeneous 좌표계로 변환되어야 한다.

# Clearing a Viewport

Clearing viewport 는 render target surface 상의 viewport rectangle 의 내용을 리셋하는 것이다. 또한 depth, stencil buffer surface 의 rectangle 또한 clear 한다.

IDirect3DDevice9::Clear 을 사용하여 viewport 를 clear 한다. Count 매개변수 를 1로 설정하고 pRects 매개변수를 전체 viewport 영역을 포함하는 단일 rectangle 의 주소로 설정하면 전체 viewport 가 clear 된다. 모든 viewport 를 clear 하는 다른 방법은 pRects 매개변수를 NULL 로 설정하고 Count 매개변수를 0으로 설정하는 것이다.

The [**IDirect3DDevice9::Clear**](https://learn.microsoft.com/en-us/windows/win32/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-clear) can be used for clearing stencil bits within a depth buffer. Simply set the Flags parameter to determine how **IDirect3DDevice9::Clear** works with the render target and any associated depth or stencil buffers. The D3DCLEAR_TARGET flag will clear the viewport using an arbitrary RGBA color that you provide in the Color argument (this is not the material color). The D3DCLEAR_ZBUFFER flag will clear the depth buffer to an arbitrary depth you specify in Z: 0.0 is the closest distance, and 1.0 is the farthest. The D3DCLEAR_STENCIL flag will reset the stencil bits to the value you provide in the Stencil argument. You can use integers that range from 0 to 2n-1, where n is the stencil buffer bit depth.

In some situations, you might be rendering only to small portions of the render target and depth buffer surfaces. The clear methods also enable you to clear multiple areas of your surfaces in a single call. Do this by setting the Count parameter to the number of rectangles you want cleared, and specify the address of the first rectangle in an array of rectangles in the pRects parameter.

# Reference

https://learn.microsoft.com/en-us/windows/win32/direct3d9/viewports-and-clipping