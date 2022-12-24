렌더링을 할 render target(back buffer) 영역을 나타내는 구조체이다. 뷰포트를 설정하는 것은 렌더링할 화면 영역을 설정하는 것이다. 

보통 render target 전체를 설정한다. 깊이 값은 0.0f~ 1.0f이다. rasterizer stage 에서 다시 화면에 객체들을 매핑할 때 깊이 값을 0 ~ 1 로 바꿔준다. 

CommandList가 Reset() 되면 반드시 다시 뷰포트를 Set 해줘야 한다.

```c++
typedef struct D3D11_VIEWPORT
    {
    FLOAT TopLeftX;
    FLOAT TopLeftY;
    FLOAT Width;
    FLOAT Height;
    FLOAT MinDepth;
    FLOAT MaxDepth;
    } 	D3D11_VIEWPORT;
```

## Members

1. FLOAT TopLeftX : 뷰포트 왼쪽의 X 위치. D3D11_VIEWPORT_BOUNDS_MIN과 D3D11_VIEWPORT_BOUNDS_MAX 사이의 범위.
2. FLOAT TopLeftY : 뷰포트 상단의 Y 위치. D3D11_VIEWPORT_BOUNDS_MIN과 D3D11_VIEWPORT_BOUNDS_MAX 사이의 범위.
3. FLOAT Width : 뷰포트의 너비.
4. FLOAT Height : 뷰포트의 높이.
5. FLOAT MinDepth : 뷰포트의 최소 깊이. 범위는 0에서 1 사이.
6. FLOAT MaxDepth : 뷰포트의 최대 깊이. 범위는 0에서 1 사이

## Remarks

모든 경우에 있어서 width와 height는 0보다 크거나 같아야 하고  TopLeftX + Width 와 TopLeftY + Height 는 D3D11_VIEWPORT_BOUNDS_MAX보다 작거나 같아야 한다.

