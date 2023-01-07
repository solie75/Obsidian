System-generated value 는 shader 작업에서 의 확실한 효율(성)을 허락하기 위해 IA stage (user-supplied input semantics 에 근거한) 에 의해 생성된다. 

instance id (VS에 보이는), vertex id (VS에 보이는), prinitive id (GS/ PS 에 보이는) 과 같은 data 를 첨부함으로써 subsequent shader stage 는 이 stage 의 과정을 최적화 하기 위해서 이러한 system value 들을 찾을 것이다. 

예를 들어, vertex shader stage 는 쉐이더에 대한 추가적인 per-vertex data 를 잡기(grab)하기 위해서 혹은 다른 작업을 수행하기 위해서 instance id를 찾을 수 있다.  geometry shader stage 와 pixel shader stage 는 같은 방식으로 per-primitive data를 잡기(grab) 위해 primitive id 를 사용할 수 있다. 


## Vertex ID

<span style="color: yellow">Vertex id 는 각 stage 에서 각각의 정점을 식별하는데 사용된다.</span> vertex id 는 default 값은 0인 32-bit unsigned integer 이다. <span style="color: yellow">vertex id 는 primitive 가 IA stage 에서 processing(처리) 될 때 vertex에 배정된다.</span> per-vertex id 를 생성하도록 IA stage 에 정보를 전달하는 shader input declaration 에 vertex id semantic 을 첨부하라.

<span style="color: yellow">Input-Assembler는 shader stage 가 사용할 수 있도록 각 vertex에 vertex id  를 추가할 것이다. 각각의 draw 호출에 대해, vertex id 는 1 씩 증가한다.</span> 인덱싱 된 draw 호출들을 거치고 count 는 처음의 값으로 리셋된다.  [**ID3D11DeviceContext::DrawIndexed**](https://learn.microsoft.com/en-us/windows/desktop/api/D3D11/nf-d3d11-id3d11devicecontext-drawindexed) 와 [**ID3D11DeviceContext::DrawIndexedInstanced**](https://learn.microsoft.com/en-us/windows/desktop/api/D3D11/nf-d3d11-id3d11devicecontext-drawindexedinstanced) 에 대해서  vertex id 는 index value 를 나타낸다. 만약 vertex if 가 2^32 - 1을 초과 하면 그것은 0으로 덮어 씌워 진다.

모든 primitive type 에 대해 vertex 는 (인접성이 없더라고) 자신과 연관(연결)된 vertex의 id 를 갖는다. 


## Primitive ID

primitive id 는 각 primitive 를 식별하기 위해서 각 shader stage 에서 사용된다. primitive id 는 default 값이 0인 32-bit unsinged integer 이다. primitive id 는 primitive 가 IA stage 에서 처리되는 과정에서 배정된다. primitive id 를 생성하는 IA stage에 정보를 제공하기 위해서 shader input declaration 에 primitive-id semantic 을 첨부하라.

IA stage 는 geometry shader 와 pixel shader stage (어느 쪽이든 IA stage 후에 첫 번째로 활성화 되는 것) 에서 사용하기 위해 각 primitive 에 primitive Id 를 추가할 것이다. 각 indexed draw call 에 대하여  primitive id 는 1씩 증가한다. 그러나 primitive id 는 새로운 instance가 시작될 때마다 0으로 리셋된다. 모든 또 다른 draw call 들은 instance id 의 값을 바꾸지 않는다. 만약 instance id 가 2^32 - 1 를 초과하면 0으로 덮어 씌워진다.

pixel shader stage 는 primitive id 에 대해서 별도의 input 을 갖지 않는다. 하지만, primitive id 를 특정하는 모든 pixel shader input 는 constant interpolation mode (<span style="color:green ">상수 보간 모드란..?</span>)를 사용한다. 

adjacent primitive 에 대한 primitive id 를 자동적으로 생성하는 것에 관해서는 지원하지 않는다. triangle strip with adjacency와 같은 인접성을 가진 primitive type에 관해서, primitive id 는 마치 인접성 없는 triangle strip내의 primitive들의 set 와 같이 오직 interior(내부) primitive (non-adjacent primitive) 에 대해서만 유지된다.

## Instance ID

instance ID  는 현재 진행되고 있는 geometry 의 instance를 식별하기 위해 각 shader stage 에서 사용된다. instance ID는 default 값이 0인 32-bit unsigned integer이다. 

만약 vertex shader input declaration 가 instance id semantic 을 포함한다면 IA stage는 각 정점 마다 instance ID 를 추가 할 것이다. 각 indexed draw call에 대해서, instance id 는 1씩 증가한다. 다른 모든 draw call 들은 instance id 의 값을 변경하지 않는다. 만약 instace id 가 2^32-1을 초과한다면 그것을 0으로 뒤덮는다.

## Example

다음의 그림은 어떻게 system value들이 IA stage 에서 instanced triangle strip 에 첨부 되는지 를 보여준다.

![[Pasted image 20230106163528.png]]

아래의 테이블은 그 같은 triangle strip의 instance 양쪽 둘 다에서 생성된 system value를 보여준다.
첫 번째 instance (instance U)는 파란색으로 표현되고, 두 번째 instance (instance V)는 초록으로 표현되었다. 직선은 primitive 내에서의 정점들을 연결하고, 점선은 adjacent vertex 들을 연결한다.

다음의 테이블은 instance U 에 대한 system-generated value를 보여준다.
| Vertex Data | C, U |  D, U |  E, U |  F U |  G, U |  H, U |  I, U |   J, U |   K, U |   L, U | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **VertexID** | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| **InstanceID** | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

| | value | value | value |
| --- |  --- |  --- |  --- |
| Primitive ID | 0 | 1 | 2 |
| Instance ID | 0 | 0 | 0 |



다음은 테이블은 instance V 에 대한 system-generated value를 보여준다.
| Vertex Data | C, V |  D, V |  E, V |  F V |  G, V |  H, V |  I, V |   J, V |   K, V |   L, V | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **VertexID** | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| **InstanceID** | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |

| | value | value | value |
| --- |  --- |  --- |  --- |
| Primitive ID | 0 | 1 | 2 |
| Instance ID | 1 | 1 | 1 |


intput assembler 는 (vertex, primitive, instance 의) id 들을 생성한다. 각 instance는 특별한 id 를 부여받는다는 것을 또한 주목 하라. 데이터는 triangle strip 의 각 instance를 구분하는 strip cut으로 끝난다.