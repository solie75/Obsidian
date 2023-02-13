ID3D11DeviceContext::PSSetShader( );

# Syntax

```c++
void PSSetShader(
  [in, optional] ID3D11PixelShader   *pPixelShader,
  [in, optional] ID3D11ClassInstance * const *ppClassInstances,
                 UINT                NumClassInstances
);
```

1. pPixelShader
	픽셀 셰이더에 대한 포인터( [ID3D11PixelShader](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nn-d3d11-id3d11pixelshader) 참조 ). **NULL** 을 전달하면 이 파이프라인 단계에 대한 셰이더가 비활성화된다.

2. ppClassInstances
	class-instance 인터페이스의 배열을 가리키는 포인터( [ID3D11ClassInstance](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nn-d3d11-id3d11classinstance) 참조 ). 쉐이더가 사용하는 각 인터페이스는 상응하는 class instance 를 반드시 가지고 있어야 하며, 그렇지 않다면 shader 는 비활성화 된다. shader 가 어떠한 인터페이스도 사용하지 않는다면 NULL 을 설정한다.

3. NumClassInstances
	배열 내의 class instance 인터페이스의 개수.


# Remark

해당 메서드는 전달된 인터페이스에 대한 참조를 보유한다.

쉐이더가 가질 수 있는 최대 인스턴스 수는 256 rodlek.

쉐이더에서 인터페이스가 사용되지 않는 경우  ppClassInstances 를 NULL 로 설정한다. NULL 이 아니라면 class instance 의 수는 쉐이더에서 사용되는 인터페이스 수와 일치해야 한다. 또한 각 인터페이스 포인터에는 해당 class instance 가 있어야 하며 그렇지 않다면 할당된 쉐이더가 비활성화 된다.