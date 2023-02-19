ID3D11DeviceContext::DrawIndexed( );

indexing 되고 인스턴스 화되지 않은 primitive 를 Draw 한다.

# Syntax

```c++
void DrawIndexed(
  [in] UINT IndexCount,
  [in] UINT StartIndexLocation,
  [in] INT  BaseVertexLocation
);
```

1. IndexCount
	draw 할 indices 의 개수.

2. StartIndexLocation
	GPU 가 index buffer 로부터 읽을 첫 번째 index 의 위치.

3. BaseVertexLocation
	vertex buffer 로부터 vextex 를 읽기 전에 각 index 에 더할 값.


# Remarks

draw API 는 rendering pipeline 에 작업을 제출한다. 두 index 들의 합이 읍수라면 해당 함수의 결과는 undefined 이다.