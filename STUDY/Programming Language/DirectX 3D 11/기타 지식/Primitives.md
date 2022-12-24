3D primitive 은 3D entity의 형식을 한 정점의 모음(collection) 이다. 가장 간단한 primitive는 point list 라고 불리는 3D 좌표 시스템의 점들의 collection 이다. 

자주, 3D primitive 는 폴리곤이다. 하나의 홀리곤은 적어도 세개의 정점으로 그려진 3D figure에 가깝다. 가장 가단한 폴리곤은 삼각형이다. Microsoft Direct3D 는 삼각형 안의 모든 세개의 정점들은 평면상에서 생성되기 때문에 삼각형들을 그것의 폴리곤들을 구성하는데 사용한다. (-> 삼각형은 무조건 하나의 면만을 갖기 때문에 Direct3D 는 모든 폴리곤은 잘게 쪼갠 삼각형으로 구성한다.)

다른 평면산의 정점을 렌더링하는 것은 비효율적이다. 삼각형은 크고 복잡한 폴리곤과 메쉬를 형성하는데 구성으로 쓰일 수 있다.

다음 그림은 cube 를 보여준다. 두개의 삼각형이 각 cube 의 면을 구성한다. 전체적인 삼각형 세팅은 하나의 cubic primitive를 형성한다. single solid form 으로 나타내기 위해 primitives의 surfaces에 texture와 material 을 지원한다. 


![[Pasted image 20221224183434.png]]

또한 부드러운 곡선을 나타내는 surface 인 pricitives 를 생성하는데 삼각형을 사용할 수 있다. 다음의 그림은 어떻게 구가 삼각형으로 이루어 지는 지 보여준다. material가 적용된 후, 그 구는 렌더링 될 때 곡선과 같이 보인다. 이것은 특히 Gouraud shading을 사용하면 그러하다. 

![[Pasted image 20221224184031.png]]

Direct3D device 는 primitive 의 다음의 유형들을 생성하고 다룬다.

### Point Lists
https://learn.microsoft.com/en-us/windows/win32/direct3d9/point-lists
### Line Lists
https://learn.microsoft.com/en-us/windows/win32/direct3d9/line-lists
### Line Strips
https://learn.microsoft.com/en-us/windows/win32/direct3d9/line-strips
### Triangle Lists
https://learn.microsoft.com/en-us/windows/win32/direct3d9/triangle-lists
### Triangle Strips
https://learn.microsoft.com/en-us/windows/win32/direct3d9/triangle-strips
### Triangle Fans
https://learn.microsoft.com/en-us/windows/win32/direct3d9/triangle-fans
