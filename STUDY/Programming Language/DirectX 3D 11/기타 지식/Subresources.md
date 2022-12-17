Direct3D는 전체 리소스를 참조하거나 혹은 리소스의 일부을 참조할 수 있다. subresource(하위 리소스) 라는 용어는 리소스의 한 부분을 나타낸다.

하나의 버퍼는 하나의 subresource 로 정의된다. Texture는 mipmap level 및/ 또는 Texture array(텍스쳐 배열)을 지원하는 몇가지 다른 텍스쳐 유형을 가지고 있기 때문에 조금 더 복잡하다.  가장 간단한 경우를 들어 시작하면 1D texture 는 하나의 subresource 로 정의된다.
![[Pasted image 20221217180419.png]]

이것은 1D texture을 구성하는 텍셀 배열이 하나의 subresource 에 포함되어 있다는 것을 의미한다. 만약 1D texture을 3개의 mipmap level 로 확장한 다면, 그것은 다음과 같이 시각화 할 수 있다.

![[Pasted image 20221217181004.png]]

이것을 세개의 subresource 로 만들어진 하나의 텍스쳐로 생각을 해보자. 하나의 subresource 는 하나의 텍스쳐에 대한 Level-of-detail(LOD)을 사용하여 인덱싱 된다(<span style ="color:green">be indexed 가 무슨 의미일까</span>). 텍스쳐 배열을 사용할 때, 특정 subresource에 접근하는 것은 LOD와 특정 텍스쳐 둘다 필요하다. 또는 API는 이 두 가지 정보 조각을 아래 그림과 같이 보이는 하나의 zero-based subresource index 로 결합한다.

![[Pasted image 20221217192916.png]]

## Subresource 선별

어떤 API 는 전체 리소스에 접근하고 또 다른 API들은 리소스의 일부에 접근한다. 보통 리소스 일부에 접근하는 메서드는 엑서스 하기 위한 subresource를 특정하기 위해서 view description 을 사용한다. 


#### 다음의 섹션에서는 텍스쳐 배열에 접근할 때 view description 에서 사용되는 용어들을 보여준다.

### Array Slice

주어진 텍스쳐 배열, mipmap의 각 텍스쳐, an array Slice (하얀 사각형으로 표현된다.)은 하나의 텍스쳐와 그것의 모든 subresource들을 포함한다. 

![[Pasted image 20221217200100.png]]


### Mip Slice

mip slice (하얀 사각형으로 표현된다.)는 한 array 속 모든 텍스쳐에 대해서 하나의 mipmap level을 포함한다.


![[Pasted image 20221217201400.png]]


## Selecting a Single Subresource

다음과 같은 slice 의 두가지 유형을 사용하여 하나의 subresource 를 선택할 수 있다.

![[Pasted image 20221217201721.png]]

## Selecting Multyple Subresource

또는 다음 그림과 같이 몇 개의 mipmap Level 혹은 몇 개의 texture 로 묶는 두가지 유형의 slice 로 subresource 를 선택할 수 있다.

![[Pasted image 20221217202002.png]]


## Note

render-target view는 오직 단 하나의 subresource 혹은 mip slice 사용할 수 있다. 그리고 둘 이상의 mip Slice 의 subresource 를 포함할 수 없다. 즉, render-target view 안의 모든 텍스쳐는 같은 사이즈여야 한다. 하나의 shader-resource view 는 subresource의 어떤 직사각형 영역을 선택할 수 있다.

