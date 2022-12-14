변위 : <span style="color: yellow">크기와 방향을 같는 양, 즉 벡터로서, 어떤 운동하는 물체의 시작점에서 끝점까지의 위치 변화</span>를 가리킨다. **처음 위치에서 나중 위치를 향하는 방향**을 <span style="color: yellow">변위의 방향</span>으로, **두 점의 직선거리**를 <span style="color: yellow">변위의 크기</span>로 설정한다.

![[Pasted image 20230109031524.png]]
(어떤 물체가 점 A 에서 출발하여 위 그림의 파란색 선을 따라 B 로 이동하였다로 할 때, 점 A 와 B를 직선 방향으로 잇는 벡터를 변위(변위벡터)라고 한다.)

좌표계를 도입하여 처음 위치를 $x_1$, 나중 위치를 $x_2$ 라고하면 변위는
$$ \Delta x = x2 - x1 $$
로 나타낼 수 있다. 여기에서 $x_1$, $x_2$ 가 실수 값이면 변위 $\Delta x$ 도 실숫값을 갖고, $x_1$ 과 $x_2$가 n 차원 공간의 점이면 $\Delta x$ 는 n 차원 공간의 벡터이다.

예시 
![[Pasted image 20230109032643.png]]

좌표평면에서 A(1, -2)를 시작점 B(-5, 6)을 끝점으로 하는 변위벡터의 크기는
$$ \vert \overrightarrow{AB}\vert = \vert (-5 6) - (1, -2)\vert \\ = \vert (-6, 8) \vert \\ = \sqrt{6^2 + 8^2} \\ = 10$$
이고, 벡터 $\overrightarrow{AB}$ 와 같은 방향을 갖는 단위 벡터(크기가 1)는
$$ \frac{\overrightarrow{AB}}{\vert \overrightarrow{AB}\vert} = \frac{(-6, 8)}{10} = \frac{1}{5}(-3,4)$$
이다.


## 변위와 순간속도와의 관계

공간상을 움직이는 물체의 변위가 시각에 대한 미분가능한 함수로 주어졌을 때 순간속도는 변위의 미분으로 주어지고, 역으로 물체의 순간속도가 시각에 대한 함수로 주어졌을 때 변위는 순간속도의 적분으로 얻어진다. 
즉 $X = X(t)$와 $\upsilon=\upsilon(t)$ 를 각각 물체의 시간 t 에서의 변위의 속도라고 할 때, 변위 함수 $X$가 미분가능하면 다음과 같은 관계식이 성립한다.

$$\upsilon(t) = \frac{dX(t)}{dt},\quad X(t) = X(t_0)\ + \  \int^{t_1}_{t_0}v(s)ds$$
