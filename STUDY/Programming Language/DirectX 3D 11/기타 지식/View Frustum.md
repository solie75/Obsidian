view frustum 은 모델이 camera space 에서 projection space 로 투영되는 방식을 정의하는 3D volume이다. 객체는 보여지기 위해서 이 3D volume 내에 위치해야 한다.

XNA Framaework 는 projection matrix 를 사용하여 view frustum 내의 정점 위치를 계산한다. 그 matrix는 y좌표를 반전하여 왼쪽 상단 모서리에 screen oirigin (화면 반전)을 reflect(반영)한다. matrix 가 적용된 후, 동차(homogenous) 정점들(즉, 정점에 (x,y,z,w) 좌표가 포함됨)은 비동차 좌표로 변환되어 레스터화 할 수 있다.

가장 흔한 유형의 투영(projection)은 perspective projection (원근법 투영)으로 하여 카메라 근처의 객체가 멀리 있는 객체보다 더 크게 보이게 만들어 준다. 원근법 투영의 경우, view frustum이 다음 그림에 표시된 것처럼 위쪽과 아래쪽이 근거리 혹은 원거리 클리핑 평면에 의해 피라미드 형태로 시각화 될 수 있다. 
![[Pasted image 20230122042330.png]]

view frustum 은 field of view (fov), z 좌표로 특정 되는 앞과 뒤 클립핑 평면 사이의 거리 로 정의된다. CreatePerspectiveFiledOfView 메서드를 사용하여 perspective fov 를 설정한다.  