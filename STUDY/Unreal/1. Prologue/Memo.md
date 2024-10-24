0. Editor
![[Pasted image 20240512144431.png]]

1강. 환경 설정

Edit -> Preferences -> General -> Region & Language 에서 언어 변경 가능

window -> Load Layout -> UE4 classic layout
프로그램의 레이아웃을 언리얼 4 버전으로 변경

ctrl + n
new Level

Content Browser -> 프로젝트 명 -> All -> Content 에 우클릭 -> new Folder 클릭
-> Maps 폴더 생성

File -> Save Current Level 로 현재의 Level 저장. (단축키 Ctrl + s)

Edit -> Project -> Maps & Modes -> default Maps -> Editor Startup Map 을 DevMap 으로 변경
Edit -> Project -> Maps & Modes -> default Maps -> Game Default Map 또한 DevMap 으로 변경
해당 프로젝트를 열었을 때 DevMap 으로 세팅된다.

2. 언리얼 엔진 기초

Place Actors 패널 -> Shaps 의 도형을 드래그로 생성 -> 생성된 도형을 드래그로 이동(가운데 지점을 클릭하면 두 개의 축을 기준으로 움직이는 것이 가능)
View Part 의 우측 상부에 이동, 회전, 스케일 변환 버튼 있음(단축키, 이동 : w , 회전 : e, 스케일 : r)

도형을 선택한 뒤 End 키를 누르면 해당 도형의 밑 면이 바닥에 접하도록 위치가 이동 된다. 이미 해당 도형이 바닥과 접하거나 겹쳐있거나 아래에 있다면 End 키는 작동하지 않는다. 

도형을 선택한 뒤 F 키를 누르면 해당 물체를 클로즈 없해서 뷰토프 화면 상에 가득 찬 상태로 볼 수 있다.

Outliner 패널에서는 light 를 포함한 생성되어 있는 모든 객체를 표시하고 있다.
Detail 패널에서는 Outliner 에 표시된 객체 중 선택된 객체의 세부 정보를 표시한다.

Outliner 는 모든 객체가 계층 구조로 이루어져 있다. 
하나의 객체를 드래그 하여 타 객체에 속하게 만들 수 있다. 이때 드래그한 객체 (타객체에 속하게된 객체) 를 Child 객체라고 하고 드래그의 목적이 되는 객체(Child 가 속한 객체) 를 parent 객체라고 할 때 Parent 객체의 위치 각도 크기의 변화에 맞춰 Child 의 위치 각도 크기 또한 변하는 반면 Child 객체의 위치 각도 크기의 변화는 Parent 객체에 영향을 끼치지 않는다.

play 패널에서 재생 버튼 클릭 또는 Alt + p 를 누를면 simulation 이 시작된다. 시뮬레이션이 시작되면 마우스가 해당 시뮬레이션에 귀속 되며 simulation 에서 마우스 귀속을 해제하고 싶다면 Shift + f1 키 을 누른다. view port 를 마우스로 클릭하면 마우스는 다시 simulation 에 귀속된다. 시뮬레이션 을 종료하지 않고 Edit 상태가 되고 싶다면 F8 키를 누른다. 이 상태에서는 view port 를 마우스로 클릭해도 해당 simulation에 마우스가 귀속 되는 것이 아니라 객체를 클릭 할 수 있게 된다. Edit 상태에서 다시 simulation 으로 되돌아 가기 위해서는 다시 F8 키를 누르면 된다.
simulation 상태에서 Outliner 에 있는 개체를 삭제해도 해당 simulation 에서만 삭제될 뿐 실제로 삭제되는 것은 아니다. 
simulation 을 종료하면 객체가 그대로 있음을 볼 수 있다.
ESC 키를 눌러 simulation 을 종료할 수 있다.

Place Actors 패널 -> Camera Actor 를 생성하면 해당 카메라는 사용자가 view port 로 보고 있는 것과는 별개로 자신의 관점에서의 view port 를 갖고 있다.

3. 블루 프린트 기초

레벨을 생성할 때마다 그에 대한 블루 프린터도 동시에 생성된다.

윗 패널에서 'List of World Blueprints available to the user for editing or creation' -> open level blueprint 클릭 -> 해당 레벨의 Level blue print Edit 창이 띄워진다.


우측 클릭 -> All Actions for this Bluprint 창 -> print Text 검색 후 클릭 -> Print Text 노드 생성
Event BeginPlay 노드의 Exec 과 Print Text 노드 의 Exec 을 드레그로 연결한다. -> 윗 패널의 Compile 버튼을 클릭하고 Ctrl + S 로 저장 -> 본 editor 창 으로 가서 Simulation 을 재생하면 좌상단에 Hello (Print Text 노드의 기본 출력 값) 이 출력된다.

빨간 색 노드 (Exec Nodes) :
- 실행 흐름을 나타낸다. 이러한 노드들은 주로 조건문, 반복문, 함수 호출 등과 같이 코드의 실행 흐름을 제어하는 데 사용된다.
- 예를 들어, if 노드는 조건에 따라 코드의 실행 여부를 결정하는 데 사용된다. Execute 노드와 같이 특정 작업이 실행되도록 하는 역할을 한다.

파란색 노드 (Variable Nodes) : 
- 데이터와 관련된 작업을 나타낸다. 이러한 노드들은 변수, 상수, 함수 등을 정의하거나 사용하는 데 사용된다.
- 예를 들어, 변수 할당 노드는 변수에 값을 할당하거나 변경하는 데 사용된다. 또한, 변수를 생성하고 변수의 값을 가졍는 데에도 사용된다.

즉 빨간색 노드는 코드의 흐름을 제어하고 실행하는 데 사용되는 반면에, 파란색 노드는 데이터를 다루고 처리하느 데 사용된다.

Blue Print Edit 창을 처음 띄웠을 때 보이는 Event BeginPlay 노드는 해당 레벨이 실행 되었을 때 일회성으로 호출되는 함수이고 Event Tick 은 매 프레임 마다 실행 되는 함수이다.

예를 들어 keyboard 1을 검색하여 ' events for when the 1 key is pressed or released ' 노드를 생성하면 시뮬레이션에서 1 키를 누를 때마다 해당 노드가 실행된다.

한개 또는 여러개의 노드를 선택하여 ( Ctrl + C , Ctrl + V ) 또는 Ctrl + D 로 노드를 복사 생성할 수 있다.

