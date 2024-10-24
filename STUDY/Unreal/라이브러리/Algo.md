# Algo::Transform

입력 범위의 요소들을 지정된 변환 함수에 적용하여 결과를 출력 범위에 저장한다.

```c++
namespace Algo
{
    template <typename InputIt, typename OutputIt, typename UnaryOperation>
    void Transform(InputIt First, InputIt Last, OutputIt DestFirst, UnaryOperation UnaryOp)
    {
        while (First != Last)
        {
            *DestFirst++ = UnaryOp(*First++);
        }
    }
}

```

- InputIt First : 입력 범위의 시작 반복자.
- InputIt Last : 입력 범위의 끝 반복자. 
- OutputIt DestFirst : 출력 볌위의 시작 반복자.
- UnaryOperation UnaryOp : 입력 요소를 변환하는데 사용될 단항 함수 객체 또는 람다 표현식.

예시
```c++
#includ "Algo.Transform.h"
#include <vector>
#include <iostream>

int main()
{
	// 입력 벡터
	std::vector<int> Input = {1, 2, 3, 4, 5};

	// 출력 벡터
	std::vector<int> Output;
	Output.resize(Input.size()); // Output 벡터의 크기를 Input 벡터의 크기과 동일하게 설정

	// 변환 함수 : 각 요소를 제곱
	auto Square = [](int x) {return x * x; };

	// Algo::Transform 사용
	Algo::Transform(Input.begin(), Input.end(), Output.begin(), Square);

	// 출력 결과 확인
	for(int Value : Output)
	{
		std::cout << Value << " ";
	}
}

return 0;
```

```c++
// ABGameSingleton.cpp
// Fill out your copyright notice in the Description page of Project Settings.


#include "GameData/ABGameSingleton.h"

DEFINE_LOG_CATEGORY(LogABGameSingleton);

UABGameSingleton::UABGameSingleton()
{
	// Row Table 에 접근하여 엑셀 데이터 불러오기.
	static ConstructorHelpers::FObjectFinder<UDataTable> DataTableRef(TEXT("/Script/Engine.DataTable'/Game/ArenaBattle/GameData/ABCharacterStstTable.ABCharacterStstTable'"));
	
	if (nullptr != DataTableRef.Object)
	{
		const UDataTable* DataTable = DataTableRef.Object;
		check(DataTable->GetRowMap().Num() > 0);

		TArray<uint8*> ValueArray;
		DataTable->GetRowMap().GenerateValueArray(ValueArray); // 행의 모든 값을 ValueArray 에 채운다.
		Algo::Transform(ValueArray, CharacterStatTable,
			[](uint8* Value)
			{
				return *reinterpret_cast<FABCharacterStat*>(Value);
			}
		); // 수정하기 힘든 코드 아닌가..? FName으로 찾는게 맞는거 아닌가
	}

	CharacterMaxLevel = CharacterStatTable.Num();
	ensure(CharacterMaxLevel > 0);
}

UABGameSingleton& UABGameSingleton::GetGameSingleton()
{
	UABGameSingleton* Singleton = CastChecked<UABGameSingleton>(GEngine->GameSingleton);
	if (Singleton)
	{
		return *Singleton;
	}

	UE_LOG(LogABGameSingleton, Error, TEXT("Invalid Game Singleton"));
	return *NewObject<UABGameSingleton>();
}
```
-> 빌드로 실행했을 때 Error 로그가 없다면 정상적으로 수행한 것이다.

#### 캐릭터 스탯 시트템 구현

프로젝트 주요 레이어에 따라