# <\iostream> std::cout, std::cin, std::endl 과 <stdio.h> printf 의 차이.

###  <\iostream> 의 경우 

- C 에서 사용불가, C++ 에서만 사용가능.
- 파일 입출력(filestream) 과 문자열 입출력(stringstream) 등이 표준 입출력과 동일한 인터페이스를 사용한다.
- 타입을 지정하지 않는다. (ex. 1 과 1.0 무엇을 출력하든, 자동으로 맞는 자료형 int, double 을 호출한다.)
- C 의 stdio buffer 에 동기화 하는 과정에서 딜레이가 발생한다. (stdio.h 보다 느리다.)

### <stdio.h> 의 경우 

- C 와 C++ 둘다 사용 가능하다. 
- 입출력 시, 입력 또는 출력 받는 변수의 타입을 사용자가 지정해주어야 한다.
```c++
printf("%d", 1) // compile : O, Error : X
printf("%f", 1) // compile : O, Error : O
```
-> 컴파일러가 인식하지 못하지만 런타임 시에 Double 타입이 1 을 인식하지 못해 Error 가 발생한다.

- C 에서 시작된 함수이기 때문에 C 의 stdio buffer 에 동기화 할 필요가 없어 iostream 의 기능보다 빠르다.
