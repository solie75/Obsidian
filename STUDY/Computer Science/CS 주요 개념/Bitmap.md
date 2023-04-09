컴퓨터의 화면은 하나의 점들이 모여 2차원의 형식을 이루고 데이터로 관리 되고 있다.

흑백영상에 대하여 가로 16 세로 5의 픽셀이 있다고 할때 
각 하나의 픽셀을 0또는 1로 채울 수 있고 이것을 <span style="color:orange">메모리 비트 패턴 즉 비트 맵이라 한다.</span>

## DDB

프로그래밍 언어 상에서 위의 가로 16, 세로 5의 비트 맵의 경우 하나의 횡을 2개의 16진법(1byte 당 16진법 1개)  으로 표시 하고 이것을 일렬로 메모리에 저장한다. 이때 16 * 5 해상도의 흑백 영상을 지원하는 그래픽카드 에서는 일렬로 나열된 데이터가 자신의 장치가 처리하는 데이터 형식과 일치하여 문제가 없다. 이렇게 <span style="color:orange">비트맵이 장치에 종속적인 형태(장치에서 별다른 변환작업 없이 바로 사용할 수 있는 형태)로 나열 되어 있는 경우, 이런 비트맴을 ' DDB(Device Dependent Bitmap)' 이라 한다.</span> 

## DIB

<span style="color:orange">비트맵의 데이터와 그래픽 장치의 지원하는 데이터 형식이 일치 하지 않을 때</span>, 단순히 비트의 화면을 구성하는 '비트의 패턴'만 필요한 것이 아니라 해당 비트맴에 대한 정보(비트맵의 크기, 하나의 픽셀에 주어질 데이터 크기 등) 또한 있어야 한다. 때문에 <span style="color:orange">실제 '비트의 패턴' 앞에 이 비트맵의 구성 정보를 담고 있는 '비트맵 헤더(Bitmap Header)'를 추가로 제공해야 한다.</span> 
이렇게 장치가 지원하는 데이터 형식과 다르더라도 <span style="color:orange">'비트맵 헤더'에 있는 정보를 보고 해당 장치에 맞도록 변화해서 사용하는 이런 비트맵은</span> 특정 장치에 종속되지 않기 때문에 <span style="color:orange">DIB(Device-independent Bitmap)이라 한다.</span> 

## RGB 및 투명도

하나의 픽셀에 대하여 색을 정할 때 총 32비트를 기준으로 Red, Green, Blue에 각 1byte 씩 배분하고 0~255 값을 사용하여 농도로 색상을 표현한다. 그리고 남은 1byte 에는 투명도(Alpha)를 명시한다. 이때 0이면 완전히 투명하여 겹쳐있지만 밑에 있는 것으로 분류되어 있는 다른 윈도우의 모습이 그대로 보이고  1이면 해당 픽셀의 완전한 색상이 보인다.

## 용량제한

BMP 파일1개의 크기가 4GiB 이상이 되도록 저장할 수 없다. 파일 헤더에서 자체 파일 크기를 적을 수 있는 길이가 4바이트 밖에 되지 않기 때문.

## 헤더의 구조

BMP 파일을 16진수 편집기를 통해 보면 어떤 정체를 알 수 없는 숫자들이 맨 처음에 나열되어 있는데, 어떤 파일이든 처음 몇몇 숫자들의 구조는 같다는 공통점이 있다. 이를 헤더라고 한다.
https://namu.wiki/w/BMP(%ED%99%95%EC%9E%A5%EC%9E%90)

## 비트 할당법

8비트 까지는 그래픽 툴에서 특정 색을 직접 지정하거나 운영체제에서 정한 "팔레트"를 사용하는데, 이를 인덱스 컬러(Indexed color) 라고 한다. 그러다가 16비트 이상의 이미지부터 비트를 적당히 3등분하여 사용하게 된다.
-   **16비트:** R5 G5 B5 X1 or R5 G6 B5
-   **24비트:** R8 G8 B8
-  **32비트:** A8 R8 G8 B8 (A는 알파)
## 32비트맵의 메모리 구성

32 비트 색상을 지원하는 비트맵은 4바이트가 사용되기 때문에 바이트 정렬을 고려해야 한다. 장치에 따라서 투명도 , Red, Green, Blue 순서가 달라질 수 있다. 
<span style="color:orange">Windows OS 에서는 32비트 색상 값이 'Alpha, R, G, B' 순</span>으로 명시된다.

예시 : Alpha 0xff, Red 0x50, Green 0x70, Blue 0x90 이라면
```c++
unsigned int pixel_color = 0xff507090; // 투명도 , R, G, B 순서
```
이와 같이 소스 코드를 구성하게 된다. 

하지만 Windows OS를 실행하는 보통의 CPU는 Little Endian 으로 되어 있기 때문에 메모리에는 역순으로 저장되어 있다. 즉, 메모리 배열상으로는 Blue값이 제일 먼저 있다는 뜻이다.(B, G, R, Alpha)

따라서 포인트 문법을 사용해서 한 바이트 씩 읽는 경우는 색상 순서에 대해서 주의해야 한다.

## 비트 맵 생성

- CreateBitmap - DDB 형식의 비트맵을 직접 생산하는 함수.
```c++
HBITMAP CreateBitmap(int nWidth, int nHeight, UINT nPlanes, UINT nBitCount, CONSTvoid* lpBits);
```

