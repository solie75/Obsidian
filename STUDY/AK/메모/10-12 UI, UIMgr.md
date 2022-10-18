다이얼 코드 가져오기
- SaveTile 에서  OPENFILENAME 부분 ( COMMDLG 헤드 선언)
- GetsaveFileName 함수 호출이 목적 ( 탐색기)
- lpstrFilter
- 여기에서 만들어진 파일 경로를 fopeen 에서 사용한다.

결국 파일 경로를 알아와서 그것으로 저장과 불러오기를 실행하는 매커니즘

불러오기 
getopenfilename

ui 를 시각화 하여 위의 저장 및 불러오기를 연결시킨다.(자체 ui 제잦ㄱ)

ㅕui 기능응ㄹ 하는 object 생성

1. Object 에서 UI 클래스 생성
2. 사용하고자 하는 level 에서 UI 배치
3. UIMgr 생성
4. ColliderTick 에서 일어나는 충돌체의 위치 에 대해서 fianl tick 에서 최종 위치를 정한다.
5. RigidBody 에 의해서 객체의 위치가 변할 수 있으니 collider 와 animator는  rigidBody 보다 위에 tick 이 돌아야 한다.
6. 

MouseOnCheck() 의 경우 카메라에 따라 이동하는 가 혹은 실제 좌표에 객체에 붙어서 이용하는가()m_bCmrAffacted)에 따라 다르게 코딩해야한다.
 
button UI 생성

버튼이 눌렸을 때 수행해야할 함수의 주소를 받아온다.
1. 불러올 함수가 전역함수일때
	1. 함수만 전달하면 된다 ( 콜백함수로 )
	2. typedef void(*TextFuncType)(void)
2. 불러올 함수가 특정 클래스의 맴버 함수일 때
	1. 함수와 그 함수를 호출할 객체를 전달해야 한다.
	2. 델리게이트( 객체 + 맴버 함수)
	3. 맴버 함수 포인터를 선언할 때에는 타입을 정확히 표시해 주어야 한다.
	4. typedef void(가져올 클라스::*DELEGATE)(void)
	5. 가져올 클라스를 명시하면 그 클라스의 맴버 함수만을 가져올 수 있다. 이는 다른 클래스 에서 가져오려면 따로 typedef 을 해주어야 한다.
	6. 이 가져올 클라스에 최상위 부모 클래스 를 두면 그 모든 자식 클래스의 맴버 함수를 가져 올 수 있다. 
