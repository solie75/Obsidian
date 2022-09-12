오직 한 개의 클래스 인스턴스만을 갖도록 보장하고, 이에 대한 전역적인 접근점을 제공한다.
예를 들어 Timer 클래스는 실시간으로 시간과 FPS 를 계산하는 클라스이기 때문에 실행 초기에 인스턴스를 하나만 생성해서 계속 업데이트 시킨다. 이때 현재 시간을 이용하여 구현해야 하는 기능이 많이 때문에 이 Timer 인스턴스에 대한 접근을 전역적으로 해야 한다. 이럴때 싱글톤을 사용한다.

```c++
#define SINGLE(type) public:\
static type* GetInst()\
{\
	static type mgr;\
	return &mgr;\
}\
private:\
	type();\
	~type();
```

```c++
class SingletonClass {
	public:
		static SingletonClass* GetInstance() {
			static SingletonClass pSingleton;
			return pSingleton;
		}
		
	private;
		FileSystem(){}
} // 데이터 영역에 만들어 놓고 주소만 쏴주는 방법
```
1. 클래스의 생성자를 private 선언하여 외부에서 생성 불가능하게 만든다.
2. 클래스의 private 에 접근이 가능한 맴버 함수를 만들되 객체없이 호출이 가능해야 한다.
3. 

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

정적 맴버 함수의 특징
- 객체 없이 호출이 가능하다. 
- this 사용이 불가능하다.
- 속해있는 class 의 private 부분에 접근이 가능하다.. -> private 부분의 생성자 에 접근이 가능하다.

```c++
int Test()
{
	static int i = 0;
	++i;

	return i;
}

int main()
{
	for(int ii = 0; ii < 5; ++ii)
	{
		Test();
	}
	return 0;
}

// 여기에서 i의 결과 값은 5이다. 정적 변수는 data 영역에서 존재하기 때문에 초기화 부분이 다시 호출 되더라도 기존의 값에 초기화 값이 대입 되지 않는다. (최초 초기화)
```