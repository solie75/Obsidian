
예외 처리(exception handling) 은 예외 상황을 처리할 수 있도록 코드의 흐름을 바꾸는 행위를 의미한다.

1. try 문
   예외가 발생할 가능성이 있는 코드 블록
2. throw 문
   try 문에서 발생한 오류에 대한 정보를 전달
3. catch 절
   발생한 예외에 대해 예외 핸들러가 처리할 내용을 담은 코드 블록

- try 문 다음에 반드시 하나 이상의 catch 절을 구현해야한다.
- 각 catch 절은 자신이 처리할 수 있는 예외 타입을 지정할 수 있다.
- 이때 특정 예외 타입 대신에 줄임표(...) 를 사용하면, 해당 catch 절은 모든 타입의 예외를 처리하게 된다. -> 하지만 이런 줄입표를 사용한 catch 절의 위치는 언제나 catch 절 중 맨 마지막에 위치해야 한다.

예시 1.

```c++
int IncreaseNum(int n)
{
    if (n < 0)
    {
        throw n;
    }
    else if (n == 0)
    {
        throw "0은 입력할 수 없습니다.";
    }

    return ++n;
}

int main()
{
    int num;

    cout << "양의 정수를 하나 입력하시오 : ";
    while (cin >> num)
    {
        try
        {
            cout << "입력한 정수에서 1이 증가한 값 : " << IncreaseNum(num) << endl;
        }
        catch (int input)
        {
            cout << input << "은 양의 정수가 아닙니다." << endl;
            cout << "양의 정수를 다시 입력하시오 : ";
            continue;
        }
        catch (const char* st)
        {
            cout << st << endl;
            cout << "양의 정수를 다시 입력하시오 : ";
            continue;
        }
        break;
    }

    return 0;
}
```

예시 1의 순서
1. try 문에 도달한 프로그램의 제어는 try 문 내의 코드를 실행한다.
2. IncreaseNum() 내에서 예외 발생(throw) 가 발생하지 않으면 프로그램의 제어는 맨 마지막 catch 절 바로 다음으로 이동한다.
3. 만약 예외가 발생하면 catch 핸들러는 다음의 순서로 적절한 catch 절을 찾는다.
   1. try 문과 가장 가까운 catch 문 부터 차례로 검사한다.
   2. 적절한 catch 문을 만나면 throw 된 값이 해당 catch 문의 매개변수로 전달되고 내부 코드를 실행한다. 적절한 catch 문이 하나도 없을 경우 익셉션이 throw 될 때 표준 라이브러리가 익셉션을 받게 되고 프로그램이 종료되어 버린다.


예시 2
```c++
double divideNumbers(double numerator, double denominator)
{
    if (denominator == 0)
    {
        throw invalid_argument("Denominator cannot be 0.");
    }
    return numerator / denominator;
}

int main()
{
    try
    {
        cout << divideNumbers(2.5, 0.5) << endl;
        cout << divideNumbers(2.3, 0) << endl;
        cout << divideNumbers(4.5, 2.5) << endl;
    }
    catch (const exception& exception)
    {
        cout << "Exception caught : " << exception.what() << endl;
    }

    return 0;
}

// 결과
5
Exception caught : Denominator cannot be 0.
```

두 번재 호출에서 익셉션이 발생하여 catch 블록으로 실행 흐름이 넘어가기 때문에 세 번째 호출은 실행되지 않는다.
익셉션을 제대로 이용하려면 익셉션을 throw 할 때 스택 변수에 무슨 일이 일어나는지 정확하게 이해하고 이를 바탕으로 catch 블록에서의 작업을 적절하게 구성해야 한다. 만약 호출과정 중에 catch 브록이 하나도 없다면 익셉션이 throw 될 때 표준 라이브러리가 익셉션을 받게 되고 프로그램이 종료되어 버린다.

