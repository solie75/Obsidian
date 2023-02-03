typedef 를 활용하여 사용자 정의 타입을 생성하고, 실제 타입 대신 사용할 수 있다. 

구문
```c
typedef (자료형) (사용자 정의 자료형)
```

※ 보통 typedef 에서 사용자 정의 자료형은 $\_t$ 접미사를 사용하여 선언한다.

typedef 는 새로운 타입을 정의하는 것이 아니라 기존 타입에 사용자 지정 이름을 붙이는 것뿐이다.

## 가독성

사용된 자료형의 목적을 알고 싶은 경우 등의 가독성을 위해 사용된다
```c
typedef int testScore_t;
testScore_t GradeTest();
```
-> GradeTest( ) 함수의 반환값은 typedef 를 통해 정수임과 동시에 시험점수를 나타냄을 알 수 있다.

## 플랫폼 세부 사항

플랫폼에 따라서, 정수는 2바이트 또는 4바이트이다. 하지만 char, short, int, long 은 해당 크기를 나타내지 않으므로 교차 플랫폼 프로그램에서 크기(비트)를 나타내어 실수를 방시하고 변수의 크기에 대해 명확히  알 수 있게 한다.
```c
#ifdef INT_2_BYTES
typedef char int8_t;
typedef int int16_t;
typedef long int32_t;
#else
typedef char int8_t;
typedef short int16_t;
typedef int int32_t;
#endif
```


## typedef 함수 포인터

구문
```c
typedef 반환형 (*사용할 함수포인터 이름) (함수 매개변수, ...)
```

예시 : 반환형이 int이고 HData 형 매개변수 2개를 받는 함수 포인터를 PCheck 로 사용
```c
typedef int(*PCheck)(HData data1, HData data2);
```



