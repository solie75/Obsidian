## Syntax

```c++
typedef struct DXGI_MODE_DESC {
	UINT                     Width; 
	UINT                     Height; 
	DXGI_RATIONAL            RefreshRate; 
	DXGI_FORMAT              Format; 
	DXGI_MODE_SCANLINE_ORDER ScanlineOrdering; 
	DXGI_MODE_SCALING        Scaling;
} DXGI_MODE_DESC;
```

1. Width
resolution 너비 값. swap chain 을 생성하기 위해서 IDXGIFactory::CreateSwapChain 메서드를 호출할 때 이 값을 0으로 특정하는 경우, 런타임은 출력 윈도우의 너비를 포함하고 이 너비 값을 swap-chain description 에 부과한다.
2. Height
resolution 높이 값. swap chain 을 생성하기 위해서 IDXGIFactory::CreateSwapChain 메서드를 호출할 때 이 값을 0으로 특정하는 경우, 런타입은 출력 윈도우의 높이를 포함하고 이 너비 값을 swap-chain description 에 부과한다.
3. RefreshRate
DXGI_RATIONAL 구조체로 설명하는 헤르츠 단위의 화면 재생 빈도율
4. Format
DXGI_RATIONAL 구조체로 설명하는 display 형식
5. ScanlineOrdering
scanline 그리기 모드를 설명하는 DXGI_MODE_SCANLINE_ORDER 열거형 형식의 맴버이다.
6. Scaling
Scaling 모드를 설명하는 DXGI_MODE_SCALING 열거형 형식의 맴버

# Remark

이 구조체는 [**GetDisplayModeList**](https://msdn.microsoft.com/en-us/library/bb174549(v=vs.85)) 와 [**FindClosestMatchingMode**](https://msdn.microsoft.com/en-us/library/bb174547(v=vs.85)) 메서드에 의해 사용된다.

다음의 값들은 display mode 와 bit-block trasger (bitblt) medel swap chain 를 생성할 때 유용한다. 그 유효한 값은 사용자가 작업하는 feature level 에 기반한다.

-   Feature level >= 9.1
    
    -   [**DXGI_FORMAT_R8G8B8A8_UNORM**](https://msdn.microsoft.com/en-us/library/bb173059(v=vs.85))
    -   [**DXGI_FORMAT_R8G8B8A8_UNORM_SRGB**](https://msdn.microsoft.com/en-us/library/bb173059(v=vs.85))
    -   [**DXGI_FORMAT_B8G8R8A8_UNORM**](https://msdn.microsoft.com/en-us/library/bb173059(v=vs.85)) (except 10.x on Windows Vista)
    -   [**DXGI_FORMAT_B8G8R8A8_UNORM_SRGB**](https://msdn.microsoft.com/en-us/library/bb173059(v=vs.85)) (except 10.x on Windows Vista)
-   Feature level >= 10.0
    
    -   [**DXGI_FORMAT_R16G16B16A16_FLOAT**](https://msdn.microsoft.com/en-us/library/bb173059(v=vs.85))
    -   [**DXGI_FORMAT_R10G10B10A2_UNORM**](https://msdn.microsoft.com/en-us/library/bb173059(v=vs.85))
-   Feature level >= 11.0
    
    -   [**DXGI_FORMAT_R10G10B10_XR_BIAS_A2_UNORM**](https://msdn.microsoft.com/en-us/library/bb173059(v=vs.85))

화면에 displaying 하기에 유효한 format 인지를 결정하기 위해서 사용자는 이 format 값들중에 하나를 ID3D11Device::CheckFormatSupport 에 전달할 수 있다. 만약 ID3D11Device::CheckFormatSupport 가 pFormatSupport 매개변수 point 가 존재하는 bit field 에서 D3D11_FORMAT_SUPPORT_DISPLAY 를 반환한다면, 그 format 은 화면에 displaying 하기에 유효하다.
