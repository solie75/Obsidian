device 인터페이스는 resource 를 생성하는 데 사용되는 가상의 adapter 를 표시한다.

**Note**  The latest version of this interface is [ID3D11Device5](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11_4/nn-d3d11_4-id3d11device5) introduced in the Windows 10 Creators Update. Applications targetting Windows 10 Creators Update should use the **ID3D11Device5** interface instead of **ID3D11Device**.

## Ingeritance

ID3D11Device 인터페이스는 IUnknown 인터페이스를 상속받는다.

## Methods

#### 1. ID3D11Device::CreateBuffer

Creates a buffer (vertex buffer, index buffer, or shader-constant buffer).

- Syntax
```c++
HRESULT CreateBuffer( 
	[in] const D3D11_BUFFER_DESC *pDesc, 
	[in, optional] const D3D11_SUBRESOURCE_DATA *pInitialData, 
	[out, optional] ID3D11Buffer **ppBuffer 
);
```
1. pDesc
D3D11_BUFFER_DESC 구조체를 가리키는 포인터.

2. pInitialData
초기화 데이터를 설명하는 D3D11_SUBRESOURCE_DATA 구조체를 가리키는 포인터.공간만 할당받기 위해서는  NULL 을 사용한다.(단,  사용하는 flag 가 D3D11_USAGE_IMMUTABLE 일 때에는 NULL 일 수 없다 ). pInitialData 에 아무 값도 전달하지 않는 경우, 리소스가 읽히ㅣ기 전에 다른 방법으로 buffer 의 내용을 작성해야한다.

3. ppBuffer
생성된 buffer 객체에 대한 ID3D11Buffer 인터페이스를 가리키는 포인터의 주소. 다른 입력 매개변수의 유효성을 검사하려면 이 매개변수를 NULL 로 설정한다.

- Return value
This method returns **E_OUTOFMEMORY** if there is insufficient memory to create the buffer. See [Direct3D 11 Return Codes](https://learn.microsoft.com/en-us/windows/desktop/direct3d11/d3d11-graphics-reference-returnvalues) for other possible return values.



## Reference

https://learn.microsoft.com/en-us/windows/win32/api/d3d11/nn-d3d11-id3d11device