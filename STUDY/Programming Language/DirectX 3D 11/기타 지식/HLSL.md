High-Level Shader Language

버텍스 쉐이더, 픽섹 쉐이더를 작성하기 위해 사용하는 언어, 문법은 미리 정의된 타입들과 함께 c 언어와 거의 같다. HLSL 프로그램은 전역 변수, 타입 정의, 버텍스 쉐이더, 픽셀 쉐이더, 그리고 geometry shader 로 구성되어 잇다. 


## Semantic

<span style="color:yellow">semantic 은 shader 입력값 혹은 출력 값에 붙는 매개변수의 사용 의도에 대한 정보를 전달하는 문자열</span>이다. Semantic은 shader stage 사이에 전달 되는 모든 변수에 필요하다.

보통, 파이프 라인 stage 사이에서 전달되는 데이터는 완전히 generic(데이터의 타입을 일반화 한다는 것의 의미하며 클래스나 메소드에서 사용할 내부 데이터 타입을 컴파일 시에 미리 지정하는 방법을 말한다...? ) 이며  시스템에 의해 평범하게 해석된다(특별하게 해석되지 않는다.). 