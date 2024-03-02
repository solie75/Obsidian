sampler state 를 설명한다.

# Syntax

```c++
typedef struct D3D11_SAMPLER_DESC {
  D3D11_FILTER               Filter;
  D3D11_TEXTURE_ADDRESS_MODE AddressU;
  D3D11_TEXTURE_ADDRESS_MODE AddressV;
  D3D11_TEXTURE_ADDRESS_MODE AddressW;
  FLOAT                      MipLODBias;
  UINT                       MaxAnisotropy;
  D3D11_COMPARISON_FUNC      ComparisonFunc;
  FLOAT                      BorderColor[4];
  FLOAT                      MinLOD;
  FLOAT                      MaxLOD;
} D3D11_SAMPLER_DESC;
```

1. Filter
texture 를 샘플링할 때 사용할 필터링 방법

- 2 ~ 4 까지 텍스처의 u, v, w 방향으로 어떻게 채울건지에 대한 변수. 범위 0~1. (w 는 대각선 방향)
2. AddressU
3. AddressV
4. AddressW

다음과 같은 선택지가 있다.

	1. D3D11_TEXTURE_ADDRESS_WRAP : 텍스처를 반복시킴
	2. D3D11_TEXTURE_ADDRESS_MIRROR : 텍스쳐가 반전되어 보임
	3. D3D11_TEXTURE_ADDRESS_CLAMP : 텍스처의 마지막 픽셀을 늘린 모습
	4. D3D11_TEXTURE_ADDRESS_BORDER : 텍스처를 색으로 지정한다.

D3D11_TEXTURE_ADDRESS_WRAP 의 경우
![[Pasted image 20230417223651.png]]
D3D11_TEXTURE_ADDRESS_MIRROR 의 경우
![[Pasted image 20230417223711.png]]
D3D11_TEXTURE_ADDRESS_CLAMP 의 경우
![[Pasted image 20230417223726.png]]
D3D11_TEXTURE_ADDRESS_BORDER 의 경우
![[Pasted image 20230417223742.png]]

6. MipLODBias
계산된 mipmap level의 편차. 예를 들어 Direct3D 가 texture 이 mipmap level 3 로 샘플링 되어야 한다고 계산 하고 MipLODBias 는 2 인 경우 texture 는  mipmap level 5 로 샘플링 된다.

6. MaxAnisotropy
Filter 가 D3D11_FILTER_ANISOTROPIC 또는 D3D11_FILTER_COMPARISON_ANISOTROPIC 로 특정된 경우 사용되는 Clamping 값으로 1~16 이 유효하다.

7. ComparisonFunc
샘플링된 데이터와 기존의 샘플링된 데이터를 비교(겹치는 부분에 대한 비교)하는 함수.

8. BorderColor[4]
2,3,4의 매개변수중 하나 이상이D3D11_TEXTURE_ADDRESS_BORDER일 때 사용. 0 ~ 1 범위로 값을 주면 그에 맞는 색깔을 샘플링

9. MinLOD
밉맵 레벨의 최소 수준 제한 값

10. MaxLOD
밉맵 레벨의 최대 수준 제한 값


## D3D11_FILTER 의 대표적인 값 2가지

^1cced4

1. Sampling 방식:
    
    - Point Filtering: 포인트 필터링은 각 텍셀의 중심에 대해 단순히 가장 가까운 텍스처 샘플을 사용하여 이미지를 확대 또는 축소합니다. 이는 텍스처의 각 픽셀이 그대로 표시되어 확대할 때 계단 현상이 발생할 수 있습니다.
    - Anisotropic Filtering: 이방성 필터링은 텍스처 샘플링 시 주변 픽셀을 고려하여 중심 픽셀의 값을 계산하는 반면, 텍스처 좌표와의 관련성을 고려하여 샘플링을 수행합니다. 이것은 텍스처를 다양한 각도로 보거나, 텍스처가 멀리 떨어진 경우에도 세부 사항을 유지하려는 경우에 특히 유용합니다.
2. 결과적인 이미지 품질:
    
    - Point Filtering: 포인트 필터링은 텍스처를 확대 또는 축소할 때 계단 현상이 발생할 수 있으며, 결과적으로 이미지의 부드럽고 자연스러운 표현이 부족할 수 있습니다.
    - Anisotropic Filtering: 이방성 필터링은 텍스처의 세부 사항을 더욱 세밀하게 보여주므로 더 자연스럽고 부드러운 이미지를 제공합니다. 특히 텍스처가 다양한 각도로 보일 때 뛰어난 결과를 보여줍니다.

요약하면, point 필터링은 간단하고 빠르지만 이미지 품질에 제한이 있으며, anisotropic 필터링은 더 높은 이미지 품질을 제공하지만 연산 비용이 높을 수 있습니다.