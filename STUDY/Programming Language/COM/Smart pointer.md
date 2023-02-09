COM 객체를 위한 스마트 포인터.

포인터를 선언(스택) 하고 동적 할당(힙) 한 경우. 스택에 존재하는 포인터 변수는 힙에 존재하는 데이터의 조수를 가리킨다. 
이 때 포인터 변수를 잘못 관리하면 스택에서는 해제가 되겠지만 힙에 데이터가 남아 있을 수 있다.(메모리 릭)
따라서 스틱에서 해제가 될때 힙의 데이터도 자동으로 해제하게 끔 프로그래밍 한 것이 스마트 포인터이다. 즉, 할당과 해제를 자동으로 관리해 준다.

일반 포인터 선언 및 사용
```c++
// 포인터 선언
ID3D11Devcie* device;

// 포인터 사용.
device // 포인터
&device // 포인터의 주소

*device // 접근
```

스마트 포인터 사용
```c++
#include <wrl.h>

using Microsoft::WRL::ComPtr;

// 스마트 스토어 선언
ComPtr<ID3D11Device> device;

// 스마트 스토어 사용
device.get(); // 포인터
device.getAddressOf(); // 포인터의 주소

*device // 접근
```


## Reference

https://junstar92.tistory.com/180