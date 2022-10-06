```c++
int sprintf(char* str, const char* format, ...);
```

- 매개변수 
	1. char* str : format 을 복사하여 집어 넣는 곳
	2. const char* format : 서식 지정자가 포함된 문자열

- 반환값
int형의 성공한 문자의 개수 ( 실패시 -1을 반환한다.)

- 기타
printf() 함수가 화면에 출력을 진행한다면 sprintf () 는 char* str 에 복사 대입한다.

