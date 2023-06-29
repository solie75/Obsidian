depth-stencil state 설명
깊이 스텐실 상태는 출력 병합 단계에서 깊이 스텐실 테스트를 수행하는 방법을 제어

```c++
typedef struct D3D11_DEPTH_STENCIL_DESC { 
	BOOL DepthEnable;
	D3D11_DEPTH_WRITE_MASK DepthWriteMask;
	D3D11_COMPARISON_FUNC DepthFunc;
	BOOL StencilEnable;
	UINT8 StencilReadMask;
	UINT8 StencilWriteMask; 
	D3D11_DEPTH_STENCILOP_DESC FrontFace;
	D3D11_DEPTH_STENCILOP_DESC BackFace;
} D3D11_DEPTH_STENCIL_DESC;
```

BOOL DepthEnable : 깊이 테스트를 활성화

D3D11_DEPTH_WRITE_MASK DepthWriteMask : 깊이 데이터로 수정할 수 있는 깊이 스텐실 버퍼 부분을 식별
	D3D11_DEPTH_WRITE_MASK_ZERO : Turn off writes to the depth-stencil buffer.
	D3D11_DEPTH_WRITE_MASK_ALL : Turn on writes to the depth-stencil buffer.

D3D11_COMPARISON_FUNC DepthFunc : 깊이 데이터와 기존 깊이 데이터를 비교하는 기능
	D3D11_COMPARISON_LESS : If the source data is less than the destination data, the comparison passes. 기존의 z 보다 현재 정점의 z 가 더 작을 때, 즉 현재 정점이 더 앞에 있을 때만 z 값(깊이)를 갱신한다. ->즉 같은 깊이의 빨 간색 -> 초록색 순서로 겹쳐서 그리면 겹치는 부분은 그대로 빨간색이다. 이는 나중에 그린 삼각형은 같은 깊이 일때 무시된다. 이때 같은 깊이에서 나중에 그리는 삼각형이 더 앞에 오게 하고 싶다면 D3D11_COMPARISON_LESS_EQUAL 이라는 값을 사용한다.
	D3D11_COMPARISON_GREATER : If the source data is greater than the destination data, the comparison passes.

BOOL StencilEnable : 스텐실 테스트를 활성화

UINT8 StencilReadMask :  스텐실 데이터를 읽기 위한 깊이 스텐실 버퍼의 일부를 식별

UINT8 StencilWriteMask :  스텐실 데이터를 쓰기 위한 깊이 스텐실 버퍼의 일부를 식별

D3D11_DEPTH_STENCILOP_DESC FrontFace : 표면 법선이 카메라를 향하고 있는 픽셀에 대한 깊이 테스트 및 스텐실 테스트의 결과를 사용하는 방법을 식별

D3D11_DEPTH_STENCILOP_DESC BackFace :  표면 법선이 카메라 반대쪽을 향하는 픽셀에 대한 깊이 테스트 및 스텐실 테스트의 결과를 사용하는 방법을 식별