상속 구조의 문제점

-> 해결하기 위해서 컴포넌트 구성방식 사용

컴포넌트 란

CCollider::CCollider(CObject* _pOwner)
	: CComponent(_pOwner)
{
}

여기에서  CComponent(_pOwner) 가 있는 이유 
사각형 