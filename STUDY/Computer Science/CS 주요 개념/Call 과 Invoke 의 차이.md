A 라는 클라스에 b( ), c( ) 메소드가 있다고 가정할 때,

call 은 단순한 메서드 호출로, new b(); 식이다.

invoke 는 간접 호출로, A 클래스의 정보에서 메서드 목록을 가져온 다음 거기에서 b( )를 골라 호출하는 것
```c++
A a = new A();
Class cls = a.GetClass();
Method m = cls.getmethod("b", null);
Object result = method.invoke(a, null);
```
즉, 객체를 만들어서 그 객체의 클래스 정보를 이용해 b 메서드를 꺼내온 뒤 간접적으로 실행한다. 이 때 A클래스의 객체와 매개변수로 들어가는 것들을 넣고 메서드에 return 값이 있다면 그것을 result 로 받는다. 

trigger 는 특정 이벤트 발생 시 메서드가 실행되도록 하는 것으로 예를 들어 Member 테이블 insert 시 테이블 로그를 기록하는 프로시저를 실행하고 싶다면  Member 테이블의 insert 에 대한 트리거로 해당 프로시저를 지정하면 된다.