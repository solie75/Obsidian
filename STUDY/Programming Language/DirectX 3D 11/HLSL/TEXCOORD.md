UV 일부 부분만 출력
텍스처의 일부분만 출력하려면, UV 좌표(텍스처 좌표)를 조정하여 텍스처 이미지의 특정 영역만을 사용하면 됩니다. 이를 텍스처 좌표의 스케일(scale)과 오프셋(offset)을 조절하여 구현할 수 있습니다.

텍스처 좌표의 스케일과 오프셋을 조정하여 원하는 부분만 출력하는 방법은 다음과 같습니다:

1. 텍스처 좌표 스케일(scale) 조정:
    
    - 텍스처 좌표 스케일을 변경하여 텍스처의 일부분만 출력할 수 있습니다.
    - 원본 UV 좌표 범위는 보통 0부터 1까지이며, 이 범위를 변경하여 출력할 부분을 선택합니다.
    - 예를 들어, 텍스처의 좌상단 1/4 영역만 출력하려면, UV 좌표를 (0.0, 0.0) ~ (0.25, 0.25)로 스케일링합니다.
2. 텍스처 좌표 오프셋(offset) 조정:
    
    - 텍스처 좌표 오프셋을 변경하여 텍스처 이미지에서 원하는 부분을 선택할 수 있습니다.
    - <span style="color: yellow">UV 좌표의 기준점을 변경</span>하여 출력할 부분을 이동시킵니다.
    - 예를 들어, 텍스처의 우측 하단 1/4 영역만 출력하려면, UV 좌표를 (0.75, 0.75) ~ (1.0, 1.0)로 오프셋합니다.

이러한 스케일과 오프셋을 조정하는 작업은 주로 HLSL의 쉐이더 코드에서 이루어집니다. Vertex Shader에서 텍스처 좌표를 조작하여 픽셀 쉐이더로 전달하거나, Pixel Shader에서 직접 조정하여 텍스처를 샘플링하는 방식으로 구현할 수 있습니다.

예시 코드에서는 텍스처 좌표를 스케일과 오프셋을 이용하여 텍스처의 우측 하단 1/4 영역만을 출력하는 방법을 보여줍니다:

```hlsl
struct PixelInput {
    float4 position : SV_POSITION; // 정점 위치 정보
    float3 normal : NORMAL; // 정점 법선 정보
    float2 texcoord : TEXCOORD0; // 텍스처 좌표 정보
};

Texture2D texDiffuse : register(t0);
SamplerState sampLinear : register(s0);

float2 scale = float2(0.25, 0.25); // 텍스처 좌표 스케일
float2 offset = float2(0.75, 0.75); // 텍스처 좌표 오프셋

float4 main(PixelInput input) : SV_Target {
    // 텍스처 좌표를 조정하여 텍스처의 우측 하단 1/4 영역만 샘플링
    float2 modifiedTexcoord = input.texcoord * scale + offset;
    float4 diffuseColor = texDiffuse.Sample(sampLinear, modifiedTexcoord);
    return diffuseColor;
}
```

위 예시 코드에서 `scale`과 `offset` 변수를 이용하여 텍스처 좌표를 수정하고, 수정된 텍스처 좌표로 텍스처를 샘플링하여 우측 하단 1/4 영역만을 출력합니다. 텍스처 좌표를 조정함으로써 출력할 텍스처 영역을 자유롭게 설정할 수 있습니다.