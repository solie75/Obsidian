ID3D11DeviceContext::VSSetShader( );

디바이스에 vertex shader 를 설정한다.

# Syntax

```c++
void VSSetShader(
  [in, optional] ID3D11VertexShader*   pVertexShader,
  [in, optional] ID3D11ClassInstance* const*   ppClassInstances,
                 UINT                NumClassInstances
);
```

1. pVertexShader
	vertexshader 를 가리키는 포인터 (see [ID3D11VertexShader](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nn-d3d11-id3d11vertexshader)). NULL 을 전달하면 pipeline stage 에서 해당 shader 가 비활성화 된다.

2. ppClassInstances
	class-instance 인터페이스의 배열을 가리키는 포인터 (see [ID3D11ClassInstance](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nn-d3d11-id3d11classinstance)). 쉐이더가 사용하는 각 인터페이스는 상응하는 class instance 를 반드시 가지고 있어야 하며, 그렇지 않다면 shader 는 비활성화 된다. shader 가 어떠한 인터페이스도 사용하지 않는다면 NULL 을 설정한다.

3. NumClassInstances
	배열 내의 class instance 인터페이스의 개수.


# Remark

해당 메서드는 전달된 인터페이스에 대한 참조를 가진다. 쉐이더가 가질 수 있는 최대 인스턴스 수는 256 개이다.

