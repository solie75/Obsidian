ID3D11DeviceContext::Draw( );

인덱스 화 되지 않고, 인스턴스 화 되지 않은 primitive 를 그린다.

# Syntax

```c++
void Draw(
  [in] UINT VertexCount,
  [in] UINT StartVertexLocation
);
```

1. VertexCount
	그려질 정점의 수.

2. StartVertexLocation
	일반적으로 정점 버퍼의 오프셋인 첫 번째 정점의 인덱스.


# Remark

Draw( ) 는 작업을 rendering pipeline 에 제출한다.

그리기 호출에 대한 정점 데이터는 일반적으로 파이프 라인에 바인딩된 정점 버퍼에서 가져온다.

pipeline 에 바인딩 된 vertex buffer 가 없을 지라고, [SV_VertexID](https://learn.microsoft.com/en-us/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics) system-value semantic 을  사용하여 런타임에서 처리 중인 현재 vertex 를 확인하여 vertex shader 에서 고유한 vertex data 를 생성 할 수 있다.
