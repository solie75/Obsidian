# 구조 설정

1. windows 데스크톱 애플리케이션으로 새 프로젝트 'client' 생성
2. main.cpp 의 윈도우 창 설정
	1. HWND 형 전역변수 'g_hWnd' 선언
	2. g_hWnd 에는 함수 CreateWindowW( ) 의 반환값을 대입한다.
	3. 함수 ShowWindow( )와 UpdateWindow( )에 매개변수로 g_hWnd 를 주어 작업 영역 크기 조절 및 창 생성 을 한다.
3. 게임의 엔진 파트와 콘텐츠 파트를 분리하기 위해, 엔진 프로젝트 추가. (이 때 엔진 프로그램은 정적 라이브러리로 한다.)
기준 프로젝트인 'Client' 를 기준으로 정적 라이브러리인 'Engine' 을 참조하여 사용
- 동적 라이브러리 를 'Client '가 참조하는 방식 :