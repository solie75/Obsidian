ID3D11DeviceContext::IASetVerttexBuffers( );

vertex buffer 들의 배열을 input-assembler stage 에 바인딩한다. 랜더링 파이프 라인의 과정이 시작되면 지정한 버퍼( 예, g_VB)가 IA 단계 시작 시에 전달 된다. 하지만 IASetVertexBuffer 호출이 렌더링 파이프 라인의 IA 과정에 속하는 것은 아니다. 단지 IA 단계에 사용될 버퍼를 알리는 것이다.

# Syntax

```c++
void IASetVertexBuffers(
  [in]           UINT         StartSlot,
  [in]           UINT         NumBuffers,
  [in, optional] ID3D11Buffer * const *ppVertexBuffers,
  [in, optional] const UINT   *pStrides,
  [in, optional] const UINT   *pOffsets
);
```

1. StartSlot
	바인딩을 한 첫번째 입력 slot. 첫 번째 buffer는 명시적으로 첫 번째 slot 에 바운딩 된다. 이로인해 배열 내의 각각의 추가적인 vertex buffer는 각 차후의 input slot 에 암시적으로 바인딩 된다. The maximum of 16 or 32 input slots (ranges from 0 to D3D11_IA_VERTEX_INPUT_RESOURCE_SLOT_COUNT - 1) are available; input slot 의 최대 수는 feature level 에 따라 결정 된다.

2. NumBuffers
	배열 내의 vertex buffer 의 개수. 버퍼의 개수는 input assembler 의 input slot 의 최대 개수를 초과 할 수 없다.

3. ppVertexBuffers
	stride value 의 배열을 가리키는 포인터. vertex buffer 배열의 각 버퍼에 대한 하나의 stride value. 각 stride 는 vertex buffer 에서 사용되는 element 의 크기이다.

4. pStrides
	stride value 들의 배열을 가리키는 포인터. vertex buffer 배열 내의 가 버퍼에 대한 하나의 stride value. 각 stride 는 vertex buffer 에서 사용되는 element의 크기 이다.

4. pOffsets
	offset value 의 배열을 가리키는 포인터. vertex buffer 배열내의 각 버퍼에 대한 하나의 offset value. 각 offset 은 vertex buffer 의 첫 번째 element 와 첫번째로 사용될 element 사이의 byte 수이다.


# Remark

작성을 위해 바인딩된 상태의 버퍼를 사용하는 이 메서드를 호출(즉, stream output pipeline stage 에 바인딩 된)하면 버퍼가 입력과 출력으로 동시에 바인딩 딜 수 없기 때문에 대신 효과적으로 NULL 을 바인딩 한다.
The debug layer will generate a warning whenever a resource is prevented from being bound simultaneously as an input and an output, but this will not prevent invalid data from being used by the runtime.

The method will hold a reference to the interfaces passed in. This differs from the device state behavior in Direct3D 10.