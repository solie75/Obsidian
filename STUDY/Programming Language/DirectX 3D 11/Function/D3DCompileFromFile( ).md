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
3. ID3DInclude *pInclude : 
https://learn.microsoft.com/en-us/windows/win32/api/d3dcompiler/nf-d3dcompiler-d3dcompilefromfile