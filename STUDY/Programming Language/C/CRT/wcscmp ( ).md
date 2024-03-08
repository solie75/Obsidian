```c++
int wcscmp (const wchar_t* string1, const wchar_t* String2);
```

wcscmp() 함수는 두 와이드 문자 스트링을 비교한다. 
널로 종료되는 wchar_t 스트링에서 작동한다. 따라서 이 함수에 대한 스트링 인수는 스트링의 끝을 나타내는 wchar_t 널 문자를 포함해야 한다. 

- 반환값
0보다 작음  : String1 이 String2 보다 작음 (String1 < String2)
0 : String1 과 String2 가 같음 (String1 == String2)
0보다 큼 : String1이 String2 보다 크다 (String1 > String2)