작성한 블루프린터를 시뮬레이션에서 재생하기 위해서는 우선 컴파일과 저장을 해야 한다.
여기에서 컴파일이란 블루 프린터를 컴퓨터가 이해할 수 있는 언어로 변경하는 행위를 말한다.

노드에 마우스를 갖다 대면 노드의 좌상단에 말풍선이 생기는데 클릭하면 해당 노드에 대한 주석을 달 수 있다.
또한 여러 노드를 드래그 한 뒤 C 키를 누르면 해당 노드들을 싸잡아서 하나의 주석을 생성한다.
해당 주석을 클릭해서 옮기고자 하면 그 주석에 속한 노드들도 같이 옮겨진다.

아무 노드를 선택하지 않은 상태에서도 C 키를 누르면 주석이 생긴다.

이러한 모든 주석을 옮겨서 다른 노드 위에 포개면 포개진 노드도 해당 주석을 옮길 때 따라 옮겨진다. 이것은 주석끼리도 마찬가지이다.

노드와 노드를 연결하는 선을 더블클릭하면 더블클릭한 자리에 변곡점이 하나 생기게 된다. 이 변곡점을 Ctrl 을 누른 상태에서 마우스로 클릭해 움직이면 선의 모양을 바꿀 수 있다.

Event BeginPlay  노드 한 개와 Print Text 노드 두 개 (편의상 A, B 라고 부름) 에 대하여 Event BeginPlay 와 A 가 연결 되어 있다고 할 때
- 연결을 끊고 싶은 경우;  해당 노드의 Exet 우클릭 -> Break This Link 로 해당 연결을 없앨 수 있다.
- 연결을 A 에서 B 로 옮기고 싶은 경우; 연결을 끊고 새로 드래그 해서 연결 또는 A Exec 을 Ctrl + 좌클릭 -> B 의 Exec 으로 드래그

연결 되어 있는 노드들이 어지러이 놓여있을 때 정리하기를 원하는 노드들을 선택하여 단축키 q 를 누르면 같은 선상에 놓이도록 정렬한다. 하지만 이때 input- output 순서대로 좌우로 정렬하지는 않는다.

4. 변수 타입

Blue Print 탭의 좌측 패널에서 Variables 칸의 + 버튼을 클릭하면 변수가 하나 생성되고 해당 변수의 이름과 변수의 자료형을 지정할 수 있다. 초기에 지정한 뒤 자료형은 언제든지 바꿀 수 있지만 변수명은 해당 변수를 선택한 뒤 F2 키를 눌러 바꿀 수 있다.

변수를 추가 한 뒤 컴파일과 저장을 하면 우측에 선택한 변수에 대한 Details 패널을 띄운다.

이때 Boolean 형의 경우 Details -> Default Value 에서 체크박스에 체크하는 행위로 true 인지 false 인지를 정한다.

Byte, Integer, Integer64 모두 정수를 나타낸다. Byte = 1byte, Integer = 4byte, Interger64 = 8byte
float 은 소수를 나타낸다. 4byte
Name, String, Text 는 모두 문자열을 나타낸다. 
Name 의 경우 언리얼 엔진 내에서 객체나 속성을 식별하는 데 사용되는 간단한 문자열 자료형으로 액터의 이름이나 함수의 이름 등을 나타내는 데에 사용된다.
String 일반적인 문자열을 나타내는 자료형, 길이와 형태에 제한이 없고 다양한 연산과 변환을 지원한다. 주로 사용자 인터페이스, 텍스트 메시지, 파일 경로 등 다양한 문자열 데이터를 처리하는데 사용된다.   
Text 문자열과 유사하지만 다국어를 지원하고 메모리가 효율적으로 저장된다. 주로 게임 내에서 사용되는 텍스트 메시지, UI 텍스트, 알림 등 다국어 지원이 필요한 텍스트 데이터를 처리하는 데 사용된다.

Level 탭 -> Tools -> Localization Dashboard 클릭 -> 하나의 Texture 에 다국어로 table 을 만들어 관리할 수 있다.

5. Get, Set

Set : 메모리의 일정 부분에 사용자가 원하는 데이터를 저장(write)하는 것.
Get : 메모리의 일정 부분에 저장되어 있는 데이터를 사용자가 읽는(read) 것. ( 여기에서 읽는다는 의미는 사용자가 해당 데이터를 활용할 수 있는 조건을 만족시키는 것을 말한다.)

[ 노드 생성 ]
Integer 형 변수를 생성하고 Hp 로 변수명을 짓는다.
- 우클릭 -> Set Hp 검색 후 클릭 -> Hp 를 인자로 갖는 Set 노드 생성
- 우클릭 -> Get Hp 검색 후 클릭 -> 메모리에 저장된 Hp의 값을 읽는다.
Set 노드에는 Hp 의 값을 입력할 수 있는 칸이 있다. 해당 칸에 임의의 수를 입력한다.

Hp 를 클릭 후 Event Graph 로 드래그 하면 Get, Set 둘 중에 하나를 고를 수 있다. 하나를 클릭하면 해당 노드가 생성된다.
Hp 를 Ctrl + 클릭 -> Event Graph 로 드래그 하면 Get 함수가, Alt + 클릭 -> Event Graph 로 드래그하면 Set 함수가 생성된다.

[ interger 출력]
Print Text 노드 생성
event Begin Play 노드 -> Set -> Print Text 순으로 연결(link) 한다.
Get 함수를 Print Text 의 In text 에 연결한다. -> 연결 되는 동시에 그 사이에 To Text ( Integer) 노드가 새로 추가된다. 해당 노드는 Interger 자로형을 Text 자료형으로 변환시킨다.
컴파일 -> 저장 -> 시뮬레이션 재생 을 하면 Hp 에 저장한 임의의 수가 재생이 시작됨과 동시에 이전 hello world 와 같이 좌상단 에 출력된다.

Get 은 output(함수의 출력) 만 있는 반면 Set 은 Input (함수에 입력) 과 Output 둘 다 가지고 있는데 이때 Output 은 Input 값 그래도이다. 따라서 Get 함수를 Print Text의 In Text 에 연결하는 것과 Set 함수의 Output을 In Text 에 연결하는 것은 같은 결과를 보인다.



6. 사칙연산

