ID3D11DeviceContext::IASetInputLayout( )

[[ID3D11InputLayout | input-layout object]] 를 input assembler stage에 바인딩한다. 


# Syntax

```c++
void IASetInputLayout(
  [in, optional] ID3D11InputLayout *pInputLayout
);
```

1. pInputLayout
	IA stage 에 의해 읽혀질 input buffer 를 설명하는 input layout object를 가리키는 포인터.


# Remark

intput-layour object는 vertex buffer data가 IA pipeline stage 에 스트리밍 되는 방법을 설명한다. [[CreateInputLayout( )]]을 호출하여 생성한다.

