# Rendering Pipeline 에서 사용되는 요소

1. Graphic 폴더에 Graphic.h 추가



rendertarget buffer 와 같은 것을 texture 2D 로 분류하고 각종 resource 들을 관리하는 하나의 배열<CResourceMgr::m_arrRes[]>에 저장해 둔다.

즉 CResourceMgr 클래스 에서
map<wstring, Ptr<CResource> m_arrRes[RES_TYPE::END];
을 맴버변수로 가지고 있음
m_arrRes 의 요소인 map 에 resource 를 저장함
 