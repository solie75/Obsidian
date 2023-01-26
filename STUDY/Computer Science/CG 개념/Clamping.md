Clamping 은 하나의 영역에서 위치를 제한하는 과정이다. wrapping 과 달리, clamping 은 그저 포인터를 가장 가까이에 있는 활용가능한 값으로 이동하는 것이다.


To put clamping into perspective, pseudocode (in Python) for clamping is as follows:

```python
def clamp(x: int | float, minimum : int | float, maximum: int | float ) -> int |float : """Limit a position to an area."""
	if x < minimum:
		x = minimum:
	elif x > maximum:
		x = maximum
	return x
```

## Uses

일반적으로, clamping은 주어진 범위에서 값을 제한하는데 사용된다. 예를들어 OpenGL 에서 glClearColor 함수는 범위가 [0,1]로 'clamped' 된 네 개의 GLfloat 값을 갖는다.

컴퓨터 그래픽에서 clamping 이 많은 용도 중에 하나는 폴리곤 내부에 디테일을 배치하는 것이다. 예를 들어, 벽의 총알 구멍 이 그러하다. wraping과 함게 사용하여 다양한 효과를 낼 수도 있다.

