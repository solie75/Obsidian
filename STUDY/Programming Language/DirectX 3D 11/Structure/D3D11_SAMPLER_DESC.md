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