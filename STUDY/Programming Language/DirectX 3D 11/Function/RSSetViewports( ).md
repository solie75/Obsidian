뷰포트의 배열을 파이프 라인의 rasterizer stage에 바인딩한다.

```c++
void RSSetViewports( 
	[in] UINT NumViewports, 
	[in, optional] const D3D11_VIEWPORT *pViewports 
);
```

## Parameters

1. UINT NumViewports : 바인딩할 뷰포트의 수.
2. const D3D11_VIEWPORT *pViewports : device 에 bind 할 D3D11_Viewport 구조체의 배열.

## Remarks

모든 viewport는 원자적으로(atomically) 하나의 operation 으로서 세팅 되어야한다. 호출로 정의되지 않은 모든 viewport는 비활성화 된다.

