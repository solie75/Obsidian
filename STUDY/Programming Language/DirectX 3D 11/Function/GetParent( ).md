IDXGIObject::GetParent( )

객체의 부모를 갖는다.

## Syntax

```c++
HRESULT GetParent(
	[in] REFIID riid,
	[out] void **ppParent
);
```

## Paramenter

1. REFIID riid : 요청된 인터페이스의 ID.
2. 부모 객체를 가리키는 포인터의 주소.

## Return value

type : HRESULT