```c++
BOOL AlphaBlend(
	HDC hdcDest,
	int soriginDest,
	int yoriginDest,
	int wDest,
	int hDest,
	HDC hdcSrc,
	int xoriginSrc,
	int yoriginSrc,
	int wSrc,
	int hSrc,
	BLENDFUNCTION ftn
);
```

- 매개변수
	1. hdcDest : 대상 장치 컨텍스트에 대한 핸들'
	2. xoriginDest : 대상 사각형의 왼쪽 위 모서리에 대한 좌표의 x값.
	3. yoriginDest : 대상 사각형의 왼쪽 위 모서리에 대한 좌표의 y값.
	4. wDest : 대상 사각형의 너비(논리 단위)
	5. hDest : 대상 사각형의 높이(논리 단위)
	6. hdcSrc : 소스 장치 컨텍스트에 대한 핸들
	7. xoriginSrc : 소스 사각형의 왼쪽 위 모서리에 대한 좌표의 x 값.
	8. yoriginSrc : 소스 사각형의 왼쪽 위 모서리에 대한 좌표의 y값.
	9. wSrc : 소스 사각형의 너비(논리 단위)입니다.
	10. hSrc : 소스 사각형의 높이(논리 단위)입니다.
	11. ftn : 소스 및 대상 비트맵에 대한 알파 혼합 기능, 전체 소스 비트맵에 적용할 전역 알파 값, 소스 비트맵에 대한 형식 정보. 소스 및 대상 혼합기능은  현재 AC_SRC_OVER로 제한된다.

- 반환 값
	- 함수가 성공하면  True 를 실패하면 False 를 반환

- 주의
	- AlphaBlend에서 사용되는 bmp 파일은 32비트에 알파 채널을 가지고 있어야 한다.