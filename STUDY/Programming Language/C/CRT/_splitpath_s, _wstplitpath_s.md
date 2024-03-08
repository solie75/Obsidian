경로 이름 (path name) 을 구성 요소로 나눈다.

# Syntax

```c
errno_t _splitpath_s(
   const char * path,
   char * drive,
   size_t driveNumberOfElements,
   char * dir,
   size_t dirNumberOfElements,
   char * fname,
   size_t nameNumberOfElements,
   char * ext,
   size_t extNumberOfElements
);
errno_t _wsplitpath_s(
   const wchar_t * path,
   wchar_t * drive,
   size_t driveNumberOfElements,
   wchar_t *dir,
   size_t dirNumberOfElements,
   wchar_t * fname,
   size_t nameNumberOfElements,
   wchar_t * ext,
   size_t extNumberOfElements
);
template <size_t drivesize, size_t dirsize, size_t fnamesize, size_t extsize>
errno_t _splitpath_s(
   const char *path,
   char (&drive)[drivesize],
   char (&dir)[dirsize],
   char (&fname)[fnamesize],
   char (&ext)[extsize]
); // C++ only
template <size_t drivesize, size_t dirsize, size_t fnamesize, size_t extsize>
errno_t _wsplitpath_s(
   const wchar_t *path,
   wchar_t (&drive)[drivesize],
   wchar_t (&dir)[dirsize],
   wchar_t (&fname)[fnamesize],
   wchar_t (&ext)[extsize]
); // C++ only
```

# Parameters

- path
Full path.

- drive
( : )  뒤에 붙는 드라이버 문자. 필요없는 경우 Null 처리.
 ^55dab2
- driveNumberOfElements
단일 byte 혹은 wide characters 로 표현되는 [[_splitpath_s, _wstplitpath_s#^55dab2|drive]] buffer 의 크기. drive 가 Null 인 경우 이 값은 0이 되어야 한다.

- dir
후행 슬래쉬를 포함한 디렉터리 경로. forward slashs(/), backslashes(\\) 또는 둘다 사용될 수 있다. 필요하지 않은 경우 Null 처리
 ^514c21
- dirNumberOfElements
단일 byte 혹은 wide characters 로 표현되는 [[_splitpath_s, _wstplitpath_s#^514c21|dir]] 의 사이즈. dir 이 Null 인 경우 이 값은 0이 되어야 한다.

- fname
기본 파일 이름 (without extension).  필요하지 않은 경우 Null 처리
 ^bdf362
- nameNumverOfElements
단일 byte 혹은 wide characters 로 표현되는 [[_splitpath_s, _wstplitpath_s#^bdf362|fname]] 의 크기. fname 이 Null 인 경우 이 값은 0이 되어야 한다.

- ext
파일이름의 확장자 (leading period 포함). 
 ^43a9a9
- extNumberOfElements
단일 byte 혹은 wide characters 로 표현되는 [[_splitpath_s, _wstplitpath_s#^43a9a9|ext]] 의 크기. ext 이 Null 인 경우 이 값은 0이 되어야 한다.

# Return value

성공적이면 0을 실패면 에러코드를 반환

# 에러가 나는 상황

_`path`_ is `NULL`
_`drive`_ is `NULL`, _`driveNumberOfElements`_ is non-zero
_`drive`_ is non-`NULL`, _`driveNumberOfElements`_ is zero
_`dir`_ is `NULL`, _`dirNumberOfElements`_ is non-zero
_`dir`_ is non-`NULL`, _`dirNumberOfElements`_ is zero
_`fname`_ is `NULL`, _`nameNumberOfElements`_ is non-zero
_`fname`_ is non-`NULL`, _`nameNumberOfElements`_ is zero
_`ext`_ is `NULL`, _`extNumberOfElements`_ is non-zero
_`ext`_ is non-`NULL`, _`extNumberOfElements`_ is zero

위의 경우중 하나라도 발생하는 경우, (https://learn.microsoft.com/en-us/cpp/c-runtime-library/parameter-validation?view=msvc-170) 에 설명된 것과 같이 유효성 검사 매개변수 핸들러가 호출된다.

버퍼가 결과를 담기에 작은 경우 모든 버퍼를 빈 string 으로 clear 하고 errno 를 ERANGE 로 세팅한 뒤 ERANGE 를 반환한다.

# Reference

https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/splitpath-s-wsplitpath-s?view=msvc-170