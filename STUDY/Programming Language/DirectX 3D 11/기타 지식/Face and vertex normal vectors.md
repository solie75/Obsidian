## Perpendicular unit normal vector for a front face

mesh의 각 면은 수직인 법선 벡터를 가진다. 그 벡터의 방향은 정점이 정의되는 순서와 좌표 시스템이 우측인이 좌측인지 에 따라서 결정된다. face normal point (면 법선) 은 face 의 front 쪽에서 시작한다. Direct 3D 에서 face 의 front 만 시각화 된다. front face 는 정점이 시계 방향으로 정의된 면이다.  
![[Pasted image 20230119164053.png]]

front face 가 아닌 모든 면은 back face 이다. Direct 3D 는 back face 를 랭더링 하지 않는다. 그러므로 back face는 버려진다(제거한다). 원한다면 back face의 랜더링에 대한 culling mode 를 바꿀 수도 있다. ([Culling State (Direct3D 9)](https://learn.microsoft.com/en-us/windows/win32/direct3d9/culling-state) 참조)

Direct3D 는 Gouraud shading, lighting, texturing effects 에 대하여 vertex unit nomal 을 사용한다. 다음의 그림은 normal 의 예시이다.

![[Pasted image 20230119165113.png]]

Polygon에 Gouraud shading 을 적용하는 경우, Direct3D 는 light source 와 surface 사이의 각도(angle) 을 계산하기 위해서 vertex normal 을 사용한다. 그것은 정점데 대한 색과 빛의 세기의 값을 계산하고, primitive의 모든 surface의 모든 point에 대하여 보간한다. Direact3D 는 각도(angle)을 사용하여 빛의 세기 값을 계산한다. 각도가 클수록, 표면을 비추는 빛이 적어진다. 평평한 객체를 만드는 경우, 다음의 그림과 같이 surface(표면)에 정점에서 시작되는 정점 법선(vertex normal)을 설정한다. 
![[Pasted image 20230119181620.png]]

그러나 그 객체가 triangle strip 으로 구성되어 있고 삼각형이 동일 평면에 있지 않을 가능성이 더 크다. strip 의 모든 삼각형에 걸쳐서 부드러운 shading 을 만드는 한가지 간단한 방법은 정점이 연결된 각 폴리곤의 면(polygonal face)에 대한 표면 법선 벡터 (surface normal vector)를 처음으로 계산하는 것이다. 정점 법선(vecter normal)은 각 표면 법선(surface normal)의 각도(angle)를 동일하게 만들어 설정할 수 있다. 그러나 이 메소드는 복잡한 primitive 에 대해서는 충분히 효율적이지는 못할 수 있다.

이 메소드는 다음의 다이어그램으로 설명된다. 이 다이어그램은 위에서 가장자리를 본 두개의 surface 인 S1 과 S2 를 보여준다. S1 과 S2 의 normal vector 는 파란색, vertex normal vector 는 빨간색이다. vertex normal vector 가 S1의 surface normal 과 이루는 각은 S2의 surface 와 vectex normal 사이의 각과 같다. 이 두 surface 가 조명되고 Gouraud shading으로 쉐이더 처리되면 그 결과는 부드럽게 쉐이딩 되고 그들 사이에 부드럽게 둥근 가장자리가 된다. 

![[Pasted image 20230120125430.png]]

만약 vertex normal 이 그것에 연관된 face 중 하나를 향하여 기울어진 경우, 광원이 만드는 각도에 따라 표면의 한 지점에 대한 빛의 세기가 증가하거나 작아지는 결과를 야기한다. 다음의 다이어 그램은 그 예를 보여준다. 다시 말하지만,  these surfaces are seen edge-on. vertex normal 은 S1 을 향하여 기울어져 있다. surface normal 에 동일한 각도를 가지는 vertex normal 인 경우보다 광원과의 각도가 더 작아진다.
![[Pasted image 20230120130712.png]]
gouraud shading 을 사용하면 3D 상에서 날카로운 모서리를 표현할 수 있다. 즉,다음의 일러스트와 같이 날카로운 모서리(edge)가 필요한 면의 어떤 교차점의 vertex normal vector를 복사하는 것이 요구된다.
![[Pasted image 20230120131141.png]]

만약 장면(scene)을 렌더링하기 위해 DrawPrimitive method 를 사용하는 경우, 날카로운 모서리로 이루어진 객체는 triangle strip 보다는 triangle list로 정의해야 한다. triangle strip 으로 정의할 때, Direct3D 는 그것을 여러개의 triangular face로 구성된 단일 폴라곤으로 대한다. Gouraud shading 은 폴리곤의 각 면(face)와 adjacent faces 사이를 모두 적용된다. 그 결과 면에서 면으로 부드럽게 쉐이딩 처리된 객체가 생성된다. triangle list 는 별개의 삼각형으로 구성된 면(face)의 연속(series)으로 구성된 폴리곤이기 때문에 , Direct3D 는 폴리곤의 각 면에 Gouraud shading 을 적용한다. 그러나, 그것은 face 에서 face 로는 적용되지 않는다. 만약 삼각형 리스트 에서 두 개 이상의 삼각형이 인접한 경우 , 그 삼각형 사이에 날카로운 모서리가 있는 것처럼 보인다.

또 다른 대안은 날카로운 모서리가 있는 객체를 랜더링할 때에  평면 쉐이딩 (flat shading)으로 변경하는 것이다. 이것은 컴퓨터의 연산에 있어 가장 효과정인 방법이지만 Gouraud shading 처리된 객체만큼 사실적으로 렌더링 되지는 않는 장면의 객체가 생성될 수 있다. 
