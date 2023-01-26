In the field of computer graphics, an anisotropic surface changes in appearance as it rotates about its geometric notmal, as is the case with velvet.
Anisotropic filtering(AF) is a method of enhancing the image quality of textures on surfaces that are far away and steeply angled with respect to the point of view. Older techniques, such as bilinear and trilinear filering, do not take into account the angle a surface is viewed from, which can result in aliasing or blurring of texture. By reducing detail in one direction more than another, these effects can be reduced easily.

## Anisotropy filtering

비등방성 뜻은 방향에 따라 물체의 물리적 성질이 다르게 변화하는 현상을 나타내는 말로, 3D 그래픽에서는 텍스쳐를 다양한 화면에서 봐야하기 때문에 이러한 비등방성이 항상 일어난다고 보면 된다. 

## 원리

밉 매핑의 기반은 텍스쳐가 축소되는 정도에 따라 미리 다운스케일된 밉맵을 사용해 안티에일리어싱의 효과를 얻는 것이다. 그러나 3D 그래픽 에서는 텍스쳐가 시선에 항상 수직인 것이 아니므로, 비스듬하게 위치한 텍스쳐의 경우 가로축과 세로축이 축소된 정도가 달라질 수 밖에 없다. 이중선형 필터링이나 삼선형 필터링에서는 이런 경우에도 하나의 값만을 이용해서 밉맵을 사용하지만, 그러지 않고 <span style="color: yellow">가로축과 세로축 각각에 대해 밉맵을 따로 만들어 두는 것이 비등방성 필터링이다.</span>

예를 들어 본래 텍스쳐의 크기가 1024x1024 라고 하면, 보통의 밉맵은 512x512, 256x256, 128x128, ... 이렇게 전 단계의 두 배 작아지는 형식이다. 따라서 가로축과 세로축이 1/2배로 축소된 경우에는 위의 보통의 밉맵과 같이 쓸것이고 아무런 문제가 없다. 그러나 가로축은 그대로인데 세로축만 1/4 로 축소된 경우(멀리 있는 바닥 텍스쳐 등)에는 512x512 밉맵 이나 256x256 밉맵을 써야만 하며, 어느 경우든 가로축의 해상도는 떨어져 보일것이다.
	따라서 가로축은 그대로이고 세로축만 1/4 로 축소된, 1024x256 밉맵을 미리 만들어 두는 생각을 해볼 수 있다. 이렇게 하면 세로축의 에일리어싱은 없으면서도 가로축의 해상도는 1024 그대로를 유지할 수 있다. 
	이렇게 1024x512, 1024x256, 1024x128 과 같이 세로 축이 축소되는 밉맵과 512x1024, 256x1024, 128x1024 와 같이 가로축이 축소되는 밉맵 등등을 모두 만들어 놓으면, 가로축과 세로축이 축소된 정도 각각에 따라 적절한 밉맵을 사용할 수 있다. 이것이 비등방성 필터링이다.

## 효과

멀리 있으면서 시선에 비스듬한 물체의 텍스쳐 선명도가 비약적으로 상승한다. 텍스처의 가로축과 세로축의 축소 비율이 크게 차이나는 경우 많이 축소된 쪽에서 에일리어싱이 일어나거나 ,또는 덜 축소된 쪽의 해상도가 떨어질 수 밖에 없는 이중선형, 삼선형 필터링과는 달리 축소된 쪽은 축소된만큼 다운스케일하여 안티에일리어싱이 되고 덜 축소된 쪽의 해상도는 그대로 남아 있으므로 선명하게 볼 수 있다. 비율 차이가 클 수록 , 븍 거리가 멀고 시야에 비스듬할 수 록 삼선형 필터링과의 차이는 커진다.