우클릭 -> + (Add), - (subtract), * (multiply), / (divide)로 사칙연산 노드 생성 각 노드는 기본적으로 두개의 입력 값과 한 개의 출력 값을 갖는다. 
노드의 우측 아래 + 버튼을 누르면 입력 값을 추가(입력 핀을 추가)할 수 있다.
입력 값 pin 우클릭 -> Remove pin 클릭 으로 추가한 입력 pin 삭제
이 입력 핀에 직접 값을 입력할 수도 있고 다른 노드와 연결하여 값을 입력할 수도 있다.

나눗셈의 경우 정수 끼리의 나눗셈은 그 출력 값이 무조건 정수여야 하기에 예상값과 다르게 소수 부분을 완전히 탈락시킨 출력 값이 나올 수 있다. 때무에 정수 끼리의 나눗셈에서 정수를 나눗셈 노드에 입력 값으로 전달하기 전에 float 형으로의 convert 노드를 거치게 한다면 나눗셈의 출력 값이 float 이 되어 정확한 값이 나온다.

 
7. 비교 연산자 사칙연산과 사용 방법이 같다 bool 값을 반환한다.


8. 연습 문제 : 총알 재장전
사용 노드 left Mouse Button, Keyboard Events R, Format Text, Branch
Format Text 는 주어진 인자값을 출력하는 노드이다. 노드를 생성하면 주어진 빈칸에 출력할 문자열을 입력한다. 이때 문자열 중 { } 사이에 놓이는 문자열을 변수 삼아 해당 변수로 입력된 값을 문자열로 변화해 원래의 문자열과 하나로 반환한다.
Branch 는 입력되는 bool 값에 따라 link 가 나뉘어 지도록 설계된 노드이다.


9. 디버깅
디버깅 하고자 하는 노드를 선택한 뒤 F9 키를 누른다. -> 마찬가지로 취소할 때에도 F9 를 누르다.
F10 을 누르면 다음 노드로 이동한다.
integer 나누기 integer 은 반환값도 integer 이기 때문에 소수점 아래를 표기하지 않는다. 따라서 float 으로 나눠주어야 한다. 이때 interger 두 개를 모두 float 으로 변환하지 않고 하나만 float 으로 나누더라도 반환값이 float 이 된다. float 이 integer 보다 우선 순위이기 때문이다.

10. 논리 연산
not boolean 검색으로 not 논리 연산자 노드 생성. 입력된 boolean 값의 반대 값을 반환
and boolean 검색으로 and 논리 연산자 노드 생성. 두 입력된 값이 true 일 때 반환 값 true, 나머지 상황은 false
or boolean 검색으로 or 논리 연산자 노드 생성. 두 입력된 값 중 하나만 true 여도 반환 값 true, 두 입력 값이 모두 false 일 때 반환값 false

11. Branch, Swquence, Flip Flop
branch 노드 입력된 bool 값에 따라 선택지를 두 갈래로 나눠 준다.
b + left mouse click 하면 branch 노드가 생성된다.
sequence : 0부터 시작하는 n 개의  then 이 존재하고 하나의 then 이 종료되면 그 다음 then 이 이어서 실행된다.
s + left mouse click 하면 sequence 노드가 생성된다.
filp flop : 해당 노드를 실행할 때 맨 처음은 A 를 실행하고 그 다음부터 이전에 실행된 선택지와 반대의 선택지를 실행한다. 즉 해당 노드를 실행할 때마다 두 선택지가 번갈아 가면서 실행된다. 
해당 노드의 반환 값인 Is A 는 현재 실행되는 선택지가 A 인지 에 대한 bool 값이다.

12. Min, Max, Flap 
Max : 입력된 두 값중 더 큰 값을 반환 한다.
Min : 입력된 두 값 중 더 작은 값을 반환한다.
Clamp : 입력된 값이 특정 범위 안에 있는지 확인하고 범위를 벗어나면 해당 값을 해당 붐위의 최소값 또는 최대값으로 제한하는데 사용된다.
- 입력 인자 1. Value : 제한하려는 값을 나타내는 입력, 2. Min : 값의 최소 범위를 나타내는 입력, 3. Max : 값의 최대 범위를 나타내는 입력.
- 만약 value 가 Min 보다 작으면 clamp 는 min 을 반환한다, 만약 value 가 max 보다 크면 clamp 는 max 를 반환한다, 만약 value 가 min 과 max 사이라면 clamp 는 value 를 반환한다.

13. For Loop, While Loop

While Loop : condition 이 참이면 Loop body 쪽의 link 된 Node 를 실행한 뒤 다시 condition 을 체크한다. condition 이 거짓이면 Completed 쪽의 link 된 Node 를 실행한다. 

For Loop : 현재 index 가 first Index 와 Last index 사이이면 Loop Body 쪽 link 된 Node 를 실행한다.
- 입력 인자 1. First Index : 인덱스 시작 값, 2. Last Index : 인덱스 종료 값 즉, First Index 가 0 이고 Last Index 가 5 이면 0, 1, 2, 3, 4, 5 총 6번 Loop Body 가 실행된다.
- 출력 인자 1. Loop body : 현재 Index 가 First Index 와 Last Index 에 포함되면 실행한다.
2. Index : 현재 인덱스이다. Loop Body 쪽 Link 된 노드의 실행이 끝나면 1 증가한다.
3. Completed : 1증가된 현재 index 가 last index 를 초과 할 때 실행한다.

For Loop with Break : break 가 추가된 for loop 노드. Loop Body 쪽에 link 된 노드에 분기점을 추가하고 특정 분기를 Break 와 연결하면 loop body 에 실행된 노드의 결과가 break 와 link 된 분기라면 for 노드가 정지한다.

14. Gate, MultiGate, Do Oncee, Do N

Gate : 실행 여부를 조작한다.
- 입력 인자 1. Enter : 이전 node 와 연결 되며 현재 gate 가 Open 상태라면 Exit 를 실행한다, 2. Open :  gate 의 상태를 open 으로 변경한다. 3. Close : gate 의 상태를 Close 로 변경한다. 4. Toggle : 현재 게이트의 상태의 반대로 변경한다. 5. Start Closed : 체크한 경우 Gate 의 default 상태를 Close 로 하고 체크하지 않은 경우 Open 으로 한다.

