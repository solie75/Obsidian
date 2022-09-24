## enum

```c++
#include <iostream>

int main()
{
	enum Color
	{
		RED, // 이 요소 RED 와 Color 의 주소는 같다.
		Blue
	};

	enum Fruit
	{
		BANANA, // 이 요소 BANANA 와  Fruit의 주소는 같다.
		APPLE
	};

	Color color1 = RED;
	Fruit fruit1 = BANANA;

	if(color1 == fruit1)
	{
		std::cout << "color and fruit are equal\n";
	}
	else
	{
		std::cout << "color and fruit are nit equal\n";
	}

	return 0;
}
```

c++ 은 color1와 fruit1 을 비교할 때, color1와 fruit1을 암시적으로 정수로 변환하고 정수를 비교한다.
color1, fruit1 모두 값 0으로 계산되는 열거자로 설정되었으므로 위의 예에서 color1 과 fruit1은 같다고 여겨진다.

## enum class

```c++
#include <iostream>

int main()
{
	enum class Color
	{
		RED,
		BLUE
	};

	enum class Fruit
	{
		BANANA,
		APPLE
	};

	Color color1 = Color::RED; // 바로 요소에 접근할 수 없다. 
	Fruit fruit1 = Fruit::BANANA;

	if(color1 == fruit1)
	{
		std::cout<<"color1 and fruit1 are equal\n"
	}
	else
	{
		std::cout<<"color1 and fruit1 are not equal\n"
	}

	return 0;
}
```

일반 열거형(enum)을 사용하면  열거자(ex. RED)는 열거형 자체와 같은 범위에 있으므로 직접 접근할 수 있다. 그러나 열거형 클래스(enum class)의 강력한 범위 규칙은 모든 열거자를 열거형 일부로 간주하므로 범위 한정자(::)를 사용하여 열거자에 접근 할 수 있다.