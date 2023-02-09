## 1. GetCursorPos ()

- 윈도우 창을 기준으로 하지 않고 기본 모니터 화면을 기준으로 좌표를 잡는다. 좌측 상단이 (0, 0)인 것은 동일하다.

#### Syntax
```c++
BOOL GetCursorPos( [out] LPPOINT lpPoint );
```

1.  lpPoint
커서의 화면 좌표를 대입 받을  [POINT](https://learn.microsoft.com/en-us/windows/win32/api/windef/ns-windef-point) 구조체를 가리키는 포인터

## 2. ScreenToClient ()

- 화면에서 지정된 지점의 화면 좌표를 작업 영역 좌표로 변환한다.

#### Syntax
```c++
BOOL ScreenToClient( [in] HWND hWnd, LPPOINT lpPoint );
```

1. hWnd
변환될 작업 영역을 가진 윈도우에 대한 handle.

2. lpPoint
작업 영역 기준으로 변경된 좌표 값을 대입받을 POINT 구조체를 가리키는 포인터.


## Remark

The function uses the window identified by the _hWnd_ parameter and the screen coordinates given in the [POINT](https://learn.microsoft.com/en-us/windows/win32/api/windef/ns-windef-point) structure to compute client coordinates. It then replaces the screen coordinates with the client coordinates. The new coordinates are relative to the upper-left corner of the specified window's client area.

The **ScreenToClient** function assumes the specified point is in screen coordinates.

All coordinates are in device units.

Do not use **ScreenToClient** when in a mirroring situation, that is, when changing from left-to-right layout to right-to-left layout. Instead, use [MapWindowPoints](https://learn.microsoft.com/en-us/windows/desktop/api/winuser/nf-winuser-mapwindowpoints). For more information, see "Window Layout and Mirroring" in [Window Features](https://learn.microsoft.com/en-us/windows/desktop/winmsg/window-features).

## Referece

https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-screentoclient