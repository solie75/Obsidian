## Syntax

```c++
HRESULT QueryInterface( REFIID riid, void **ppvObject );
```

## Parameters
1. REFIID riid : 요구되고 있는 인터페이스의  interface identifier(IID)에 대한 참조.
2. void **ppvObject : riid parameter 에 명시된 IID 가 있는 인터페이스에 대한 포인터의 주소이다. 인터페이스 포인터의 주소를 전달하기 때문에, 메서드는 포인터의 주소를 쿼리중인 인터페이스에 덮어쓸 수 있다.

비록 하나의 객체가 (인스턴스화 되기 전에) 정적으로 제공하는 기능을 표현할 수 있는 매커니즘이 있다 하더라도, 근본적인 COM 매커니즘은 QueryInterface 로 불리는 'IUnknown' 메서드 를 사용한다. <span style="color: yellow">모든 인터페이스는 IUnknown 으로부터 파생되기에 모든 인터페이스는 QueryInterface의 구현이 있어야 한다. 구현에 관계없이 이 메서드는 호출자가 원하는 포인터를 가진 인터페이스의 IID를 사용하여 객체를 쿼리한다.</span> 만약 그 객체가 그 인터페이스를 지원한다면, QueryInterface 는 인터페이스에 대한 포인터를 검색하는 동시에 또한 AddRef 를 호출한다. 그렇지 않으면 E_NOINTERFACE 에러 코드를 반환한다.

사용자는 반드시 Reference Counting 규직을 언제나 준수해야 한다는 것을 주의해야 한다. 만약 사용자가 참조 카운트를 0으로 감소시키기 위해서 인터페이스 포인터에 Release 를 호출하였다면, 그 포인터는 다시 사용할 수 없다. 가끔 객체에 대한 약한 참조를 얻을 필요가 있을 수 있지만(당신이 포함하기를 원하는 포인터가 참조 카운트 증가가 없는 인터페이스 중에 하나이기를 바란다면) queryInterface 에 이어서 Release 가 호출되게 함으로서 받아들여지지 않게 한다. 