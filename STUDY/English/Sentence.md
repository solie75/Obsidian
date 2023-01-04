나의 처음 해석 - @
번역 해석 - $


1. "Regardless of implementation, this method queries an object using the `IID` of the interface to which the caller wants a pointer."
- 구현에 상관없이, 이 메서드는 호출자가 원하는 포인터를 가진 인터페이스의 IID 를 사용하여 객체를 쿼리한다 (필자 해석)

2. " To use the texture as an input to a shader, create a by calling ID3D11Device::CreateShaderResourceView."

3. "Any render targets not defined by this call are set to NULL"
-> @ : "어떤 render target 도 이 호출이 NULL 로 설정됨으로서 정의되지 않는다."
-> $ : "이 호출로 정의 되지 않은 모든 render target 은 NULL 로 설정된다." 

4. " if the render-target views were created from an array resource type,"
-> @ 만약 그 render target view 가 배열 리소스 타입으로부터 생성된다면

5. "Creating a fully-typed resource restricts the resource to the format it was created with."
-> @ 완전한 typed resource 를 생성하는 것은 그 리소스를 생성될 때의 형식을 제한한다.
-> $ 완전한 typed resource 를 생성하는 것은 그 리소스를 생성될 때의 형식으로 제한한다.

6. "A resource view is conceptually similar to casting the resource data so that it can be used in a particular context."
-> @ 리소스 뷰는 특정한 context 내에서 사용 될 수 있는 리소스 데이터를 캐스팅하는 것과 개념적으로 비슷하다.
-> $ 리소스 뷰는 특정 context에서 사용될 수 있도록 리소스 데이터를 캐스팅 하는 것과 개념적으로 유사하다.

7. "A view created for a typeless resource alwaps has the same number of bits per component;"
-> @ view는 언제나 각 component(구성요소)마다 같은 크기의 비트를 갖는 typeless 리소스를 생성한다.  
-> $ typeless 리소스로 생성된 view 는 언제나 각 component(구성요소)마다 동일한 수의 비트를 가진다.

8. "The format specified must be from the same family as the typeless format used when creating the resource."
-> @ 지정된 포맷은 리소스 생성할 때 사용된 typeless 포맷 같은 family 출신이어야 한다. 
->$ 지정된 형식은 리소스를 만들 때 사용된 유형 없는 형식과 동일한 패밀리에 속해야 합니다.

9. "Identify how the buffer is expected to be read from and written to."
-> @ 어떻게 버퍼를 읽고 쓰는지 식별한다.

10. "the number of depth slices in each level is half the number of the previous level (minimum 1) and rounded down if dividing by two results in a non-whole number"
-> $ 각 수준의 깊이 슬라이스 수는 이전 수준 수의 절반(최소 1)이며 2로 나눈 결과 정수가 아닌 경우 내림된다.

11. " This may require providing a dummy vertex (forming a degenerate triangle), or perhaps by flagging in one of the vertex attributes whether the vertex exists or not."
-> @ 이것은 더미 정점의 공급을 요
-> $  이를 위해서는 더미 정점(퇴화 삼각형 형성)을 제공하거나 정점의 존재 여부에 관계없이 정점 속성 중 하나에 플래그를 지정해야 할 수 있습니다.

12. "the shader must program its own choice of SV_PrimitiveID value for each output primitive if a subsequent pixel shader inputs SV_PrimtiveID."
->$ 만약 다음 픽셀 쉐이더가 SV_PrimitiveID 를 입력한다면 shader는 각 출력 primitive 에 대해 자체적으로 선택한 SV_PrimitiveID를 프로그래밍 해야한다.

13. "Primitives with adjacency (except line strips) contain exactly twice as many vertices as the equivalent primitive without adjacency, where each additional vertex is an adjacent vertex. "

-> @ (line strip 를 제외한) 인접성을 가진 Primitive는 인접성을 갖지 않고 나머지는 동등한(같은)  Primitive 의 딱 두배 많은 정점을 포함한다. (이때 각각의 추가적인 정점이 바로 adjacnet vertex 이다.)
-> $ 인접성이 있는 프리미티브(라인 스트립 제외)는 인접성이 없는 동등한 프리미티브보다 정확히 두 배 많은 정점을 포함하며 각 추가 정점은 인접한 정점입니다.