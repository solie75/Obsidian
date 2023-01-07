Direct3D 10 이후에서는 [**D3D_PRIMITIVE_TOPOLOGY**](https://learn.microsoft.com/en-us/windows/desktop/api/D3DCommon/ne-d3dcommon-d3d_primitive_topology) enumerated type 에 나타내져 있는 몇개의 primitive type ( or topologies) 를 지원한다. <span style="color: yellow">이러한 type 들은 정점(vertex)가 파이프라인에서 어떻게 해석되고 렌더링 되는지를 정의한다.</span>

정점들을 담고있는 정점 버퍼는 단순 일단의 정점들을 메모리에 저장하는 자료구조일 뿐이다. 정점 버퍼 자체는 그 정점들을 어떤 식으로 조합하여 primitive 를 형성할 것인지를 말해 주지 않는다. 
-> primitive 를 형성하는 방식을 Direct3D 에게 알려 주는 데 쓰이는 수단이 바로 Primitive topology 이다.

## Basic Primitive Types

다음의 기본 primitive type 들이 제공된다.

1. Point List
2. Line List
3. Line Strip
4. Triangle List
5. Triange Strip

각각의 primitive type 들은 [[c. Primitive Topology#^d604ab]]에 시각화 되어 있다.

input-assembler stage는 vertex buffer 와 index buffer 로부터 데이터를 읽는다.  이러한 primitive에 그 data 를 assemble 한다. 그 다음 그 데이터를 remaining pipeline stage 에 보낸다. ( input-assembler stage 에 대한 primitive type을 특정하는 [**ID3D11DeviceContext::IASetPrimitiveTopology**](https://learn.microsoft.com/en-us/windows/desktop/api/D3D11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology) 메서드를 사용할 수 있다.)

## Primitive Adjacency

^5e13f8

Direct3D 10 이후 버전에서 point list를 제외한 primitive type 에는 두 가지 버전이 있다. 하나는 인접성을 가진 primitive type 이고 다른 하나는 인접성이 없는 primitive type 이다. 인접성을 가진 Primitive 는 surrounding vertices (주위 정점)를 포함한다. 반면에 인접성 없는 primitive는 오로지 target (해당) primitive의 정점만을 포함한다. 예를 들어, line list primitive는 입접성을 포함하는 line list primitive 에  해당한다.

Adjacent primitive 는 geometry에 대한 더 많은 정보를 제공할 의도되었다. 또한 Adjacent Primitive 는 오직 geometry shader를 통해서 볼 수 있다. Adjacency(인접성)은 silhouette detection, shadow volume extrusion 등 으로 사용되는 geometry shader 들에 대해 유용하다.

예를 들어 인접성을 가진 triangle 리스트를 그리길 원한다고 가정해보자. 36개의 정점( 인접성을 가진)을 포함한 하나의 triangle list는 6개의 완성된 primitice들을 생상할 것이다.

(line strip 를 제외한) 인접성을 가진 Primitive는 인접성을 갖지 않고 나머지는 동등한(같은)  Primitive 의 딱 두배 많은 정점을 포함하며 이때의 각각의 추가적인 정점이 바로 adjacnet vertex 이다.

## Winding Direction and Leading Vertex Positions

^d604ab
다음의 일러스트에서 보여주는 것처럼, leading vertex 는 primitive 내의 첫 번째 non-adjacent vertex 이다. primitive type 은 각각이 다른 primitive 에 대해서 쓰이는 한 여러개의 정의된 leading vertex 들을 가질 수 있다. 인접성을 가진 triangle strip 에 대해 leading vertices 는 0, 2, 4, 6 등이다. 인접성을 가진 Line strip 에 대해서, leading vertice 는 1, 2, 3, 등이다. 반면에 하나의 adjacent primitive는 leading vertex 가 없다. 

다음의 그림은 input assembler가 생성할 수 있는 모든 primitive type에 대한 vertex ordering을 보여준다. 


![[Pasted image 20230103135134.png]]


![[Pasted image 20230104111605.png]]

1. vertex : 3D 공간에서의 하나의 점
2. Winding Direction : 하나의 primitive 를 assembling 할 때의 vertex order(순서). 시계방향이나 반시계방향이 될 수 있다. ( [[6. Rasterizer Stage#^845379]] : 후면 선별(방향 설정 기준))
3. Leading vertex : per-constant data를 포함한 primitive 안에 첫 번째 non-adjacent vertex.

## Generating Multiple Strips

strip cutting을 통해서 multipe strips 를 만들어 낼 수 있다. 간단명료하게 RestartStrip HLSL function을 호출하거나, 또는 index buffer 안에 특정한 index value 를 삽입하여 strip cut 을 수행할 수 있다. 그 특정한 값은  -1 로,  32-bit 인덱스에서는 즉 0xffffffff이고 16-bit 인덱스에서는 0xffff이다. -1 인 인덱스는 현재 strip 의 명확한 'cut' 또는 'restart'를 가리킨다. 이전의 index 은 이전의 primitive 혹은 strip을 완료한다. 그리고 다음 index 는 새로운 primitive 와 strip 을 시작한다.  generating multiple strip의 더 많은 정보는 [Geometry-Shader Stage](https://learn.microsoft.com/en-us/previous-versions//bb205146(v=vs.85)). 참고

## Primitice with Adjacency

인접성(adjacency) 정보를 가진 삼각형 목록에서는 각 삼각형 마다 **인접 삼각형** 이라고 부르는 이웃한 삼각형 세 개를 포함 시킬 수 있다. 인접 삼각형은 geometry shader 에서 인접 삼각형들에 접근해야하는 특정한 geometry shader algorithm을 구형할 때 쓰인다. geometry shader 가 그런 triangle with adjacency에 접근하기 위해서는 인접한 삼각형들의 정보도 vertex buffe와 index buffer 에 담아서 pipeline에 제출해야 하고, 반드시 topology 를 D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST 로 지정해야 한다. 그래야 pipeline이 vertex buffer로부터 삼각형과 그 인접 삼각형들을 구축 할 수 있다. 

인접 기본 도형의 정점은 오직 geometry shader의 입력으로만 쓰일 뿐, 실제로 그려지지 않는다. geometry shader가 아예 존재하지 않는 경우에도 primitice with adjacency가 그려지지 않는다.

## Index

^d28d47

삼각형 목록을 이용하여 사각형 한 개, 팔각형 한 개 를 구축한다고 할때 정점 배열이 다음과 같다고 하자
```c++
Vertex quad[6] = {
	v0, v1, v2, // 삼각형 0
	v0, v2, v3  // 삼각형 1
}

Vertex octagon[24] = {
	v0, v1, v2, // 삼각형 0
	v0, v2, v3, // 삼각형 1
	v0, v3, v4, // 삼각형 2
	v0, v4, v5, // 삼각형 3
	v0, v5, v6, // 삼각형 4
	v0, v6, v7, // 삼각형 5
	v0, v7, v8, // 삼각형 6
	v0, v8, v1  // 삼각형 7
}
```
![[Pasted image 20230106190217.png]]

하나의 3차원 물체를 형성하는 삼각형들은 다수의 정점을 공유하게 된다. 
하지만 정점들이 중복되면
1. 메모리 요구량이 증가한다.
2. 그래픽 하드웨어의 처리량이 증가한다.
때문에 index(색인)을 사용하여 중복의 단점을 없앤다. 즉, 고유한 정점들로 정점 목록을 만들어 두고, 어떤 정점들을 어떤 순서로 사용해서 삼각형을 형성하는 지는 그 정점 들의 색인들을 적절히 나열함으러써 지정한다. 
색일을 적용한다면 사각형을 형성하기 위한 vertex list 와 index list는 다음과 같이 된다.
```c++
	Vertex quadV[4] = {v0, v1, v2, v3};
	UINT quadIndexList[6] = {
		0, 1, 2, // 삼각형 0
		0, 2, 3  // 삼각형 1
	}

	Vertex octagonV[9] = [v0, v1, v2, v3, v4, v5, v6, v7, v8]
	UINT octagonIndexList[24] = {
		0, 1, 2, // 삼각형 0
		0, 2, 3, // 삼각형 1
		0, 3, 4, // 삼각형 2
		0, 4, 5, // 삼각형 3
		0, 5, 6, // 삼각형 4
		0, 6, 7, // 삼각형 5
		0, 7, 8, // 삼각형 6
		0, 8, 1  // 삼각형 7
	}
```

그래픽 카드는 정점 목록의 고유한 정점들을 처리한 후, Index List를 이용하여 정점들을 조합해 삼각형을 형성한다. 정점들에서 '중복'이 없어진 대신 색인들에 중복이 생기지만 다음의 두 가지 이유로 문제되지 않음을 알 수 있다.
1. 색인은 그냥 정수이르모 완전한 정점 구조체보다 적은 양의 메모리를 차지한다.
2. 정점 캐시 순서가 좋은 경우 그래픽 하드웨어는 중복된 정점들을 (너무 자주)처리하지 않아도 된다.
