# DX9 와 DX11 의 설계 차이

Direct9 까지의 특징

API 를 설계할 때 항상 싱글 코어를 고려한다.

API 자체가 GPU 의 정점 처리 기능을 규정한다.

언제나 'vertex input -> rasterizer -> 픽셀 처리 -> display' 라는 큰 단계를 API 레벨에서 정의하고 있었다. 그리고 이것을 Fixed pipeline 이라고 불렀다.

Direct 11 부터의 특징

![[Pasted image 20230310140832.png]]
<direct11 의 pipeline>

Directx11 의 큰 특징 3개

- GPU Tessellation
- Compute Shader
- Multi-threaded rendering

Directx11 은 멀티코어(CPU)와 GPU 에게 최대한 많은 일을 시킬 수 있는 구조로 하드웨어를 구성하고, 그것을 기반으로 하여 런타임과 API 를 설계한다. 

Directx9 까지 API 가 변화를 주도했다면 
Directx11 부터는 하드웨어가 주도하고 있다고 할 수 있다. 그에 따라 API 도 또한 더욱 하드웨어에 가깝게 설계되었다.

# Command 생성

API 를 통한 호출은 결국 하드웨어가 인식할 수 있는 command 형태로 전달 되어진다.
![[Pasted image 20230310141541.png]]

Dx9 까지는 커맨드를 생성할 수 있는 인터페이스가 오직 하나로 위의 그림의 모든 과정을 하나의 인터페이스가 처리했다. 그리고 그렇게 저장된 커맨드 들을 그래픽 카드가 순차적으로 실행하였다.
IDirect3DDevice9 와 같은 인터페이스가 그러한 역할을 하였다. 해당 인터페이스는 실제 API 로 하드웨어에 명령 수행을 지시하기 보다 'API 를 하드웨어가 이해할 수 있는 command 로 변환해 주는 것에 가까웠다. 또한 그 command를 통하여 실제로 하드웨어가 실행을 한다.' ^a884cf

하드웨어가 이해할 수 있는 command 는 드라이버가 알 고 있다. 이 드라이버는 Core API 나 런타임과 통신한다.
텍스쳐 로딩, 메모리 생성, 바인딩 등의 API 를 사용하면 이와 같은 커맨드를 'CPU'가 생성한다.

cpu 의 코어수가 늘어나면서 동시에 모든 코어가 작동시킬 대안으로 멀티 스레드 기반의 렌더링 작업과 Compute shader 가 등장하였다. 여기에서 멀티스레딩 기반의 렌더링은 CPU 에게 렌더링 작업을 분산 시키는 것이고 Compute Shader 는 GPU 를 CPU 처럼 활용하는 것이다.

# Resource 처리 API

- Thread unsafe API
멀티 스레드들이 동시에 호출할 수 있지만 그것에 대한 안정성에 대해서 전혀 보장되지 않는다.

- Thread safe API
어떠한 동기화 모델 (..?) 에 의하여 API 가 보호된다. 여러 스레드에서 API 를 호출하더라도 그것들의 안정적인 동작이 보장된다. 이는 동기화 작업이 별도이기에 느려질 수 있다.

- Free threaded API
여러 스레드에서 호출되고 어떠한 동기화 모델 없이도 API 가 안정적으로 동작한다는 것을 의미한다. 때문에 아주 높은 성능 향상을 얻는다.

# Resource 처리

- DX9 까지