MultiGate : 현재 인덱스에 따라 정해진 출력 부 노드를 실행한다. 한번 실행하면 정해진 조건에 따라 다음 인덱스로 넘어간다.
- 입력 인자. 1. Reset : 현재 인덱스를 Start Index 로 변경한다, 2. Is Random : 현재 인덱스를 임의로 설정하여 실행한다. 출력된 인덱스는 제외하고 나머지 출력되지 않는 인덱스 중에서 임의로 설정한다. loop 가 체크 된 경우 모든 인덱스가 출력된 뒤에 다시 모든 인덱스 중 임의로 한 인덱스를 설정한다, 3. loop 모든 인덱스를 출력한 뒤에 다시 그 과정을 반복할 지에 대한 체크. loop 가 체크 되지 않은 상태에서 모든 인덱스를 출력하면 그 뒤로 해당 MultiGate 노드는 아무런 출력을 하지 않는다, 4. Start Index 맨 처음 출력할 인덱스를 지정한다. Is Random 이 체크 되었다면 Start Index 가 맨 처음 출력 되고 나머지는 임의의 인덱스가 출력된다. Is Random 이 체크되지 않고 Loop 가 체크 되어 있다면 Start Index -> Last Index -> 0 -> Start Index -1 흐름으로 출력이 진행된다.

Do Once : 출력할 수 있는 기회가 단 한번이다..
- 입력 인자 1. Enter : Completed 출력을 한번 실행한다. 2. Reset : Do Once 가 한번 출력할 수 있는 상태로 만든다. 3. Start Closed 시작부터 출력할 기회가 없는 상태에서 노드를 생성한다.

Do N : Do Once 와 다르게 출력할 수 있는 기회의 수를 설정할 수 있다.
- 입력 인자 1. Enter : 출력 가능 Count 가 0 이 아니면 출력인 Exit 를 호출한다. Exit 를 호출한 뒤에는 출력 가능 count 를 1 감소시킨다. 2. N : 출력할 기회의 수를 설정한다. 3. Reset : 출력 가능 Count 를 N 에 설정한 수로 설정한다. 
- 출력 인자 1. Exit : Count 가 0이 아닐 때 Enter 로 입력이 들어오면 출력한다. 2. Counter : 현재 출력할 수 있는 기회의 수 를 출력한다.

15. Enum, Switch, Custom Event
- DevMap -> Content Browser -> All -> Content -> Folder 생성 (BluePrints) -> 해당 폴더에서 우클릭 -> Blueprints -> Enumeration 선택 (EState)
- EState 파일 더블클릭 으로 Enum 창 생성 -> ' + Add Enumerator ' 을 클릭하여 Enumerators 항목에 Enum 을 추가하고 각 enum 항목의 Display Name 을 Idle, Moving, Attack 으로 설정
- Event Graph 에서 Variables 추가 클릭 -> 자료형 검색에서 EState 검색 후 선택 -> 해당 자료형 이름 State 로 변경 -> 컴파일 한 뒤 해당 변수의 Default Value 를 보면 변수 State 에 설정할 수 있는 값들이 Idle, Moving, Attack 세가지 임을 확인 할 수 있다.
- 열거형을 비교하고 싶다면 Equal 이 아니라 Equal(Enum) 노드를 사용해야 한다.
Enum 에 대해 Get 을 한뒤 해당 Get Node 에 대해서 출력 부분 좌클릭-> 드래그 -> Actions taking a(n) EState Enum 창 생성 -> 해당 창에서 Switch 검색 -> Switch On Estate 선택하여 생성 -> Estate 의 모든 요소를 출력 분기로 나누는 노드이다.
- 우클릭 -> Add Custom Event 선택 -> Details 창에서 PrintChoice 로 이름 변경 이렇게 생성된 Custom Node 는 우클릭 -> 검색으로 추가로 생성이 가능하며 추가로 생성된 노드가 실행되면 Add Custom Event 로 만들어진 빨간색 Custom Node 가 실행된다.

16. Function
Level Blue Print 에서 생성하는 Function 은 맴버 함수이다.
BluePrints 폴더 에 우클릭 -> Blueprint -> Blueprint Function Library 선택 -> MyFuncLib 으로 이름 바꾸기
이 Blurprint function Library 에 생성되는 Function 은 C++ 의 Static Function 에 해당한다.
함수 노드를 클릭 -> Details 창의 Inputs 와 Outputs 에서 입력인자와 출력 을 정할 수 있다.

17. 복사와 참조

기본적으로 Bluprint function Library 에 생성되는 변수는 Local Variables 이다. 또한 Function 에 사용되는 인자 또한 Local Variables 이다. Level Blue Print 의 맴버 변수를 Function Library 의 함수에 인자로 주었을 때 blueprint function Library 에서 사용되는 인자값은 Level Blue print 의 맴버 변수가 아니라 Blueprint function Library 의 로컬 변수이다. 이를 복사 라고 한다. 그렇다면 Blueprint Function Library 에서 Level Blue Print 상의 맴버 변수 값을 바꾸기 위해서는 Blueprint Function Librayr 의 함수의 인자 값의 Details 창 -> input 의 인자를 선택하면 Pass-by-reference 을 체크하면 된다. 이렇게 되면 BluePrint Function Library 의 인자가 Level Blue Print 의 맴버 변수의 메모리를 직접적으로 가리켜(연결하여) BluePrint Function Library 의 인자 변수의 값을 변경하면 Level Blue print 의 맴버 변수의 값을 변경하는 것과 같다. 이것을 참조라 한다.

18. 디버깅

메뉴 바 -> Debug -> Blueprint Debugger 선택 -> Call Stack
디버깅 상태에서 f10 은 함수 단위로 넘어가고, f11 은 노드 단위로 넘어간다.
메뉴 바 -> Debug -> Blueprint Debugger 선택 -> 해당 Function 파일 이름 -> Breakpoints : 역대로 사용했던 모든 breakpoint 가 나열되어있다.

19. 동적 배열 이론

