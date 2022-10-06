# open ( )

- 표준 c 라이브러리에서 제공하는 fopen함수와 달리 리눅스에서 제공하는 함수이다.
- fopne 과 차이점
	- open() 함수는 사용하는 파일을 구별하기 위해 파일 디스크립터 라는 정수 번호로 지정하여 사용하는 반면 fopen() 함수는 FILE 구조체 정보, 즉 파일 스트림 정보를 구한다. 

# fopen_s ( )

- 원형
```c++
errno_t fopen_s(
	FILE** pFile,
	const char *filename
	const char *mode
);
errno_t _wfopen_s(
	FILE** pfile,
	const wchar_t *filename,
	const wchar_t *mode
);
```

- 매개변수
	1. pFile : open한 파일 스트림을 설정할 FILE* 형식 변수의 주소
	2. filename : 파일 이름
	3. mode : 허용되는 엑세스 형식

- 입출력 모드 ( 세번째 매개변수 )
	1. r : 읽기 모드일 때 사용. 파일이 없을 경우 오류
	2. w : 쓰기 모드일 때 사용. 파일이 없을 경우 생성
	3. a : append의 약자. 이어쓰기 모드일 때 사용. 파일이 없을 경우 생성
	4. t : Text
	5. b : Binary

- 반환값
	- 성공시 0, 실패시 오류코드를 반환.

- fopen과 차이점
	- _s는 safety의 의미이다.
```c++
FILE* fp = fopen(const char* filename, const char* mode);
```

- fclose
	-  스트림을 이용해서 파일을 읽거나 쓰는 증의 조작을 마치면 fclose 를 이용하여 스트림 소멸을 요청해야 한다. fclose 는 운영체제가 할당한 자원을 반환하거나 버퍼링 되었던 데이터를 출력해주는 역할을 한다.

- 참조
https://jhnyang.tistory.com/196