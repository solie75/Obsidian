C++17에서 'std::experimental::filesystem'이 제공되었을 때 나타나는 경고 메시지를 무시하고 컴파일러에서 경고를 비활성화 하는 전처리기 디렉티브이다.

이전 버전의 C++ 에서는 파일 및 디렉터리 경로를 다루기 위해 boost::filesystem 라이브러리를 사용해 왔는데, C++17 부터는 이 라이브러리가 C++ 표준 아리브러리로 포함되면서 'std::filesystem'으로 변경되었다.

std::experimental::filesystem 으로 제공 될 때, 다음의 경고 메시지를 출력한다.
"warning C4996: 'std::experimental::filesystem::xxx': Function call with parameters that may be unsafe - this call relies on the caller to check that the passed values are correct."

여기에서 warning C4996 은 함수 호출 시 보안 상 문제가 발생할 수 있는 매개변수가 전달되었을 때 발생한다.

위의 경고 메시지에서 'std::experimental::filesystem::xxx'는 호출된 함수의 이름을 나타내며, 'Function call with parameters that may be unsafe'는 함수 호출 시 전달된 매개변수가 보안상 문제가 발생할 가능성이 있다는 것을 의미한다. 즉, 호출된 함수가 매개변수를 제대로 검증하지 않고 사용할 수 있다는 것을 나타낸다.

경고 메시지에서 제공된 "this call relies on the caller to check that the passed values are correct"는 호출된 함수가 전달된 매개변수가 올바른지 검증하지 않고 호출되었다는 것을 나타낸다. 이는 호출하는 코드에서 매개변수를 검증해야 한다는 것을 의미한다.

이 경고 메시지는 보안 상의 문제를 야기할 수 있는 코드에 대해 발생한다. 따라서 이 경고 메시지를 해결하기 위해서는 해당 함수의 매개변수를 제대로 검증하고, 안전한 코드를 작성해야 한다. 예를 들어, 문자열 처리 함수를 사용할 때는 문자열의 길이를 확인하거나, 버퍼 오버플로우 등의 보안 문제가 발생할 가능성이 있는 상황을 처리하는 코드를 작성해야 한다.

<experimental/filesystem> 은 C++17 이전 버전에서 파일 시스템 관련 기능을 제공하기 위한 헤더 파일이다. C++17 부터는 'filesystem' 헤더 파일이 표준 라이브러리에 추가되어 더욱 표준화되었다.

'<experimental/filesystem>' 헤더파일은 C++ 표준 라이브러리에 포함되어 있지 않기 때문에, 사용하기 위해서는 컴파일러나 라이브러리에서 해당 기능을 지원해야 한다. 이 헤더 파일에서 제공하는 기능은 '<filesystem.>' 과 거의 동일하지만 namespace 와 몇몇 함수들이 약간 다르게 정의되어 있다.

'<experimental/filesystem>' 헤더 파일에서 제공하는 기능으로는 파일 및 디렉토리의 존재 여부, 경로 조작, 파일 및 디렉토리 생성 및 삭제, 파일 및 디렉토리 이동 등이 있다. C++17 이전 버전에서 파일 시스템 관련 기능을 구현할 때 유용하게 사용될 수 있다.