선형 구조 : 자료를 순차적으로 나열한 상태
- 배열 : 연속된 메모리 공간, 방을 추가하거나 축소하는 것이 불가능하다. 기존의 메모리 크기 와 추가할 메모리 크기를 더한 만큼의 메모리 크기를 새로운 메모리 공간에 할당하고 해당 메모리에 기존 배열을 복사한 뒤 추가한다.
integer 형 변수 추가 -> details -> variable Type 에서 우측 single, array, set, map 에서 array 선택 -> Default value 에서 해당 배열에 요소를 추가할 수 있다. 해당 요소는 인덱스로 나열된다.
배열 변수 -> ctrl + 좌클릭 드래그 -> 'Read the value of variable numbers' 노드 -> Get ( a copy ) 와 Get ( a Ref ) 두개가 있다. 이때 두 노드 모두 몇 번재 인덱스의 정보를 가져올지를 입력 인자로 준다.
배열 변수 -> ctrl + 좌클릭 드래그 -> 'Read the value of variable numbers' 노드 -> Length : 배열이 몇개의 요소를 가지고 있는지를 반환한다.
배열 변수 -> ctrl + 좌클릭 드래그 -> 'Read the value of variable numbers' 노드 -> For Each Loop : 해당 배열의 모든 요소에 대해 Loop Body 를 실행한다.
배열 변수 -> ctrl + 좌클릭 드래그 -> 'Read the value of variable numbers' 노드 -> Add : 새로운 값을 입력하여 배열에 요소 추가.
배열 변수 -> ctrl + 좌클릭 드래그 -> 'Read the value of variable numbers' 노드 -> clear : 모든 요소 삭제
배열 변수 -> ctrl + 좌클릭 드래그 -> 'Read the value of variable numbers' 노드 -> Contains : 찾고자 하는 요소의 값을 입력하면 존재 여부에 대해 boolean 으로 반환한다.
배열 변수 -> ctrl + 좌클릭 드래그 -> 'Read the value of variable numbers' 노드 -> Finds : 찾고자 하는 요소의 값을 입력하면 해당 값의 인덱스 번호를 반환한다. 없으면 -1 을 반환한다.
배열 변수 -> ctrl + 좌클릭 드래그 -> 'Read the value of variable numbers' 노드 -> Set Array Elem : 입력하는 Index 에 해당하는 요소를 입력하는 Item 값으로 설정, 배열이 갖고 있는 요소 수를 초과하는 인덱스를 설정하면 오류가 난다 하지만 Size to Fit 항목을 체크하면 기존의 요소 수를 초과할 경우 입력한 Index 에 맞게 배열을 증가 시킨 뒤에 해당 인덱스에 값을 설정한다.


- 스택 / 큐

비선형 구조 : 하나의 자료 뒤에 다수의 자료가 올 수 있는 형태
- 트리
- 그래프

20. 버블 정렬

21. 해시 테이블
메모리를 더 잡아 먹는데신 성능을 올리는 방식
기준을 정해서 데이터를 분리하여 관리하며 정해진 값을 기준에 맞게 정리(계산) 하는 과정을 hash 라고 한다.
변수를 추가하고 해당 변수의 detail -> value type  에서 Key 와 value 의 자료형을 정한다. 왼쪽이 key 오른쪽이 value 이다.
맵 자료형에 대한 여러가지 함수들
Add : key 와 value 를 설정한다. 기존에 존재하는 key 에 Add 함수를 적용하면 새로 생성하는 것이 아니라 기존 key 와 match 되어 있는 value 를 덮어쓴다.
Remove : 입력한 Key에 대한 map 요소를 삭제한다.
Find :  입력한 Key 에 대한 value 과 boolean(찾으려는 값이 있다면 true 를 없다면 false 를) 을 반환한다.
Clear : 모든 요소 삭제
Is Empty : 하나라도 값이 존재하는지에 대한 여부
Keys : 해당 map 의 모든 요소의 Key 값을 array 형태로 반환한다.
Values : 해당 map 의 모든 요소의 value 값을 array 형태로 반환한다.

22. 블루 프린트 클래스, 객체 지향
Level 에서 Actor 로 Cylinder 을 생성하고 선택한 채로 level blue print 에서 우클릭을 하면 'All Actions for this Bluprint' -> Call Function on Static Mesh Actor -> Create a Reference to Cylinder 를 선택한다.
우클릭 -> Get All Actors Of Class 로 모든 Actors 를 가져올 수도 있다.
하지만 위의 방법은 각 actor 들을 관리하기 힘들다.
따라서 각 Actor 별로 관리 되도록 한다
All -> Content -> BluePrints -> 우클릭 -> BlueprintClass -> Actor 선택 -> BP_Player 명명
BP_Player 를 더블클릭하면 Viewport, construction Script, Event Graph 창을 갖는 Asset: BP_Player 창이 뜬다.
클래스는 주물이다.
BP_Player 의 Component 에서 +Add를 눌러 여러 도형을 첨가한다.
Level 창의 Place Actors 에서 BP_Player 검색 및 생성 (3개 생성) -> BP_Player  창에서 모형 변경 -> Compile and save -> Level 창으로 돌아가면 3개의 BP_Player Actor 3개가 변경한 모형으로 적용되어 있다.

23. 멤버 변수와 멤버 함수
하나의 클래스에 속하는 변수와 함수
BP_Player 의 Functions 탭에서 맴버 함수를 , Variables 탭에서 맴버 변수를 추가할 수 있다.
Event Graph 에서 BP_Player 형 객체를 생성하는 방법 : Event Graph 에서 우클릭 SpawnActor None 선택 -> class 인자를 BP_Player 로 설정 -> Spawn Transform 인자를 드래그 -> Make Transform 선택 (위치 init 값 설정)
맴버 함수는 해당 클래스의 맴버 변수에 접근이 가능하다.
멤버 변수는 객체의 메모리 크기를 생각하면서 추가해야 하지만 맴버 함수는 그럴 필요는 없다. 맴버 함수는 code 영역 에 저장된다.

24. Is Valid
Event Graph 에서 우클릭 Spawn Actor None 생성
- class 인자에서 BP_Player 선택 
- Spawn Transform 인자에서 드래그 -> Make Transofrm 선택 
- Return Value 반환 갑 드래그 -> promote to variable  선택 : 해당 클래스형 변수 생성 (Player1 으로 명명)

Valiables 탭에서 BP_Player 형으로 변수를 생성한다 (Player2 로 명명)

SpawnActor BP_Player 의 Return Value 로 Player1 을 Set 라고 Player 1의 Set 노드의 반환 값으로 Player2 를 Set 할 때 Player1 과 Player2 의 관계는 어떻게 될까?

실험 : BP_Player 에 integer 형 멤버 변수 HP 를 추가 (default value : 0)하고 위의 Player2 의 Set 노드에 다음으로 연결하여 HP Set 노드를 생성한다(Set value : 50). 이렇게 되면 player1 의 HP 가 50 이 되는 걸까? 아니면 Player 2의 HP 가 50 이 되는걸까?
결과는 두 BP_Player 객체의 HP 모두가 50 으로 설정 된다.