texture loading 을 위하여 우선 Loader Thread 가 특정 파일을 읽는다. 이 파일을 바탕으로 실제 메모리를 할당하기 위해서는 Device 인터페이스를 통해야 한다. Directx 에서 resource 관련 메모리를 할당하는 것은 Device 인터페이스를 통해서만 가능하기 때문이다. [[MultiThreading and Deffered Context#^a884cf]]
멀티 스레드 기반의 리소스 로딩은 실제로는 많은 부분이 메인 스레드(Render Thread 등) 에 의존하여 작업이 수행된다.

resource 를 생성하기 위해서는 반드시 Create, Lock & Unlock 작업이 필요하다.

- DX11 부터

현재 DirectX 에서 사용하는 대부분의 리소스 작업은 Free Threaded 이다. 리소스 처리를 위해 특정한 동기화 처리가 API 레벨에서는 필요하지 않다. -> 개발자의 몫
Directx11 부터 Free threaded 한 API 와 그렇지 않은 API 는 구별되며 리소스 처리는 Free threaded 의 한 부분에 존재한다. 이 Free threaded 의 경우 커맨드의 생성 순서가 중요하지 않기 때문에 빠른 속도로 처리할 수 있다. 메인 스레드가 별도로 다른 작업을 수행하여 커맨드 생성 작업이 있다고 해도 그것과 별개로 커맨드 생성 작업을 수행할 수 있다.
리소스 처리를 이렇게 분리하는 것이 멀티스레드 기반의 렌더링의 초석이 된다.

# Deferred Context

멀티스레드 기반의 렌더링을 위해서 Directx 11 은 다음의 세가지 를 고려한다.
- 비동기적으로 자유스러운 리소스 로딩과 관리 (이는 렌더링과 동시에 수행 될 수 있어야 한다.)
- 멀티스레드 형식으로 렌더링 커맨드 생성. (여러 스레드로 나눠서 렌더링 작업을 할 수 있는 경우)
- Display list 의 지원

이 중 첫번재 리소스에 관해서는 위에서 설명되었다.

### Device 의 분리

DX 9 까지의 GPU 관련 내용의 처리는 Device 인터페이스를 통해서 수행되었다. 즉, Devcie 인터페이스를 통해서만 command를 생성할 수 있었다. 오직 싱글 코어 기반으로 설계되었기 때문에 다음의 그림과 같은 상황이 연출되었다.
![[Pasted image 20230311114103.png]]

고로 DX 11 부터는 Device 인터페이스에 집중된 작업을 분리할 방법이 필요하게 되었고 그에 따라 다음의 구조가 되었다.

![[Pasted image 20230311131031.png]]

DX 11 부터 개발자가 다루어야할 커맨드 생성 인터페이스는 다음 3가지 이다.

1. Device : Free threaded  API 만을 사용하는 인터페이스 (주로 리소스 포함. 버퍼, 텍스쳐, 쉐이더 등)
2. Device Context : 실제 렌더링과 관련된 인터페이스
	1. immediate context : 반드시 한개만 존재해야 하며 실제 렌더링과 관련된 커맨드를 생성하는것과 동시에 생성한 커맨드를 그래픽 카드로 보내는 일도 한다.
	2. Deffered Context : 이 인터페이스의 유무가 멀티스레드 기반의 렌더링과 일반적 렌더링을 구별하는 기준이 된다.

쓰임 때문에 분류되어있지만 Immediate context 와 Deffered Context 의 인터페이스는 ID3D11DeviceContext 이다.
실제 맴버 변수 선언을 예를 들어보면 다음과 같다.
```c++
ID3D11Device* m_ipGPU;
ID3D11DeviceContext* m_ipImmediateContext;
ID3D11DeviceContext** m_ippDeferredContextArray;
```
동일한 인터페이스를 사용하는 것에 대하여 'Deferred Context 는 Immediate Context 의 모든 기능을 지원한다는 의미'로 생각하면 된다.

결국 Device Context 는 다음의 구조로 확장 될 수 있다.
![[Pasted image 20230311141526.png]]
멀티스레딩 기반으로 렌더링을 한다는 것은 멀티스레딩 기반으로 커맨드를 생성해서 이들을 순차적으로 그래픽 카드로 보내는 것을 의미한다.

# Deffered Context

하나의 응용프로그램에서 여러 Deffered context 가 생성될 수 있으며 하나의 스레드에 하나의 Deferred Context 가 사용될 수 있다. 해당 스레드는 thread unfafe 이다.
이와 같이 하나의 스레드에 할당 된 Deferred Context 는 Display List 를 생성한다.
Display List 는 GPU 가 바로 처리 가능한 커맨드를 모아둔 버퍼라고 할 수 있다. 일반적으로  CPU 가 커맨드를 생성하는 시간이 오래 걸리기 때문에 Display List 로 변경예정이 없는 커맨드를 미리 생성하여 모아두면 성능을 개선할 수 있다.

# Remark

정리하면 Deferred Context 는 각각의 스레드에 의해서 DisplayList 를 생성하고, 이들을 그래픽 카드에 보내기 위해 버퍼에 저장한다. 실제로 그래픽 카드에 커맨드를 보내기 위해서는 반드시 Immediate Context 를 통해햐 하며 이때 Immediate Context 를 통해서 직접적으로도 커맨드를 생성할 수 있다.

# Reference

https://vsts2010.tistory.com/166