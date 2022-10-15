1. Tile.cpp
2. Level.cpp
	1. CreateTile()
	2. DeleteObject()
	3. Tile Position
	4. Tile Size 정의
	5. 
3. TileRender()
	1. Rectangle()
	2. EditorLevel 의 카메라 시점
4. CEditorLevel::Init()
	1. 아틀라스 이미지 로딩
	2. 타일생성
5. CTile.SetImgIdx()
6. CEditorLevel::Init()
	1. 각 타일에다가 사용할 아틀라스 이미지오, 이미지 인덱스를 세팅
7. TileRender()
	1. iMaxCor, ICurRow
	2. Rextangle 대신 Birblt
-> 정해진 행 열의 갯수대로 타일이 생성됨
6-1 에서  SetImagIdx 를 i 로 하면 순서대로 타일 출력


1. 윈도우 기본 메뉴바 main.cpp
2. WndProc
3. WM_COMMAND =  도움말
4. default resource. 
5. 리소스 뷰
6. 보기 -> 속성
7. 코딩으로 메뉴바 디자인 (주먹구구식)
	1. CreateMenu ()
	2. SetMuenuInfo( )
	3. SetMenu()
8. 리소스 뷰에서 타일개수 변경 추가 -> 임의 자동 id -> ID 변겅 -
9. -=>resource.h 에 추가되어 있음
10. wm_command 에 만든 id 를 case 로 둔다.
11. DialogBox() 의 makeintresource 로 저장해 놓은 디자인 사용
12. 리소스 뷰는 윈도우 7이 형태로 보여준다. 실제로 적용되는건 10으로 리소스 뷰에서 보이는 형태랑 실제 형태랑 다르다는 것이다.
13. DialogBox 함수의 About
14. WM_COMMAND 는 대부분 버튼을 누를 때 발생한다.
15. 모달방식 : 자식 윈도우가 직렬적 작용
16.  모달less 방식; 자식 윈도우와 부모 윈도우가 병렬적 작용 ( CreateDialog)
17. 리소스 뷰에서 타일갯수 정하는 다이얼 만들기
18. main 의 About 함수를 CEditorLevel 에 가져다가 (Tile countDialof Proc) ABout->TileCount 함수명 변경
19. TileCopunt 함수를 전방선언하여 main 에서 알 수 있게 한다.

신중은의 상상은 현실이 된다.