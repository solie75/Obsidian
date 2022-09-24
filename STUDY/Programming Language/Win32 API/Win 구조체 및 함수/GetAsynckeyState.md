## 원형

```c++
SHORT GetAsyncKeyState(int vKey)
```

## 반환값

1. 0 (0x0000) : 이전에 누른적 없고 호출시점에 안눌린 상태
2. 0x8000 : 이전에 누른적이 없고 호출 시점에서 눌린 상태
3. 0x8001 : 이전에 누른적이 있고 호출시점에서 눌린 상태
4. 1 (0x0001) : 이전에 누른 적이 있고 호출 시점에서 안눌린 상태

이 때 0x8000 & 0x8000 으로 AND 연산하면 결과가 0x8000 이고,
0x0001 & 0x8000 으로 AND 연산하면 결과가 0 이다.

```c++
if(GetAsyncKeyState(VK_LEFT) & 0x8000) -> 지금 키가 눌림 
if(GetAsyncKeyState(VK_LEFT) & 0x0001) -> 이전과 지금 사이에 키가 눌림 
if(GetAsyncKeyState(VK_LEFT)) -> 둘다 가능
```