CLevelMgr.cpp

```c++
void CLevelMgr::LevelMgrTick()
{
	m_pCurLevel->LevelTick();
}
```

부분에서 발생한 오류이다. 이 위에 

```c++
void CLevelMgr::LevelMgrInit()
{
	// Level 생성
	m_arrLevel[(UINT)LEVEL_TYPE::START] = new CStartLevel;
	m_arrLevel[(UINT)LEVEL_TYPE::STAGE_01] = new CStage01Level;

	m_pCurLevel = m_arrLevel[(UINT)LEVEL_TYPE::START];
	m_pCurLevel->LevelInit();
}
```

이 부분에서 분명히  m_pCurLevel 부분에 LEVEL_TYPE::START 가 들어 갔음에도 nullptr 로 되어있는 이유는 CEngine 쪽에서  LevelMgrInit  자체를 코딩하지 않아서 레벨매니저의 초기화 부분이 이루어지지 않았기 때문이다.

해당 변수에 nullptr 로 엑세스 위반 오류가 뜨면
1. 자신이 해당 변수에 값을 대입했는가
2. 값을 대입하는 함수 등의 부분을 호출하였는가를 확인하자.