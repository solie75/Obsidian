
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

Near Plane 과 Far Plane 사이의 점 $(x, y, z)$를 투영한 투영평면 상의 점 $(x', y', z')$ 을 구하기 위해서는 $(x, y, z)$에 행렬 연산을 해주어야 한다.

