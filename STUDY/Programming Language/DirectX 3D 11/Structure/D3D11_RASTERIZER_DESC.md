
래스터라이저 상태는 래스터라이저 단계의 동작을 정의.

```c++
typedef struct D3D11_RASTERIZER_DESC { 
	D3D11_FILL_MODE FillMode;
	D3D11_CULL_MODE CullMode;
	BOOL FrontCounterClockwise;
	INT DepthBias;
	FLOAT DepthBiasClamp;
	FLOAT SlopeScaledDepthBias;
	BOOL DepthClipEnable;
	BOOL ScissorEnable;
	BOOL MultisampleEnable;
	BOOL AntialiasedLineEnable;
} D3D11_RASTERIZER_DESC;
```

D3D11_FILL_MODE FillMode : 렌더링할 때 사용할 채우기 모드를 결정
	D3D11_FILL_WIREFRAME - 꼭짓점을 연결하는 선을 그립니다. 인접한 꼭짓점이 그려지지 않습니다.
	D3D11_FILL_SOLID - 꼭짓점에서 형성된 삼각형을 채웁니다. 인접한 꼭짓점이 그려지지 않습니다.
	
D3D11_CULL_MODE : 지정된 방향을 향한 삼각형이 그려지지 않음을 나타냄
	D3D11_CULL_NONE - 항상 모든 삼각형을 그립니다.
	D3D11_CULL_FRONT - 전면에 있는 삼각형은 그리지 마세요.
	D3D11_CULL_BACK - 후면에 있는 삼각형을 그리지 마세요.
	
BOOL FrontCounterClockwise :
	삼각형이 전면 또는 후면인지 확인합니다. 이 매개 변수가 **TRUE**이면 해당 꼭짓점이 렌더링 대상에서 시계 반대 방향으로 표시되고 시계 방향인 경우 역방향으로 간주되는 경우 삼각형이 전면으로 간주됩니다. 이 매개 변수가 **FALSE**이면 반대입니다.

INT DepthBias : 지정된 픽셀에 추가된 깊이 값

FLOAT DepthBiasClamp : 픽셀의 최대 깊이 바이어스

FLOAT SlopeScaledDepthBias : 지정된 픽셀의 기울기에서 스칼라

BOOL DepthClipEnable : 거리에 따라 클리핑을 사용하도록 설정
	하드웨어는 항상 래스터화된 좌표의 x 및 y 클리핑을 수행합니다. **DepthClipEnable**이 기본값인 **TRUE**로 설정되면 하드웨어는 z 값도 클립한다.
	**DepthClipEnable**을 **FALSE**로 설정하면 하드웨어는 z 클리핑(즉, 이전 알고리즘의 마지막 단계)을 건너뜁니다. 그러나 하드웨어는 여전히 "0w < " 클리핑을 수행합니다. z 클리핑을 사용하지 않도록 설정하면 픽셀 수준에서 부적절한 깊이 순서가 발생할 수 있습니다. 그러나 z 클리핑을 사용하지 않도록 설정하면 스텐실 섀도 구현이 간소화됩니다. 즉, 후면 클리핑 평면을 벗어나는 기하 도형에 대한 복잡한 특수 사례 처리를 방지할 수 있습니다.

BOOL ScissorEnable : 가위 사각형 컬링을 사용하도록 설정합니다. 활성 가위 사각형 외부의 모든 픽셀은 컬링됩니다.

BOOL MultisampleEnable : MSAA(다중 샘플 앤티앨리어싱) 렌더링 대상에서 사분면 또는 알파선 앤티앨리어싱 알고리즘을 사용할지 여부를 지정한다. Set to **TRUE** to use the quadrilateral line anti-aliasing algorithm and to **FALSE** to use the alpha line anti-aliasing algorithm.

BOOL AntialiasedLineEnable : 줄 앤티앨리어싱을 사용할지 여부를 지정합니다. 선 그리기를 수행하고 **MultisampleEnable** 이 **FALSE**인 경우에만 적용됩니다
