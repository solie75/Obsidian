컴파일된 shader 로 vertex-shader 객체를 생성한다. 

## Syntax

```c++
HRESULT CreateVertexShader( 
	[in] const void *pShaderBytecode, 
	[in] SIZE_T BytecodeLength, 
	[in, optional] ID3D11ClassLinkage *pClassLinkage, 
	[out, optional] ID3D11VertexShader **ppVertexShader );
```

## Parameters

1. const void *pShaderBytecode : 컴파일된 쉐이더(blob)를 가리키는 포인터.
2. SIZE_T BytecodeLength : 컴파일된 vertex shader (blob)의 크기.
3. ID3D11ClassLinkage *pClassLinkage : class linkage interface를 가리키는 포인터. NULL 일 수 있다. ( [ID3D11ClassLinkage](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nn-d3d11-id3d11classlinkage))
4. ID3D11VertexShader **ppVertexShader : ID3D11VertexShader 인터페이스를 가리키는 포인터의 주소. 만약 NULL 이면, 모든 다른 매개변수가 유효하다면(be validated) 그리고 만약 모든 매개변수가 유효성 검사(validation)에 전달 되면 이 api 는 S_OK 대신에 S_FALSE 를 반환한다.


