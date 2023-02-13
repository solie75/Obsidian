ID3D11DeviceContext::IASetPrimitiveTopology( )

input assembler stage 에 대한 input data 를 설명하는 primitive 유형과 데이터 순서에 대한 정보를 바인딩한다. 

# Syntax

```c++
void IASetPrimitiveTopology(
  [in] D3D11_PRIMITIVE_TOPOLOGY Topology
);
```

1. Topology
	primitive 의 유형과 primitive data 의 순서.