## 함수 원형

GetTickCount, GetTickCount64 함수는 시스템이 시작한 시점부터 이 함수를 호출한 시점까지 흘러간 시간을 1000분의 1초단위의 시간으로 알려주는 함수.
32비트를 사용하는 GetTickCount 대신 64비트를 사용하는 GetTickCount64를 사용하자.

## 예시

이 함수를 두 번 사용해서 어떤 작업에 소요된 시간을 쉽게 계산.

미세한 시간 오차가 발생할 수 있기 때문에 정밀한 시간 측정흔 multimedia timer 나 high-resolution timer 를 사용
https://learn.microsoft.com/ko-kr/windows/win32/multimedia/multimedia-timers
https://learn.microsoft.com/ko-kr/windows/win32/winmsg/about-timers