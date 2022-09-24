- 게임에서 하나의 프레임이 기준이 된다면 1초당 다른 프레임을 지원하는 두 컴퓨터에 대해서 공정하지 않을 수 있다. 
- 한프레임당 공격을 1회 실행 한다면. 1초에 높은 프레임을 가진 컴퓨터가 그만큼 공격을 많이 한다는 뜻이기 때문이다. 
- 따라서 게임은 절대적 시간을 기준으로 같은 입력에는 같은 실행이 이루어 져야 한다.
- 만약 게임에서 1의 거리를 이동하려는 목적의 입력에 대해서 1프레임 동안 이동이 한번 이루어 진다고 할 때 144프레임의 컴퓨터는 144 * x = 1(이동거리) 가 되어야 하고, 60의 프레임의 컴퓨터는 60 * y = 1 가 되어야 한다.
- 여기에서 x와 y 가 바로 DeltaTime 이다.
- 이 방정식을 조정하여 DeltaTime을 기준으로 식을 맞추면, ' (1초 동안 이루어지는 실행) / (프레임) 수 ' = DeltaTime  이다.  


각각의 <span style="color:orange">컴퓨터의 메인보드에는 고해상도 타이머가 존재</span>한다. <span style="color:orange">이 타이머는 일정한 주파수(1초에 각 타이머의 성능에 맞는 진동수)를 가지고 있다.</span>

## QueryPerformanceCounter

```c++
BOOL QueryPerformanceCounter(LARGE_INTEGER *lpPerformanceCount);
```

매개변수로 넘어간 변수에 '함수가 호출된 시점의 타이머 값'을 넘겨준다.


QueryPerformanceFrequency

```c++
BOOL QueryPerformanceFrequency(LARGE_INTEGER *lpFrequency);
```

매개변수에 현재 시스템의 고해상도 타이머의 주파수(1초당 진동수)를 반환한다.

<span style = "color:yellow">DeltaTime(프레임 사이 시간) = </span>
<span style = "color:yellow">(현재 프레임의 누적 진동수 - 지난 프레임의 누적 진동수) / 초당진동수 </span>

예를 들어 한 프레임 사이의 진동수가 2천만이고 해당 컴퓨터의 초당 진동수가 천만이면 하나의 프레임에 걸리는 시간 즉, DeltaTime 은 2초 이다.