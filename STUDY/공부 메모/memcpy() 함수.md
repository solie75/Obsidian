memory copy, 즉 메모리의 값을 복사하는 기능을 하는 함수.

```c++
void* memcpy(void* Dst, const void* Src, size_t Size)
```

void* Dst : 복사본이 될 메모리를 가리키는 포인터

const void* Src : 복사의 원본이 될 메모리를 가리키고 있는 포인터

size_t Size : 복사할 데이터(값)의 길이(바이트 단위)

-> memcpy( 복사본 메모리, 원본 메모리, 길이)

- 주의
1. 길이를 계산할 때 char* 타입의 c언어 문자열 형태의 문자열의 전체를 복사할 때 맨 뒤에 문자열의 끝을 알리는 "\0"의 길이도 게산해서 넣어야 하기 때문에 +1의 길이만큼 해주어야 한다.


- 예제
1. int* 타입
```c++
#include <string.h>
#include <iostream>
#include <stdio.h>

using namespace std;

int main() {
	int src[] = { 1,2,3 };
	int dest[3];

	// 메모리 복사
	memcpy(dest, src, sizeof(src));

	// 원본 배열
	for (int i = 0; i < 3; i++)
	{
		cout << src[i] << endl;
	}

	// 복사완료
	for (int i = 0; i < 3; i++)
	{
		cout << dest[i] << endl;
	}

	return 0;
}
```

2. char* 타입 전체 복사
```c++
#include <string.h>
#include <iostream>
#include <stdio.h>

int main() {
	char src[] = "MapleStory";
	char Dst1[] = "abcdefghijklmnopqrstuvwxyz";
	char Dst2[] = "abcdefghijklmnopqrstuvwxyz";
	
	// 메모리 복사 : src 길이 만큼만 복사
	memcpy(Dst1, src, sizeof(char) * 10);

	// 메모리 복사 : src 길이 +1 만큼 복사
	memcpy(Dst2, src, sizeof(char) * 10 + 1);

	cout << src << endl;

	cout << Dst1 << endl;
	cout << Dst2 << endl;
	
	return 0;
}
```

![[Pasted image 20220903222551.png]]
Dst1은 "MapleStory" 까지만 복사 되었기 때문에
"MapleStoryKlmnopqrstuvwxyz\0" 가 되고
Dst2는 "MapleStory\0" 까지 복사 되었기 때문에
"MapleStory\0lmnopqrstuvwxyz\0" 가 된다. 