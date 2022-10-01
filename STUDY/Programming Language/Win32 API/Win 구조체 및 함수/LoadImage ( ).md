아이콘, 커서 , 애니메이션 커서 또는 비트맵을 로드한다.

```c++
HANDLE LoadImageA( 
	[in, optional] HINSTANCE hInst,
	[in] LPCSTR name,
	[in] UINT type,
	[in] int cx,
	[in] int cy,
	[in] UINT fuLoad 
);
```
1. HINSTANCE hInst
	1. 로드할 이미지가 포함된 DLL  또는 실행파일(.exe)의 모듈에 대한 핸들
	2. OEM 이미지 , 독립 실행형 리소스 (아이콘, 커서 또는 비트맵 파일)를 로드하려면 NULL 로 설정한다.
2. LPCSTR name
	1. 로드할 이미지
3. UINT type
	1. 로드할 이미지 유형
4. int cx
	1. 너비, 이 매개변수가 0이고 LR_DEFAULTSIZE가 사용되지 않으면 함수는 실제 자원 너비를 사용한다.
5. int cy
	1. 높이, 위와 같다.
6. UINT fuLoad
	1. 여러 값에 따라 의미하는 바가 다르다.
	2. LR_CREATEDIBSECTION : uType매개변수가 IMAGE_BITMAP 을 지정하면 함수가 호환되는 비트맵이 아닌 DIB 섹션 비트맵을 반환한다.
	3. LR_DEFAULTSIZE : csDesired 또는 cyDesired 값이 0으로 설정된 경우 커서 또는 아이콘에 대해 시스템 메트릭 값으로 지정된 너비 또는 높이를 사용한다. 이 플래스가 지정되지 않고 csDesired 및 cyDesired가 0으로 설정되면 함수는 실제 리소스 크기를 사용한다.

- 반환값
	- 함수가 성공하면 새로 로드된 이미지의 핸들을 반환한다. 실패하면 NULL 반환