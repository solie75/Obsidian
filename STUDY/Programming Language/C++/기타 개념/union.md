c++ 에서 제공하는 묶음 타입(class, struct, union) 으로 공유체라 말한다.

union은 모든 면에서 구조체와 같으나 모든 멤버 변수가 하나의 메모리 공간을 공유한다는 점이 다르다.
또한 모든 멤버 변수가 같은 메모리를 공유하기 때문에 union 은 하나의 멤버 변수 밖에 사용할 수 없다.

![[Pasted image 20240313180301.png]]

union 은 순서가 규칙적이지 않고, 미리 알 수 없는 다양한 타입의 데이터를 저장할 수 있도록 설계된 타입이다.
union 은 가장 큰 멤버 변수의 크기로 메모리를 할당받는다.
=> union 을 자료형으로 하는 배열을 사용하면, 같은 크기로 구성된 배열 요소에 다양한 크기의 데이터를 저장할 수 있다.

예시 1
```c++
union ShareData
{
	unsigned char a;
	unsigned short b;
	unsigned int c;
};

int main()
{
	ShareData var;
	var.c = 0x12345678;

	std::cout << var.a << std::endl;
	std::cout << var.b << std::endl;
	std::cout << var.c << std::endl;

	return 0;
}

// 결과
x
22136
305419896
```

union 에 저장된 값의 의미는 값을 저장할 때 공용체의 어떤 멤버 변수를 사용했는지에 따라 달리 해석된다.

열거체 이용 예시
```c++
enum Weather { SUNNY = 0, CLOUD = 10, RAIN = 20, SNOW = 30};

int main(void)
{
	int input;
	Weather wt;
	std::cout << "오늘의 날씨를 입력 : " << std::endl;
	std::cout << "(SUNNY=0, CLOUD=10, RAIN=20, SNOW=30)" << std::endl;
	std::cin >> input;
	wt = (Weather)input;

	switch (wt)
	{
	case SUNNY:
		std::cout << "오늘의 날씨 맑음" << std::endl;
		break;
	case CLOUD:
		std::cout << "오늘의 날씨 흐림" << std::endl;
		break;
	case RAIN:
		std::cout << "오늘의 날씨 비" << std::endl;
		break;
	case SNOW:
		std::cout << "오늘의 날씨 눈" << std::endl;
		break;
	default:
		std::cout << "정확한 상숫값 입력" << std::endl;
		break;
	}

	std::cout << "열거체 Weather의 각 상숫값은 " << SUNNY << ", " << CLOUD << ", "

		<< RAIN << ", " << SNOW << "입니다.";

	return 0;
}
```

열거체를 사용하면 변수가 지니는 값에 의미를 부여할 수 있기 때문에 union 의 멤버 변수에 참고하여 사용하기 좋다.

활용 예시
```c++
// Collision ID 를 정의하는 코딩
CCollisionMgr.h 에 CollisionID 를 나타내는 union 정의 

union CollisionID
{
	struct
	{
		UINT LeftID;
		UINT RightID;
	};

	UINT_PTR id;
};

GameObject 사이의 Collsion 관계를 따질 때 두 GameObject 의 Collider2D 로 ID 를 생성한다.

CollisionID tempID = {};
tempID.LeftID = 첫 번째 Collider 의 ID;
tempID.RightID = 두 번째 Collider 의 ID;

union 은 각 하나의 메모리를 변수 끼리 공유한다.
즉 struct 의 값을 변경하면 메모리의 값이 변경되어 UINT_PTR 이 표현되는 값이 달라지고. 반대로
UINT PTR 의 값을 변경하면 메모리의 값이 변경되어 struct 로 표현되는 값이 달라진다.
```
