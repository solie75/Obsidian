개념적으로 masking 은 어떠한 것을 선택적으로 무시한다는 것이다. 보통 어떠한 것을 분리하려할 때 masking한다.

다음의 두 가지 중심으로 사용된다.

1. Masking data such as bit fields or flags for bitwise operation.
2. Masking image, often for composition or other effects.

## image mask

컴퓨터 그래픽에서 배경 위에 위치하도록 의도된 이미지가 주어졌을 때 transparent(투명한) 영역은 binary mask 를 통해서 특정된다. 이 방법으로 , 각 의도된 이미지에 대해 거기에 실제로는 두 개의 비트맵이 있는 것이다. 실제 이미지에서 사용되지 않는 영역은 픽셀 값이 0 bit 로 설정되고 이미지 영역과 일치하는 추가 'mask' 는  0 bit 로 픽셀값이 주어지고 그 주위로는 1bit 값이 픽셀에 주어진다.  아래와 같이 검은색 픽샐은 0 bit 를, 하얀색 픽셀은 1 bit 를 갖는다. 
![[Pasted image 20230124151821.png]]

런타임 때에 이미지를 배경 위에 배치하려는 경우, 프로그램은 먼저 'Bitwise AND' 연산자를 사용하여 원하는 좌표에 image mask를 사용하여 화면 픽셀의 bit 를 masking 한다. 이렇게 하면 투명한 영역의 배경 픽셀들은 보존되고 겹치는 이미지로 인해 가려질 픽셀의 비트들은 0으로 리셋된다. 

그 다음 프로그램은 bitwise OR 연산자를 사용하여 이미지 픽셀의 bit와 배경 픽셀의 bit 를 결합하여 이미지 픽섹의 bit 를 랜더링한다. 이러한 방식으로 픽셀 주변의 배경을 유지하면서 이미지 픽셀이 적절하게 배치 된다. 그 결과 배경 위에 이미지가 완벽하게 합성된다. 

![[Pasted image 20230124153116.png]]

This technique is used for painting pointing device cursors, in typical 2-D videogames for characters, bullets and so on (the [sprites](https://en.wikipedia.org/wiki/Sprite_(computer_graphics) "Sprite (computer graphics)")), for [GUI](https://en.wikipedia.org/wiki/GUI "GUI") [icons](https://en.wikipedia.org/wiki/Icon_(computing) "Icon (computing)"), and for video titling and other image mixing applications.

Although related (due to being used for the same purposes), [transparent colors](https://en.wikipedia.org/wiki/Palette_(computing)#Transparent_color_in_palettes "Palette (computing)") and [alpha channels](https://en.wikipedia.org/wiki/Alpha_channel "Alpha channel") are techniques which do not involve the image pixel mixage by binary masking.
## reference

https://en.wikipedia.org/wiki/Mask_(computing)#Image_masks



