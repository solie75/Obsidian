'프로그램'이 실행되어 메모리에 재배치 되는 것을 '프로세스'라고 한다. 이 프로세스는 작업을 진행하기 위한 명령어 목록을 가지고 있는 메모리를 의미하는 용어이다.
이때 이 명령어 목록에서 실행 흐름에 따라 하나씩 명령어를 수행하는 주체가 ' 스레드(Thread)이다'.

Thread : Process 에 포함된 명령어를 실제로 실행하는 주체(작업자)

- Windows 운영체제에서 프로세스가 만들어지면 반드시 하나의 스레드를 가지게 되며 이 스레드가 프로그램에 작성된 작업 순서대로 작업을 진행한다.

- Multithreading
Windows 운영체제에서 새로운 스레드가 필요할 때 , CreateThread라는 API 함수를 사용하여 추가한다.
이 두 스레드는 각각 자신의 작업을 수행하기 때문에 'Multitasking'이 이루어지는데 이 상황을 'Multithreading'이라고 한다.

- 스레드는 왜 필요한가
원래는 하나의 cpu 가 하나의 코어를 가지고 있어 특정 시점에 하나의 프로세스에 포함된 한 개의 명령만 수행되었다. -> 실행흐름을 여러개 만드는 것이 아닌 프로세스를 여러개를 만들어서 동시에 실행되는 것처럼 보이는 멀티 태스킹 기술을 사용

현재  하나의 cpu에 여러개의 코어가 존재하면서 특정 시점에 여러개의 실행 흐름이 존재할 수 있게 되었다. 이렇듯 하나의 프로세스 내에서도 여러 개의 실행 흐름이 존재하도록 만들어야 하기 때문에 스레드라는 개념이 생기게 되었다.