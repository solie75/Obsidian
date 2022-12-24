Cube map (입방체(정육면체) 맵)은 기본적으로 여섯 장의 텍스처로 된 배열이나, 그 텍스쳐들을 해석하는 방식이 독특하다. cube mapping  을 이용하면 하늘에 텍스쳐를 입히거나 물체가 주변환경을 반사하는 모습을 흉내내기가 쉬워진다. 

## Cube mapping

Cube mapping 의 핵심은 여섯 장의 텍스처를 어떤 좌표계의 좌표축들에 정렬된 입방체의 여섯면으로 사용한다는 것이다. 입방체 텍스쳐가 축에 정렬되어 있으므로, 각 면은 좌표계의 각 주축 방향에 대응된다. 따라서 입방체의 특정 면을 지칭할 때에는 그 면과 교차하는 방향의 축(±X,±Y,±Z)을 언급하는 것이 자연스럽다. Direct3D는 입방체의 특정 면을 지칭하는 용도로 D3D11_TEXTURECUBE_FACE라는 열거형을 제공한다.
```c++
typedef enum D3D11_TEXTURECUBE_FACE{
	D3D11_TEXTURECUBE_FACE_POSITIVE_X = 0,
	D3D11_TEXTURECUBE_FACE_NEGATIVE_X = 1, 
	D3D11_TEXTURECUBE_FACE_POSITIVE_Y = 2, 
	D3D11_TEXTURECUBE_FACE_NEGATIVE_Y = 3, 
	D3D11_TEXTURECUBE_FACE_POSITIVE_Z = 4, 
	D3D11_TEXTURECUBE_FACE_NEGATIVE_Z = 5
}
```

2차원 텍스처와는 달리 입방체 맵에서는 한 텍셀을 2차원 텍스처 좌표로 지필 할 수 없다. 입방체 맵의 한 텍셀을 지칭하려면 3차원 텍스처 좌표가 필요하다. 이 3차원 텍스처 좌표는 원점에서 시작하는 하나의 3D 조회 벡터(lookpu vector) v를 정의한다. v의 3차원 좌표가 지칭하는 텍셀은 v 가 입방체 맵과 교파하는 곳의 텍셀이다. v 가 텍셀 표본들 사이의 한 점에서 교차하는 경우에 텍스쳐 필터링이 적용된다.

*remark : 조회 벡터의 크기는 중요하지 않다. 의미있는 것은 방향뿐이다. 방향이 같고 크기가 다른 두 벡터는 입방체 맵의 같은 지점에서 표본을 추출한다.

![[Pasted image 20221223153301.png]]
[간결함을 위해 2차원으로 표시. 그림의 정사각형은 어떤 좌표계의 중심에 놓이며 좌표축에 정렬된 하나의 입방체를 나타낸 것. 원점에서 벡터 v를 쏘았을 때 그것과 사각형 변(입방체의 면)이 만나는 지점의 텍셀이 바로 추출할 텍셀이다. 이 그림의 v는 +Y축에 해당하는 입방체의 면과 만난다.]

*조회 벡터 (lookup vector)는 cube map가 같은 공간에 있어야 한다.