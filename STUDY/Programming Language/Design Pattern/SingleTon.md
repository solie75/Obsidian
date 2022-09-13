<span style="color:orange">오직 한 개의 클래스 인스턴스만을 갖도록 보장하고, 이에 대한 전역적인 접근점을 제공한다.</span>
예를 들어 Timer 클래스는 실시간으로 시간과 FPS 를 계산하는 클라스이기 때문에 실행 초기에 인스턴스를 하나만 생성해서 계속 업데이트 시킨다. 이때 현재 시간을 이용하여 구현해야 하는 기능이 많이 때문에 이 Timer 인스턴스에 대한 접근을 전역적으로 해야 한다. 이럴때 싱글톤을 사용한다.

## 데이터 영역을 사용하는 싱글톤

```c++
class MyCore
{
public:
	static MyCore* GetInstance()
	{
		static MyCore core1;
		return &core1;
	}
private:
	MyCore();
	~MyCore();
}
```
1. private 영역에 클래스 생성자 선언
2. -> 외부에서 생성할 수 없으므로 객체 없이 호출 할 수 있는 정적 맴버 함수 사용
3. 정적 변수로 데이터 영역에 싱글톤 생성
4. 사용자가 원할 때(더이상 사용하지 않아 객체게 쓸모없어 졌을때 등) 객체를 삭제할 수 없다는 단점이 있다.

##  힙 데이터 영역을 사용하는 싱글톤


```c++
// 동적 할당 다른 버전
class MyCore
{
public :
	static MyCore* GetInstance()
	{
		static MyCore* MyCorePtr = nullptr;

		if(MyCorePtr == nullptr)
		{
			MyCorePtr = New MyCore;
		}
		return MyCorePtr;
	}

	static void Delete()
	{
		MyCore* MyCorePtr = GetInstance();

		delete MyCorePtr;
		
		MyCorePtr = nullptr;
	}
private :
	MyCore();
	~MyCore();
}
```
	1.정적 맴버 함수 속 정적 변수를 선언한다.(GetInstance가 재호출 되더라도 MyCorePtr의 값은 변하지 않는다.)
	2. 정적 변수인 MyCorePtr 을 nullptr로 초기화한다.
	3. 조건문으로 현재 할당 되어있는 상태인지 확인하고 아니라면 새로 동적할당한다.

```c++
class CEngine
{
private:
	static CEngine* m_pInst;
	
public:
	static CEngine* GetInst()
	{
	// 정적 맴버 변수는 Data 영역에 하나만 존재하기 때문에 this를 알 필요가 없다. 정적 맴버 함수에서도 접근이 가능하다.
		if(nullptr == m_pInst) // 한번도 만들어 진적이 없었다면
		{
			m_pInst = new CEngine; // 엔진 객체 동적 할당.
		}
		return m_pInst;
	}

	static void Delete()
	{
		if(nullptr != m_pInst)
		{
			delete m_pInst;
			m_pInst = nullptr;
		}
	}
}

//
cpp 파일에서 CEngine* CEngine::m_pInst = nullptr; 로 초기화 해줄것.

// 나중에 지워주어야 한다. 
CEngine::Delete();


```

1. 정적 맴버 변수에 