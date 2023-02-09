High-Level Shader Language

버텍스 쉐이더, 픽섹 쉐이더를 작성하기 위해 사용하는 언어, 문법은 미리 정의된 타입들과 함께 c 언어와 거의 같다. HLSL 프로그램은 전역 변수, 타입 정의, 버텍스 쉐이더, 픽셀 쉐이더, 그리고 geometry shader 로 구성되어 잇다. 


## Semantic

<span style="color:yellow">semantic 은 shader 입력값 혹은 출력 값에 붙는 매개변수의 사용 의도에 대한 정보를 전달하는 문자열</span>이다. Semantic은 shader stage 사이에 전달 되는 모든 변수에 필요하다.

보통, 파이프 라인 stage 사이에서 전달되는 데이터는 완전히 generic(데이터의 타입을 일반화 한다는 것의 의미하며 클래스나 메소드에서 사용할 내부 데이터 타입을 컴파일 시에 미리 지정하는 방법을 말한다...? ) 이며  시스템에 의해 평범하게 해석된다(특별하게 해석되지 않는다.). 

## System-Value Semantics

^6755d8

System-value semantics 는 Direct3D 10에서 등장했다. 모든 system-value는 rasterizer stage 에서 해석되는 SV_prefix 와 시작한다. system-value는 파이프라인의 다른 부분에서 효력이 있다. 예를 들어, SV_Position 은 출력 값 뿐만 아니라 vertex shader 에 대한 입력 값으로서 지정될 수 있다. Pixel shader 오직 system-value semantic 인 SV_Depth와 SV_Target 으로 매개변수(Parameter) 를 작성(write)할 수 있다. 

다른 sytem value( SV_VertexID, SV_InstanceID, SV_IsFrontFace )는 특정한 값을 해석할 수 있는 파이프 라인의 첫번째 활성화 쉐이더(active shader)에만 입력할 수 있다. 이후에, 그 shader function 은 그 value 를 subsequent stage 에 전달해야만 한다.

SV_PrimitiveID는 특정한 값을 해석할 수 있는 파이프라인의 첫번째 활성화 shader 에 전달되는 input 으로만 될 수 있는 이 규칙에서 예외이다. 하드웨어는 hull-shader stage, domain-shader stage 에 대한 입력과 동일한 id 값을 제공할 수 있고 그 이후에는 geometry-shader stage 혹은 pixel-shader stage 둘중 먼저 가능한 스테이지가 활성화된다.

만약 tessellation이 활성화 된다면, hull-shader stage와 domain-shader stage는 존재한다.  주어진 patch 에서 , 같은 PrimitiveID 는 patch의 hull-shader invocation과 모든 tessellated domain shader invocation을 지원한다. geometry-shader stage 또는 pixel-shader stage 가 활성화 상태라면 동일한 PrimiticeID는 또한 다음 활성화 stage로 전파한다.

만약 geometry shader가 SV_PrimitiveID 를 입력하고 그것이 0이나 각 invocation 당 하나 이상의 primitive를 출력할 수 있기 때문에, 만약 다음의 pixel shader 가 SV_PrimitiveID 를 입력한다면 shader는 각 output primitive 에 자체적으로 선택한 SV_PrimitiveID 값을 프로그래밍 해야한다. 

또 다른 예로, SV_PrimitiveID는 vertex 가 multiple primitexs의 member 이기 때문에  Vertex_shader stage 에서는 해석될 수 없다. 