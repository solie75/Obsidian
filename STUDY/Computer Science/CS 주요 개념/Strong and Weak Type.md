컴퓨터 프로그래밍에서 프로그래밍 언어를 구어체로 분류하는 많은 방법 중 하나.
그러나 용어의 의미에 대한 정확한 기술적 정의는 없다.

일반적으로

<span style="color: yellow">strong type : 컴파일 시간에 더 엄격한 유형 지정 규칙을 가진다. -> 이는 컴파일 중에 오류와 예외가 발생할 가능성이 더 높다는 것을 의미</span>한다. 
이러한 규칙의 대부분은 변수 할당, 함수 반환 값, 프로시저 인수 및 함수 호출에 영향을 미친다. 
Dynamically typed language 도 강력한 유형이 될 수 있다. 

weak type : 느슨한 유형 지정 규칙을 가지며 예측할 수 없거나 심지어 잘못된 결과를 생성하거나 런타임에 암시적 유형 변환을 수행할 수 있다. 

string type 에서는 해당 type 에 할 수 있는 일들이 명확하게 정의 되어 있는 반면, weak type은 사용되는 시점에 암시적으로 cast가 될 수도 있고, ad-hoc polymorphism 도 될 수 있다.
-> strong type은 다른 type과의 operation 이 지원되지 않고, weak type 은 어느 정도 지원된다. 

## Pointers

 어떤 프로그래밍 언어에서는 pointer 를 숫자 값 처럼 표시하고 그것을 사용자가 연산 할 수 있도록 허락한다. 이러한 언어는 <span style="color: yellow">포인터 연산이 language's type system을 우회하는데 사용할 수 있기 때문에 weakly type으로 여겨진다.</span>


리소스의 레이아웃(또는 memory footprint을 완전히 명시(지정)하는 두가지 방법이 있다.

- Typed : <span style="color: yellow">리소스가 생성될 때 type 을 완전히 지정</span>한다.
- Typeless : <span style="color:yellow ">리소스가 파이프 라인에 바인딩 될 때 type 을 완전히 지정</span>한다.

완전한 Typed resource의 생성은 리소스를 생성될 때의 형식으로 제한한다. 이것은 최적화된 엑세스의 런타임(<span style="color:green">the runtime to optimize access의 의미는?</span>)을 가능하게 한다. 특히 만약 리소스가  응용프로그램에서 매핑(하나의 값을 다른 값으로 대응시키는 것)할 수 없음을 나타내는 flag로 생성된 경우에 그러하다. 특정 타입으로 생성된 리소스는 D3D10_DDI_BIND_PRESENT flag로 생성되지 않는 한 view 매커니즘(<span style="color:green ">view mechanism 의 의미는?</span>)을 사용하여 재해석 할 수 없다. 만약 D3D10_DDI_BIND_PRESENT가 설정되었다면, 심지어는 원래의 리소스가 fully typed 로서 생성되었다고 할지라도 render target 또는 shader resource view 는 적절한 family(<span style="color:green">appropriate family 의 의미는?</span>)의 어떤 fully typed 맴버를 사용하여 이러한 리소스에서 생성될 수 있다. 

typeless 리소스에서, 리소스가 처음 생성될 때 유형을 알 수 없다. 응용 프로그램은 사용 가능한 typeless 포맷 중에서 선택해야 한다. 할당받을 매모리의 크기와 runtime이 (<span style="color:green ">runtime이 시간적 개념인데 subtexture를 생성할 수 있는가, runtime에 라고 해석해야 하는가</span>) mipmap 의 subtexture를 생성해야 하는지를 명시해야 한다. 그러나 정확한 데이터 포맷(메모리가 integers, floating point values, unsigned integers 등 중에 무엇으로 해석 될지)은 리소스가 리소스 뷰를 사용하여 pipeline에 바인딩 될 때까지 결정되지 않는다.  텍스쳐 포맷은 그 텍스쳐가 파이프 라인에 바인딩 될 때까지 유연하게 유지되기 때문에, 그 리소스는 weakly typed storage 라고 부른다. Weakly typed storage는 컴포넌트(구성 요소)의 수와 각 컴포넌트(구성 요소)의 비트 수가 다른 두 포맷에서 동일하면 재사용 되거나 reinterpret 될 수 있다.

각 위치에서 포맷을 완전히 규정하는(자격을 입증하는) 고유한 view 를 각각의 하나의 리소스가 갖는다면 그 리소스는 여러개의 파이프 라인 스테이지에 바인딩 될 수 있다.
For example, a resource created with the format DXGI_FORMAT_R32G32B32A32_TYPELESS could be used as a DXGI_FORMAT_R32G32B32A32_FLOAT and a DXGI_FORMAT_R32G32B32A32_UINT at different locations in the pipeline simultaneously.
