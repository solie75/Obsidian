세컨드 비트맵과 같이 하나의 창 전체의 이미지 효과를 가지는 것

세컨드 비트맵의 핸들과 DC 를 하나의 CTexture 로 볼 수 있다.
-> 리소스 매니저에서  CreateTexture() 함수로  해상도만 지정해주면 메모리상에 텍스쳐를 만드는 개념
-> 결국 m_mapTex 에 관리 될 테니 이름도 정해 준다.

CTexture 에 Create() 함수 추가
-> 파일 경로는 가지고 있지 않다

실제 CreateTexture 가 쓰이는 곳은 CCameraMGr 의 생성자 부분이다.]
-> CCameraMgr  도 이제 Rende r  할 것이 생김 CCameraMgrRender 함수 추가

Fade in Fade out 추가 def.h

CCameraMgr 에 
->FadeOut Fade IN 함수 추가
-> 현재 이펙트가 무엇인지 아는 변수 추가
-> 

작업 큐 생성
