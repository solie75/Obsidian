size_t 는 unsigned int 이며, 문자열이나 메모리의 사이즈를 나타낼 때 사용한다. 

```c
typedef unsigned int size_t;
```

size_t 는 32비트 운영체제에서는 '부호없는 32비트 정수'이고 64비트 운영체제에서는 '부호없는 64비트 정수'이다.

그러나  'unsigned int' 혹은 'int' 는 64비트 OS 라고 하여 꼭 64비트 정수는 아니다. 이것이 size_t 와 unsigned int 형의 차이이다.

메모리나 문자열 등의 길이를 구할 때에는 'unsigned int' 대신 size_t 형 으로 길이가 반환된다.