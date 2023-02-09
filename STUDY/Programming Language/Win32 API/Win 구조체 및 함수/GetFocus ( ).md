창이 호출 스레드의 메시지 큐에 연결된 경우 키보드 포커스가 있는 창에 대한 핸들 검색.

## Syntax
```c++
HWND GetFocus();
```

## Return value

HWND, 반환 값은 키보드 포커스가 있는 창에 대한 핸들. 호출 스레드의 메시지 큐에 키보드 포커스가 있는 연결된 창이 없으면 반환 값은 NULL 이다.

## Remark

**GetFocus** returns the window with the keyboard focus for the current thread's message queue. If **GetFocus** returns **NULL**, another thread's queue may be attached to a window that has the keyboard focus.

Use the [GetForegroundWindow](https://learn.microsoft.com/en-us/windows/desktop/api/winuser/nf-winuser-getforegroundwindow) function to retrieve the handle to the window with which the user is currently working. You can associate your thread's message queue with the windows owned by another thread by using the [AttachThreadInput](https://learn.microsoft.com/en-us/windows/desktop/api/winuser/nf-winuser-attachthreadinput) function.

To get the window with the keyboard focus on the foreground queue or the queue of another thread, use the [GetGUIThreadInfo](https://learn.microsoft.com/en-us/windows/desktop/api/winuser/nf-winuser-getguithreadinfo) function.