변수를 생성할 때 사용자가 보는 변수는 생성된 메모리의 주소를 가지고 있다. 예를 들어 BP_Player 클래스로 만든 변수 Player1 은 djelsrkdp 메모리 어딘가에 할당 된 BP_Player 크기의 메모리의 주소를 갖고 있는 것이다.
즉 Variables 탭 상에서만 추가하고 메모리를 초기화 해주지 않으면 해당 변수를 사용할 수 없다.
따라서 
SpawnActor BP_Player 의 Return Value 는 할당된 BP_Player 형 크기의 메모리의 주소이고 해당 주소를 Player1 에 설정한 것이 된다. 또한 Set Player1 노드에서 반환하는 값 조차 해당 주소이고 그 주소를 Player2 에 설정하고 있으므로 Player 1 과 Player 2 는 같은 메모리를 가리키고 있는 것이 된다.
즉 Player1 과 Player2 둘 다 Hp 를 50 으로 설정한 것이 아니라 하나의 메모리 주소에 대해 HP 를 50 으로 설정하였고 해당 메모리 주소를 Player1과 Player2 가 동시에 가리키는 것이다. 

위와 같이 변수에 메모리 주소를 초기화 하는 것은 중요하다. 이렇게 해당 변수가 메모리 주소를 가지고 있어 이용가능한지를 판별하려 할 때
 Is valid 노드를 사용한다. c++ 에서 해당 변수가 nullptr 인지 주소 값을 가지고 있는지 판단하는 것과 같다.

25. 상속
여러 클래스들이 공통된 분모가 많을 때 관리를 쉽게 하기 위해서 공통분모로만 이루어진 부모 클래스를 만들고 해당 부모 클래스에서 여러가지 변형시킨 자식 클래스를 만들어 관리할 수 있다. 이 부모 클래스로부터 그 구조를 그대로 받아 추가 하거나 변형시키는 것을 상속이라고 한다.
BP_Archer, BP_Knight,, BP_Mage 클래스 생성
이미 만들어진 class 간에 상속 관계 설정하는 법 : 자식이 될 클래스(ex. BP_Knight)의 Asset 창 -> Class Setting -> Details 탭 -> Class Options -> Parentㅇ Class 에서 BP_Player 로 설정 (BP_Knight가 BP_Player 를 상속 받았다.)

새로 만드는 경우
Pick Parent Class 창의 All Classes 에서 상속을 하고자 하는 부모 클래스를 지정한다. (ex. BP_Player)

26. 캐스팅
BP_Player 클래스와 그를 상속 받는 BP_Knight 를 추가한다.
-> SpawnActor BP_Knight 노드 생성 -> output 드래그 -> Promote to variable 로 클래스 형 변수 생성
이때 Valiables 에서 BP_Player 형으로 변수를 생성하고 위에서 생성된 Knight Set 대신에 Player Set 에 SpawnActor BP_Knight 의 반환값을 입력한다.
이때 knight 클래스 형에서 player 클래스 형으로의 형 변환이 이루어진다.
이때 knight 에만 존재하는 맴버 변수나 맴버 함수에 대해 소실되는 것이 아닌가? 자식 객체가 부모 객체에 추가 되는 건데? 와 같이 생각할 수 있지만
단순히 주소 값을 넣는 것이기 때문에 데이터의 소실 등을 일어나지 않고 단순히 접근 가능한 멤버 변수나 멤버 한수가 달라질 뿐이다.
위와 반대로 SpawnActor BP_Player 노드의 반환 값을 Knight Set 노드에 입력할 수는 없다.
즉 자식 클래스는 부모 클래스에 대입할 수 있지만 부모 클래스를 자식 클래스에 대입할 수 없다.

27. 은닉성(캡슐화)
클래스를 사용할 때 해당 클래승의 멤버 변수나 멤버 함수 중에 겉으로 드러내서 자유롭게 접근 및 사용할 수 있도록 허용한 것이 있고 그와 반대로 숨긴것이 있다.
맴버 변수의 경우 variables 에 선언된 변수의 우측에 눈을 뜨고 있는 이모지면 level 시뮬레이션의 detail 에서 확인 또는 수정할 수 있지만. 눈을 감고 있는 이모지면 Level 시뮬레이션의 detail 에서 해당 변수 정보가 보이지 않는다.
멤버 변수 -> detail 탭 -> Variable -> Private 를 체크하면 Level blue print 에서 해당 멤버 변수에 접근 할 수 없다. 즉 해당 멤버 변수는 속한 클래스 창 내에서만 확인 및 사용이 가능해진다.
멤버 함수의 경우 -> detail 탭 -> Graph -> Access Specifier 에서 Public, Protected, private 로 설정(접근 지정자 설정)할 수 있다. (default 는 public)
이때 public 은 누구나 접근 가능, protected 는 자기 자신 또는 자식 클래스에서 접근 가능 , private 는 자기 자신 내에서만 접근 가능하다.
이렇게 멤버 변수나 멤버 함수의 숨김 정도를 설정하는 성질을 은닉성이라 하고 은닉된 멤버 변수와 멤버 함수를 활용하는 로직을 짜는 것을 캡슐화 라고 한다.

28. 다형성
부모 클래스의 함수를 자식이 사용하려고 할때 그 함수를 변형시키고 싶다면?
BP_Monster 클래스와 그를 상속 받는 BP_Orc 클래스 추가 -> BP_Monster 에 멤버 함수로 Shout 를 추가. -> BP_Orc 의 Functions tap 에 마우스를 갖다 개면 생기는 Override 드롭박스 클릭 -> Shout ...BP_Monster 선택
위의 과정으로 BP_Orc 에는 Shout 노드 ->Parent : Shout 노드 형태의 Shout 함수가 생긴다.
(Parent : Shout 노드가 보이지 않는다면 Shout 노드 우클릭 -> Add Call to parent function 선택 으로 추가할 수 있다.)
이때의 Parent : Shout 노드 는 BP_Parent 의 Shout 함수 호출을 뜻한다.

그렇다면 Spawn Actor BP_Orc 노드 추가 -> Variables 에서 BP_Monster 형으로 변수 'Monster' 추가 -> Spawn Actor BP_Orc 의 반환형을 Set Moster 에 입력 -> Set Monster 의 반환형에 대해 Shout 노드 호출
-> 결과는 BP_Orc 의 Shout 를 호출한다. 이를 통해 클래스 변수와 상관 없이 메모리가 어떤 타입으로 생성되는지 BP_Orc 인지 BP_Monster 인지가 중요하다는 것을 알 수 있다.

