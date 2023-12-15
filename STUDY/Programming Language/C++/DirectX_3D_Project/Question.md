## Singleton atexit()

^35aaf0
typedef void (\*EXIT)(void); 는 함수 포인트이다.
atexit() 의 원형은 다음과 같다.
```c++
int atexit(
   void (__cdecl *func )( void )
);
```
atexit 에 CSingleton<\T>::Destroy 를 인자로 줄 때 EXIT 함수포인트 형으로 변환하는 이유가 무엇인가?

정적 맴버 함수인 CSingleton<\T>::Destroy 의 주소를 일반 함수인 EXIT 로 형변환 하는 이유가 무엇인가. atexit 함수가 요구하는 인자의 원형에 맞추기 위해서 인가?

