## SelectObject ( )

SelectObject 함수는 DC에 저장된 GDI Object의 핸들 값을 변경할 때 사용한다. 

```c++
HGDIOBJ SelectObject(HDC _dc, HGDIOBJ hgdiobj)
```
hdc : GDI Object를 교페할 DC의 핸들 값.
hgdiobj : 새롭게 설정할 GDI Object의 핸들 값.

bitmap의 경우에는 DC가 메모리 DC 인 경우에만 교체가 가능하다.

## 반환 값

이 함수는 GDI Object 교체 작업이 성공되면 DC가 이전에 사용하고 있던 GDI Object 의 핸들 값(HGDIOBJ 자료형)을 반환하고 실패하면 NULL 값을 반환한다.

## 사용예시

```c++
HDC h_dc = GetDC(m_hWnd) // 윈도우에서 사용할 DC를 생성
HBRUSH h_brush = CreateSolidBrush(RGB(0, 0, 255)); // 파란색 brush GDI Object 생성
HGDIOBJ h_old_brush = SelectObject(h_dc, h_brush); // 새로 생성된 Brush로 교체

Rectangle(h_dc, 50, 50, 100, 100); // 파란색으로 채워지는 사각형

SelectObject(h_dc, h_old_brush); // 기존에 사용하는 Brush 복구
DeleteObject(h_brush); // 파란색 Brush 객체 제거
ReleaseDC(m_hWnd, h_dc); // DC 제거
```

## 형변환 주의

HGDIOBJ 자료형이 HBITMAP, HPEN, HBRUSH, HFONT 의 상위 자료형처럼 처리된다.

다음과 같이 사용이 가능하다.
```c++
HBRUSH h_brush = CreateSolidBrush(RGB(0, 0, 255));
HGDIOBJ h_gdiobj = h_brush; // HBRUSH 형식을 HGDIOBJ에 대입가능
```

다음의 반대의 경우는 오류가 발생한다. 
```c++
HBRUSH h_brush = CreateSolidBrush(RGB(0, 0, 255));
HGDIOBJ h_gdiobj = h_brush; // HBRUSH 형식을 HGDIOBJ에 대입가능
HBRUSH check = h_gdiobj; //  오류발생!!
```

이를 해결하기 위해서는 아래와 같이 정확한 핸들 자료형으로 변환하여야 한다.
```c++
HBRUSH check = (HBRUSH)h_gdiobj;
```

SelectObject 형변환

```c++
HDC h_dc = GetDC(m_hWnd);
HBRUSH h_brush = CreateSolidBrush(RGB(0, 0, 255));
// hBrush h_old_brush = selectObejct(h_dc, h_brush); // 오류발생
hBrush h_old_brush = (HBRUSH)selectObejct(h_dc, h_brush);
```
SelectObject의 반환 값이 HGDIOBJ 자료형이기 때문에 형변환을 해주어야 한다.