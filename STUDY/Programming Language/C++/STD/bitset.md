bitset 은 [index] 로 접근해서 연산자로 쓰고 읽는 것이 가능.
주의 : 기존 배열이나 문자열과 다른게 시작 인덱스 "0" 이 앞쪽이 아닌 뒤쪽 이다. (역순)

- 관련 함수

1. reset(index) : index 비트를 0 으로 변경 -> bit[index] = 0; 생략시 전체 비트를 0 으로 변경
2. set(index, val) : index 비트를 val 값으로 변경 -> bit[index] = val; 여기서 val 은 0 또는 1이며, 생략 시 전체 비트 1 로 변경
3. flip(index) : index 비트 반전, 생략 시 전체 비트 반전 -> !bit[index]
4. all() : 모든 비트가 1일 경우 true/ 그렇지 않으면 false
5. any() : 비트가 1인 것이 하나라도 존재한다면 true/ 그렇지 않으면 false
6. none() : 비트가 1인 것이 하나도 없다면 true/ 하나라도 있으면 false/ 모든 비트가 0인지 확인하는 용도
7. size() : bitset의 size 변환
8. count() : 현재 bitset에 있는 비트 1 개수 반환
9. to_string() : bitset을 문자열로 변환
10. to_ulong() : bitset을 unsigned long integer 타입으로 변환
11. to_ullong() : bitset을 unsigned long long 타입으로 변환

- bitset 생성
```c++
bitset<5> bit; // 00000
bitset<5> bit("10101"); // 문자열 "10101"로 초기화
bitset<20> bit(29); // 10 진수 29로 초기화

---

#include <iostream>
#include <bitset>

using namespace std;

int main()
{
	bitset<5> bit("10101");
	cout << bit << endl;
	int n = bit.to_ulong();
	cout << "10진수 표현: " << n << endl;
	if(bit.any())
	{
		cout << "not zero" << endl;
	}
	else
	{
		cout << "zero" << endl;
	}
}

// 결과
10101
10진수 표현: 21
not zero
```


```c++
int main()
{
	bitset<20> bit(11); // 0000 0000 0000 0000 1011
	string str = bit.to_string();
	cout << str << endl;
	if (bit.any())
	{
		// 1 이 처음 나오는 곳을 find, 찾은 지점부터 문자열 끝 부분을 추출
		str = str.substr(str.find('1'));
	}
	else
	{
		str = "0";
	}
	cout << "2진수 변환: " << str << endl;

	cout << "bitset 중 1의 개수 : " << bit.count() << endl;

	for (int i = 0; i < bit.size(); i++)
	{
		// bitset 은 뒤에서 부터 접근
		cout << bit[i] << " ";
	}
	cout << endl;

	return 0;

}

// 결과
00000000000000001011
2진수 변환: 1011
bitset 중 1의 개수 : 3
1 1 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
```
