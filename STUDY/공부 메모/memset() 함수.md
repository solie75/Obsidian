- 메모리를 할당받은 변수의 공간은 쓰레기 값이 남아 있다. 이러한 쓰레기 값들을 없애기 위해서 사용할 수 있는 방법 중 하나.

- 메모리의 내용을 원하는 크기만큼 특정값으로 설정할 수 있다.

- 동적이나 정적으로 생성한 배열을 초기화 할때 자주 사용한다.

```c++
#include <string.h> // c
#include <cstring.h> // c++


void* memset(void* ptr, int value, size_t num);
```

- 인자 값
ptr : 메모리의 크기를 변경할 포인터.
value : 초기화 값.
num : 초기화 크기 반환 값.

- 반환 값
성공 시 ptr 을 반환하고, 실패 시 NULL 을 반환.

- 예제
```c++
#include <stdio.h>
#include <string.h>

int main() {
	int arr[5];
	for (int i = 0; i < 5; i++){
		printf("[%d]", *(arr + i)); // 배열에는 쓰레기 값.
	}
	printf("\n");

	memset(arr, 0, sizeof(int) * 5); // 크기 5 만큼 0으로 초기화.

	// 결과 : arr의 총 5개의 요소는 모두 쓰레기 값에서 0으로 초기화 되었다.
}
```

- 주의

```c++
memset(arr, 1, sizeof(int)* 5);
```
위의 경우 두 번째 인자 값 value은 memset 함수 안에서 unsigned char 로 바꿔서 메모리 블록을 다루게 된다.  이때 int 형은 4byte 이고 unsigned shar 은 1byte 이므로 int형 상수 1은 8bit 단위로 1로 초기화 되는 것 ( 00000001 00000001 00000001 00000001)
-> memset() 함수를 초기화 할 목적으로 사용할 때는 반드시 0 혹은 null 로만 사용.