이미지의 특정 색을 투명하게 적용시켜 화면에 출력해주는 함수.

- 원형

```c++
BOOL TransparentBlt(

    HDC hdcDest,
    int xoriginDest,
    int yoriginDest,
    int wDest,
    int hDest,
    HDC hdcSrc,
    int xoriginSrc,
    int yoriginSrc,
    int wSrc,
    int hSrc,
    UINT crTransparent);
```

![[Pasted image 20221004111754.png]]

- hdcDest
이미지를 출력할 위치의 핸들입니다.

- xoriginDest, yoriginDest
이미지를 출력할 위치인 x, y 좌표입니다.

- wDest, hDest
출력할 이미지의 너비, 높이입니다.
이 크기만큼 **hdcSrc**로부터 받은 이미지를 이 너비와 높이에 맞게 이미지 크기를 변경합니다.

- hdcSrc
이미지의 핸들입니다.

- xoriginSrc, yoriginSrc
가져올 이미지의 시작지점인 x, y좌표입니다.
지정한 위치부터 **wSrc**, **hSrc**만큼 이미지를 잘라옵니다.

- wSrc, hSrc
원본 이미지로부터 해당 크기만큼 잘라낼 이미지의 너비, 높이입니다.
이 크기만큼 원본 이미지에서 잘라내어 **wDest**, **hDest**의 크기에 맞게 이미지 크기를 변경합니다.

crTransparent
투명하게 할 RGB 색상입니다.
**RGB(255, 0, 255)** 이렇게 색상을 지정합니다.