29. 인터페이스

다중 상속 이란 : 객체가 여러 부모 객체를 상속받는 것. 

인터페이스를 파서 약속을 해야한다...?
BP_Player 클래스 생성. BP_Player 를 상속 받는 BP_Orc 클래스 생성.
All->Content-> Blueprints 폴더 우클릭 -> Blueprints -> Blueprint Interface 선택

blueprint interface 에서는 node를 만들 수 없다. interface 내에서 기능을 구현하는 것이 아니라 interface 에서 정의 하는 함수들을 해당 interface 를 구현하는 클래스가 사용가능하다라는 정의에 가깝다.
대신 함수의 input 과 output 은 추가할 수 있다.

BP_Orc -> Class Settings -> details -> interfaces -> implemented interfaces -> Add -> 추가하고자 하는 interface 명 검색 및 선택 -> Compile
위의 과정을 거치면 BP_Orc 클래스에서 My Blueprint -> Interfaces 에 추가한 interface 의 함수들이 추가되어 있다.

위의 interface 에서 함수를 생성할 때 반환 값이 있다면 함수로 인식이 되지만 반환 값이 없다면 Event 로 인식이 된다.
따라서 interface 에서 반환 값이 있는 함수를 설정하여 클래스 의 interraces 탭에서 함수로 인식 된다면 해당 함수는 클릭해서 창을 띄웠을 때 로컬 변수를 갖을 수 있는 것을 확인 할 수 있다.
이벤트에서는 딜레이 함수를 사용할 수 있지만 함수의 경우 호출되면 자신의 모든 과정을 다 호출해야 하기 때문에 딜레이 함수를 사용할 수 없다.

event 는 함수의 호출을 위한 트리거의 역할을 한다고 보면 된다.

Level blueprints 의 evnet Graph 에서 변수를 추가하고 그 자료형을 IFlyable 로 정한다 (monster 로 명명).  SpawnActor BP_Orc 의 반환 값을 monster 변수의 Set 에 입력한다. -> monster Set 함수의 반환 값으로 IFlyable 에서 정의한 함수 및 이벤트를 호출할 수 있다.

30. 구조체
Blueprint 폴더 -> 우클릭 -> blueprints -> structure 선택하여 생성
structure 파일 클릭해서 창열기 -> Add Variable 로 구조체의 요소 추가.
level blueprint 의 valiable 에서 변수를 생성할 때 생성한 구조체로 변수 생성 할 수 있다.
생성된 구조체형 변수의 details 창을 보면 구조체의 요소들의 기본 값을 수정할 수 있는 것을 볼 수 있다.
생성된 구조체형 변수의 Get 노드의 반환 값에서 구조체 형 이 아닌 구조체 각 요소를 각각 반환하고 싶다면 : 반환 값 우클릭 -> Split Struct Pin 선택 
반대로 각 요소말고 하나의 구조체형으로 반환하고 싶다면 반환 값 우클릭 -> recombine struct pin 선택
이는 구조체형 변수의 Set 노드의 입력 값 및 반환 값 에서도 마찬가지로 작용한다.

Get 함수 자체에서 말고 반환 값을 드래그 -> Break (구조체형 변수명) 선택 으로 구조체를 요소별로 나누는 노드를 생성할 수 있다.
특정 값만 세팅하고 싶은 경우 Get 함수 반환 값을 드래그 -> Set members in (구조체형 변수명) 선택, 생성된 노드를 클릭하면 details -> Default Category 에서 세팅을 원하는 요소를 체크하면 된다.

동일한 구조체형으로 생성된 변수 structVal1, structVal2 에 대해서 structVal2 의 Set 함수에 structVal1 의 Get 함수의 반환 값을 입력하는 경우 값의 복사가 이루어진다. 이후에 structVal2 의 요소를 바꾸어도 structVal1 에 영향이 가지 않는다.

구조체 형을 입력 값으로 받는 함수가 있다고 할 때 ; 해당 함수의 입력 값 드래그 -> Make (입력값 구조체명)선택
함수에 입력으로 쓰일 구조체 의 요소 값을 세팅할 수 있다.

31. 벡터 이론

32. 로컬 좌표와 월드 좌표
Shapes 를 생성하고 simulatio 창의 윗쪽 메뉴를 보면 지구본 또는 xyz 축의 원점에 접한 직육면체 이모지를 볼 수 있는데 전자는 현재가 월드 공간임을, 후자는 로컬 공간임을 나타낸다.

두 개의 Shapes 를 생성하여 하나는 ParenShape 라고 하고 하나는 ChildShape 라고 할 때 ChildShape 를 ParenShape에 속하게 만든다. 
이때 ParentShape -> Details -> Transform -> Location 이 Relative 또는 World 가 값이 똑같지만 (기준이 world space 의 원점이기 때문)'
ChildShape -> Details -> Transform -> Location 에서 Relative 와 World 가 값이 다르다 이는 Relative 일 때는 기준되는 원점이 ParentShape 의 위치이고 World 일 때에는 기준되는 원점이 world 좌표상 의 원점이기 때문이다.

33. Vector : 특정 방향으로의 Shape 의 이동
두 개의 Shape 를 생성하여 하나를 Player, 다른 하나를 Monster 라 명명한다.
Shape 선택 -> Level Blueprint Edit 에서 우클릭 -> Creator a Reference to Shape 명 으로Player 와 Monster 의 reference 를 Level Blueprint Edit 에 생성한다.
Vector 형 으로 2개의 변수 생성 (각각 Direction, NormalizedDirection 으로 명명)
여기에서 Vector 변수의 Get 노드를 생성하고 반환 인자를 우클릭 -> split struct pin 선택 하면 반환 값이 X, Y, Z 로 나뉘어 진다. 이를 통해 Vector 는 float 요소가 3 개인 structor 라는 것을 알 수 있다.
Player가 Monster 에 다가기 위해서는 player 로부터 Monster 에 대한 방향 을 알아야 하는데 이는 Monster 위치 벡터 - Player 위치 벡터 계산으로 구할 수 있다.
Player Ref 노드와 Monster Ref 노드 의 반환 값으로부터 Get Actor Location 노드를 생성한다. 
Monster 의 Get Actor Location 노드의 반환 값 에 Player 의 Get Actor Location 노드의 반환 값을 뺀다(Substract 노드).
위의 Substruct 노드의 반환 값 (player 가 Monster 를 향한 방향벡터)을 Direction 변수의 Set 노드에 입력한다.
Direction 의 Set 노드의 반환 값에 Normalize 노드 생성 (방향벡터를 단위벡터로 변환) -> Normalize 노드 반환값을 Normalized Direction 변수의 Set 에 입력
Event Tick 노드 생성 (매 프레임 마다 호출된다.)
player Get 노드 생성 및 반환값으로 Set Actor Location 노드 생성
Normalized Direction의 Get 노드 생성 -> 해당 노드 반환 값을 multiply 노드에 입력한다. 이때 남은 입력 값은 1.0, 1.0, 1.0 으로 한다.
Get ActorLocation 의 반환 값과 위의 Multiply 반환 값을 더한 뒤 Set Actor Location 의 New Location 인자에 입력한다.
 Player 의 Get 함수의 반환 갑을 Set Actor Location 의 Target 에 입력한다.