1. nWidth : 생성할 비트맵의 폭.
2. nHeight : 생성할 비트맵의 높이.
3. nPlanes : 16색상을 사용하는 비트맵에서는 4개의 플랜, 대부분의 색상은 1개의 플랜 사용 따라서 '1이라고 명시'
4. nBitCount : 색상을 표현하는데 필요한 비트 수
5. lpBits : 초기 비트 패턴을 설정할 때 사용한다. 이 매개 변수에 전달되는 메모리는 생성할 비트맵이 사용할 메모리 크기와 일치해야 하기 때문에 'nWidth * nHeight * nBitCount /8' 의 크기를 가져야 한다. 또한 RGB 값이 나열되어 있어야 한다. 이 값이 NULL 이면 생성된 비트맵은 전체가 검은색(RGB 가 0, 0, 0)이다.

## 비트맵 제거

```c++
BOOL DeleteObject(HGDIOBJ ho);
```
Deleteobject 함수는 비트맵 뿐만 아니라 GDI Object를 제거할 때 또한 사용할 수 있다.
이 함수가 갖는 하나의 매개변수에 제거하고 싶은 GDI Object의 핸들을 명시하면 된다. 
```c++
HBITMAP h_my_bmp = CreateBitmap(16, 8, 1, 32, NULL);
DeleteObject(h_my_bmp);
```

## CreateBitmap 사용시 주의사항

CreateBitmap 함수는 그래픽 장치에 종속된 DDB 형식의 비트맵을 생성하기 때문에 <span style="color:orange">4번재 인자인 색상 수에 특정 값을 고정하여 사용하면 프로그램의 호환성이 떨어지게 된다.</span> ( 그래픽 장치마다 지원 색상수가 다른 경우, 운영체제 설정에서 색상 수 설정이 다르게 된 경우 등)  따라서 현재 장치가 어떤 색상 수를 사용하는지 얻어서 사용하는 것이 좋다. (이때 현재 프로그램에서 사용하는 그래픽 장치에 대한 속성은 DC(Device Context)가 가지고 있다.) 
```c++
// 화면의 색상수를 얻는다. h_dc는 현재 위도우의 dc 핸들 값.
int color_depth = ::GetDeviceCaps(h_dc, BITSPIXEL);
HBITMAP h_my-bmp = CreateBitmap(16, 8, 1, color_depth, null);
```

## CreateCompatibleBitmap

사용중인 DC를 전달해서 비트맵을 생성한다. (비트 플랜의 개수, 생상 수 등이 불필요)
```c++
HBITMAP CreateCompatibleBitmap(HDC hdc, int nWidth, int nHeight);
// 첫번째 인자가 NULL 이면 현재 윈도우가 사용하는 기본 DC 속성을 가지고 비트맵을 생성한다.
```

## 운영체제와 비트맴 생성의 과정

<span style="color:orange">Windows 운영체제는 비트맴 생성에 대한 요청이 발생하면 비트맵을 생성하고 응용프로그램에는 HBITMAP 형식으로 핸들 값을 전달해 준다.</span>  그리고 해당 비트맵을 사용하고 싶을 때전달 받은 핸들 값으로 비트맵 관련 함수를 호출한다. 이렇게 비트맵을 운영체제가 직접 관리하기 때문에 비트맵의 '비트패턴'을 직접 조작할 수가 없다.

## GetBitmapBits

비트맵의 '비트 패턴'을 복사해오는 함수
```c++
LONG GetBitmapBits(HBITMAP hbmp, LONG cbBuffer, LPVOID lpvBits);
```
1. hbmp : '비트 패턴'을 복사할 비트맵의 핸들 값.
2. cbBuffer : 복사할 바이트 수(폭 * 높이 * 색상 수). 이 값이 실제 비트 패턴 보다 크더라도 딱 실제 비트 만큼만 복사. GetObject()함수로 폭, 높이, 색상 수 를 알수 있다. 
3. lpvBits : '비트 패턴'이 복사될 메모리의 주소를 명시.
4. 반환 값 : 실제로 복사한 '비트 패턴'의 크기가 반환. 비트 패턴 복사에 실패하면 0을 반환.

## SetBitmapBits

비트맵에 새로운 비트 패턴을 설정하는 함수. 현재 SetDIBits 함수로 대체

```c++
LONG SetBitmapBits(HBITMAP hbmp, DWORD cb, const VOID *pvBits*)
```
1. hbmp : '비트 패턴'을 설정할 비트맵의 핸들 값.
2. cb : 설정할 바이트 수
3. pvBits : 새로운 '비트 패턴'이 저장된 메모리의 주소.

```c++
HBITMAP h_bitmap = CreateBitmap(300, 200, 1, 32, NULL);

unsigned int* p_pattern = new unsigned int[300*200];

for(int i = 0; i < 300*200; ++i)
{
	*(p_pattern + i) = 0xFF0000FF;
}

int copy_size = SetBitmapBits(h_bitmap, 300*200*4, p_pattern);

delete[] p_pattern;
DeleteObject(h_bitmap);
```

## BitBlt

지정된 소스 장치 컨텍스트에서 대상 장치 컨텍스트로 픽셀의 사각형에 해당하는 색상 데이터의 비트 블록 전송을 수행한다.

```c++
BOOL BitBlt(
  [in] HDC   hdc,
  [in] int   x,
  [in] int   y,
  [in] int   cx,
  [in] int   cy,
  [in] HDC   hdcSrc,
  [in] int   x1,
  [in] int   y1,
  [in] DWORD rop
);
```
	여기에서 첫번째 매개변수는 대상 장치 컨텍스트에 대한 핸들이다. 6번째 매개변수는 소스 장치 컨텍스트에 대한 핸들이다. 9번째 매개변수는 래스터 작업 코드이다. 그중 'SRCCOPY'는 원본 사각형을 대상 사각형에 직접 복사한다.

