IUnknown 인터페이스는 QueryInterFace 메서드를 통해서 주어진 오브젝트의 다른 인터페이스를 가리키는 포인터를 얻기 위해서 client 를 사용할 수 있다. 그리고 AddRef 와 Release 메서드를 통해서 Object 의 존재에 관래 조정할 수 있다. 모든 CON interface 들은 직접적이든 간접적이든 IUnknown 을 상속 받는다. 따라서 IUnknown의 세 개의 메서드는 모든 interface에 대한 vtable 에서 첫번째 순서이다.

# Reference

https://learn.microsoft.com/en-us/windows/win32/api/unknwn/nn-unknwn-iunknown