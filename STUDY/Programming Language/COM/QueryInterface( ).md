## Syntax

```c++
HRESULT QueryInterface( REFIID riid, void **ppvObject );
```

## Parameters
1. REFIID riid : 요구되고 있는 인터페이스의  interface identifier(IID)에 대한 참조.
2. void **ppvObject : riid parameter 에 명시된 IID 가 있는 인터페이스에 대한 포인터의 주소이다. 인터페이스 포인터의 주소를 전달하기 때문에, 메서드는 포인터의 주소를 쿼리중인 인터페이스에 덮어쓸 수 있다.


. Because you pass the address of an interface pointer, the method can overwrite that address with the pointer to the interface being queried for.
. 인터페이스 포인터의 주소를 전달하기 때문에 메서드는 해당 주소를 쿼리 중인 인터페이스에 대한 포인터로 덮어쓸 수 있습니다.