특정한 윈도우의 부모 혹은 소유자의 핸들을 검색한다.

## Syntax

```c++
HWND GetParent(
	[in] HWND hWnd 
);
```

## Parameters

HWND hWnd  : 부모 윈도우의 핸들을 검색할 윈도우의 핸들 값.

## Return value

type : HWND

만약 윈도우가 자식 윈도우라면, 반환값은 부모의 핸들 값이다. 만약 윈도우가 WS_POP 스타일의 최상위 윈도우라면 그 반환값은 소유자 윈도우의 핸들 값이 된다.
함수가 실패하면 그 반환값은 NULL 이다. 

이 함수는 다음의 이유로 보통 실패한다. 
- 윈도우가 소유자가 없거나 WS_POPUP 스타일이 아닌 최상의 레벨의 윈도우인 경우
- 소유자가 WS_POPUP 인 경우

## Remarks

GetParent 를 대신하여 윈도우의 소유자 윈도우를 얻기 위해서 GW_OWNER flag 를 가지고 GetWindow 를 사용한다. GetParent 사용 대신에 소유자 없이 부모 윈도우를 얻기 위해서는 GA_PARENT flag 를 가지고 GetAncestor 를 사용한다.

