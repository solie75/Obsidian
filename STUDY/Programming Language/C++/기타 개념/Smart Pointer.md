c++ 은 메모리 할당 및 해제로 메모리 관리를 직접 하기 때문에 메모리가 해제되지 않을 수 있고 이러한 메모리 릭은 문제가 된다. 이를 방지하기 위해 메모리를 자동으로 해제해 주는 클래스 템플릿인 스마트 포인터를 사용한다.

1. shared_ptr

```c++
// 생성

std::shared_ptr<자료형> 스마트포인트명(new 자료형(값));
std::shared_ptr<자료형> 스마트포인트명 = std::make_shared<자료형>(값);

make_shared() 함수는 전달받은 인수를 활용하여 지정된 객체를 생성하고 생성된 객체를 참조하는 shared_ptr 을 반환하기 때문에 shared_ptr 객체를 예외발생에 대해 안전하게 생성가능하다.
```

하나의 객체 (하나의 메모리 영역) 을 여러 스마트 포인터가 참조할 때 사용하는 것으로 해당 메모리 영역을 참조하는 shared_ptr가 늘어날 때마다 참조 카운트(reference count)가 씩 증가하고 이 카운트를 각 shared_ptr가 공유한다. 각 shared_ptr가 수명을 다할 때 참조 카운트가 1씩 감소하며 마지막 shared_ptr가  수명을 다하거나 main 함수가 종료되면 참조 카운트가 0이 되어 delete 를 사용해 메모리를 자동으로 해제한다.

![[Pasted image 20231103122211.png]]

2. unique_ptr
객체(메모리 영역) 에 대해서 하나의 스마트 포인터 만이 가리킬 수 있도록 할때 사용한다.

```c++
// 생성

std::unique_ptr<int> UP(new int(5));
std::unique_ptr<int> UP = std::make_unique<int>(4);
```

다른 unique_ptr 을 선언한 뒤 새로 선언한 unique_ptr 에 Up 을 대입하려고 하면 에러가 발생한다.

따라서 unique_ptr 이 가리키는 하나의 메모리 영역에 대해 unique_ptr 을 변경하고 싶다면 move() 함수를 이용한다.

```c++
std:unique_ptr<int> UpTest = std::make_unique<int>(6);
UpTest = std::move(UP);

UpTest 를 출력해보면 4가 출력된다.
6이 기록된 메모리를 가리키던 UpTest 가 move() 호출 후에 UP 이 가리키던 4가 기록된 메모리를 가리키기 때문이다.
```

3. weak_ptr
shared_ptr 에 대입된 객체를 참조하되 참조 카운트를 증가하고 싶지 않을 때 사용한다.
서로를 가리키는 shared_ptr 은 참조카운터가 0이 될 수 없으므로 메모리가 해제되지 않는 순환 참조(circular reference) 가 발생한다. weak_ptr 은 이러한 순환참조를 제거하기 위해 사용된다.