위의 과정대로 컴파일하고 simulation 을 시작하면 실제로 움직이지 않는다. simulation 을 종료하면 Message Log 창이 뜨고 -> StaticMeshActor_2 : StaticMeshComponent0 has to be 'Movable' if you'd like to move. 로 경고가 뜬다. 이는 player 의 details-> Transform-> mobility 에서 Static 으로 되어 있기 때문이다. 이를 Movable 로 변경해줘야 한다.

이때 위의 과정에서 multiply 에서 1.0, 1.0, 1.0 로 고정값을 주었는데 사실 이 방법은 틀린 방법이다. 개인 컴퓨터 별로 프레임이 다른데 위와 같은 방법을 사용하면 사양이 좋은 컴퓨터에서 더 많이 움직일 것 아닌가. 때문에 Event Tick 의 Delta Seconds 를 이용하여 기존의 하드 코딩한 자리에 입력 한다.

player 와 monster 사이의 거리는 Direction 변수 에 대한 Vector Length 로 구한다.

33. rotator

player 가 회전해서 monster 를 바라보게 한다.

변수 자료형 목록을 보면 Transform 과 Rotator, Vector 를 볼 수 있다. 
Transform 변수를 선언하여 Get 함수를 생성한 뒤 그 반환 값을 Split Struct Pin 을 해주면 Location(Vector형), Rotation(Rotator 형), Scale(Vector형) 으로 나뉘어 지고 또 Split struct pin 을 해주면 각 반환 요소가 float 3개(x, y, z)로 나뉘어 진다는 것을 알 수 있다.
그렇다면 세 개 모두 Vector로 하면 될 것을 왜 Rotator 만 자료형이 별도로 존재할 까? 이유는 Rotator 가 x, y, z 이외의 w 요소를 가지고 있기 때문이다. 회전에서 w 없이 x, y, z 만 가지고 연산을 하려고 하면 짐벌럭 현상이 발생하기 때문이다.
Ratotor  변수를 Get 반환 값을 Quaternion 노드로 연결한뒤 해당 노드의 반환값을 split struct pin 해주면 x, y, z, w 네 개의 요소가 모두 보인다.
Plyer 와 Monster 의 Get 함수의 반환 값으로 Get Actor Location 을 생성하고 두 노드의 반환 값으로 Find Look at Rotation 노드를 생성한다. 
이 노드는 first 입력 인자가 Target 입력인자를 바라보게 끔하는 rotation 을 계산해 준다.
Player 의 Get 노드의 반환 값을 Target 으로하는Set Actor Rotation 노드를 생성한뒤 New Rotation 입력 인자에 Find Look at Rotation 의 반환 값을 입력해준다.

34. 슬라임 경주

blueprint 클래스 생성 -> BP_Slime1 명명 -> Add -> Sphere -> Sphere  을 드래그로 Defualt SceneRoot 에 덮어씌운다. (Sphere가 해당 클래스에서 가장 근원적인 요소가 된다.)
BP_Slime1 -> Components -> Add -> Collision -> Sphere Collision 생성 (Collider1 으로 명명)
Collider1 -> Details -> Shape -> Sphere Radius 로 충돌체의 크기를 조절할 수 있다.
충돌이 발생하면 Coillider1 -> Event Graph -> Event ActorBeginOverlap 노드가 실행된다.
기본 창 -> Place Actors -> Volumes -> Blovking Volume 선택 -> 직육면체의 충돌체 생성 -> 긴 바 형태로 변경
Bp_Slime1 class -> Deatils -> Collision -> Generate Overlap Events during Level Streaming 를 체크

- 여러개의 슬라임을 생성하고 random 값을 부여하여 다른 속도로 충돌체로 이동하도록 한다.
- 각 슬라임에 이름을 부여하여 충돌체와 충돌하면 자신의 이름을 출력할 수 있도록 한다.
- 여러 개의 슬라임중 무엇이 일등하였는지를 출력할 수 있도록 한다.

35. Event Dispatcher

위의 슬라임 경주에서 Event Tick 에 연결하여 슬라임 객체의 충돌에 대한 데이터를 저장했다. 이 연산 과정은 매 프레임 발생하게 되는데 Slime  객체가 overlap event 를 호출할 때에만 위의 연산 과정을 호출하면 훨씬 연산 비용을 줄일 수 있다.

slime class -> My Blueprint -> Event Dispatchers -> + -> OnArrived 로 명명
기본 event graph -> Valiables -> Add -> BP_Slime 형 Array 로 변수 생성 및 Slimes 명명
기본 event graph -> event Beginplay -> Get All Actors Of Class (Actor Class -> BP_Slime 설정) 노드 -> Set Slimes 노드 -> For Each Loop  노드 -> Array Element -> OnArrived 검색 -> Assign On Arrived
BP_Slime -> Evnet Diapatchers -> OnArrived 선택 -> Details -> Inputs -> + 버튼 클릭 -> string 형 변수 Name 으로 명명
BP_Slime -> Event Graph ->Event Actor BeginOverlap 노드 -> Call On Arrived 노드
위의 Call On Arrived 노드가 호출되면 기본 Event Graph 의 Bind Event To ON Arriveㅇ 노드 -> OnArrivedEvent 이벤트 가 호출된다.

더이상 해당 이벤트를 사용하지 않는 경우 Array Unbind Event from on Arrived
On ArrivedEvent 를 Unbind Event From on Arrived 에 연결



