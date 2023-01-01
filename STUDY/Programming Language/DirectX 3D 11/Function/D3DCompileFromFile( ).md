주어진 타겟에 대하여 HLSL 코드를 Bytecode 로 컴파일한다.

## Syntax

```c++
HRESULT D3DCompileFromFile( 
	[in] LPCWSTR pFileName, 
	[in, optional] const D3D_SHADER_MACRO *pDefines, 
	[in, optional] ID3DInclude *pInclude, 
	[in] LPCSTR pEntrypoint, 
	[in] LPCSTR pTarget, 
	[in] UINT Flags1, 
	[in] UINT Flags2, 
	[out] ID3DBlob **ppCode, 
	[out, optional] ID3DBlob **ppErrorMsgs );
```

## Parameters

1. LPCWSTR pFileName : shader code 를 포함한 파일의 주소를 나타내는  constant null_terminated string을 가리키는 포인터이다.
2. const D3D_SHADER_MACRO *pDefines : shader macros를 정의 하는 D3D_SHADER_MACRO 구조체의 optional array이다. 각 macro definition 은 이름과 null-terminated definition 을 포함한다. 만약 사용하지 않을 거면, NULL 을 설정한다. 배열의 마지막 구조체(structure)는 terminator(마지막에 있는 존재)의 역할을 하고 모든 맴버는 NULL로 세팅되어야 한다.
3. ID3DInclude *pInclude : 포함 파일을 다루는데 사용하는 컴파일러인  ID3DInclude 인터페이스에 대한 optional pointer 이다. 만약 이 매개변수가 NULL 로 설정되거나  shader가  "#include"를 포함한다면 컴파일 에러가 발생한다. 기본 include handler를 가리키는 포인터인 D3D_COMPILE_STANDARD_FILE_INCLUDE 매크로를 전달할 수 있다. 이 기본 include handler 는 현재 디렉토리에 연관된 파일을 include 한다.;
4. LPCSTR pEntrypoint : shader 실행이 시작되는 곳의 함수 를 가키리는 entry인 shader의 이름을 포함하는 constant null-terminated string 을 가리키는 포인터이다. (컴파일할 대상인 함수의 이름)
5. LPCSTR pTarget : shader target나 컴파일을 위한  shader의 특징 세트(집합) 을 특정 짓는 constant null-terminated string을 가리키는 포인터이다. 그 shader target 은 shader model(셰이더 버전) 이 된다. 그 target 은 또한 effect type 이 될 수 있다.
6. UINT Flags1 : bitwise OR 연산자 를 사용함으로서 조합되는 shader compile option의 조합.
7. UINT Flags2 : bitwise OR 연산자 를 사용함으로서 조합되는 effect compile option 조합. 결과 값은 컴파일러가 그 effect 를 어떻게 컴파일하는지를 지정한다. shader 를 컴파일하거나 effect 파일이 없을 때, D3DCompileFromFile 은 Flags2를 무시한다. Flags2를 0으로 설정하는 것이 추천된다. 왜냐하면 호출된 함수가 비포인터 매개변수를 사용하지 않을 경우 비포인터 매개변수를 0으로 설정하는 것이 좋은 프로그래밍 습관이기 때문이다.
8. ID3DBlob **ppCode : <span style="color:yellow">컴파일된 코드에 접근하는데 사용할 수 있는 ID3DBlob 인터페이스를 가리키는 포인터를 받는 변수를 가리키는 포인터</span>
9. ID3DBlob **ppErrorMsgs : 컴파일러 오류 메시지에 접근하는데 사용할 수 있는 ID3DBlob 인터페이스를 가리키는 포인터를 받는 변수를 가리키는 선택적 포인터. 만약 에러가 없다면 NULL.
