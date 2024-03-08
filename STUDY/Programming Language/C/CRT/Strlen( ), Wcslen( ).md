문자열의 길이를 구할 때 호출한다.

# strlen( )

1. string length 의 줄임말로 정확히는 MBCS 길이를 구하는 함수이다.
2. string.h 를 include 한다.
3. 문자열의 시작 주소를 매개변수로 전달하면 문자열 길이를 반환한다.
4. 문자열의 시작 주소에서 NULL 을 만나기 전까지 오른쪽( 높은 메모리 주소)으로 1칸씩 이도하며 이동할 때마다 1씩 카운팅한다. -> <span style="color: yellow ">문자열 시작 주소에서 NULL 을 만나는 이전 위치까지의 간격(바이트 수)을 구하는 것</span>


# wcslen()

1. wide character set length 의 줄임말로 정확히는 WBCS을 지원하는 문자열 길이를 구하는 함수이다.
2. tchar.h 와  wstring.h 을 include 한다.
3. WCS 문자열의 시작 주소르 매개변수로 전달하면 문자열 길이를 반환한다.
4. 문자열의 시작 주소에서 NULL 을 만나기 전까지 오른쪽 (높은 메모리 주소)으로 2칸씩 이동하며 이동할 때마다 1씩 카운트 된다. -> <span style="color: yellow">문자열 시작 주소에서 NULL 을 만나는 이전 위치까지의 간격에서 나누기 2를 한 것이라 볼 수 있다.</span> 