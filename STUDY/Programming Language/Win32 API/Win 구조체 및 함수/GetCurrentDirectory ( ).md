현재 프로그램의 작업 경로를 얻을 때 사용한다.

```c++
DWORD GetCurrentDirectory (DWORD nBufferLength, LPTSTR lpBuffer);
```

1. nBufferLength
자신이 작업 경로를 저장하기 위해 전달한 메모리의 크기.

2. lpBuffer
작업 경로를 저장할 메모리의 주소

3. 반환값
성공적으로 동작해서 작업 경로를 얻었으면 lpBuffer 변수에 실제로 저장한 문자열의 길이가 반환. 실패하면 0 반환.
