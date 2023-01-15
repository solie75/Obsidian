연산의 대상이 될 수 있는 것의 유형을 바꾸는 것

표현 범위가 좋은 타입에서 표현 범위가 넓은 타입으로의 타입 변환은 문제가 되지 않으나 반대의 경우인 표현 범위가 좁은 타입으로의 타입 변환에서는 데이터의 손실이 발생할 수 있다.

## Implicit type conversion

묵시적 유형 변환 (자동 유형 변환)

컴파일러가 자동으로 실행해주는 유형 변환
c 언어에서는 대입 연산 시 연산자의 오른쪽에 존재하는 데이터의 유형이 연산자의 왼쪽에 존재하는 데이터의 유형으로 묵시적 유형 변환이 진행된다.
또한, 산술 연산에서는 데이터의 손실이 최소화되는 방향으로 묵시적 타입 변환이 진행된다.

## Explicit type conversion

명시적  유형 변환 (강제 유형 변환)
사용자가 type cast 연산자를 사용하여 강제적으로 수행하는 타입 변환을 말한다.