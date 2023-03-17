buffer resource 는 요소(element)로 그룹화 된 fully typed data의 collection 이다. 버퍼를 사용하여 위치 벡터(position vector), 법선 벡터(normal vector), 정점 버퍼의 텍스쳐 좌표, 인덱스 버퍼의 인덱스, 디바이스 상태 등의 다양한 데이터 값을 저장할 수 있다. 하나의 버퍼 요소(buffer element)는 1개 부터 4개 까지의 구성요소로 구성된다. 버퍼 요소는 packed data 값 (예를 들어 R8G8B8A8 surface value), 단일 8비트 정수, 또는 4개의 float point 값 들을 포함할 수 있다. 

하나의 버퍼는 구조화되지 않는 리소스로 생성된다. 구조화 되지 않은 상태로 생성되기 때문에 버퍼는 어떠한 mipmap level 도 포함할 수 없고 읽을 때 필터링 될 수 있으며 또한 멀티 샘플링 될 수 없다.

## Vertex Buffer

^2a545d

<span style="color:yellow">정점 버퍼는 형상을 정의하는데 사용되는 정점 데이터를 포함한 배열</span>이다. 정점 데이터는 위치 좌표, 색 데이터, 텍스져 좌표, 법선 데이터 등을 포함한다.

정점 버퍼의 가장 간단한 예는 오직 위치 데이터를 가지고 있는 것이다. 그것은 아래와 같이 시각화 할 수 있다.

![[Pasted image 20221224162442.png]]

종종 정점 버퍼는 3D 정점을 완전히 규정하는데 필요한 모든 데이터를 포함한다. 이것의 예는 각 정점 위치, 법선, 텍스쳐 좌표를 포함하는 정점 버퍼가 될 수 있다. 이 데이터는 보통 아래의 그림과 같이 각 정점 요소 세트로서 구성된다.

![[Pasted image 20221224163931.png]]

이 정점 버퍼는 각 정점 데이터를 포함한다. 각 정점은 세가지 요소를 저장한다. (위치, 법선, 텍스쳐 좌표). 위치와 법선은 각각이 보통 3개의 32비트 float과 2개의 float 을 사용하는 텍스쳐 좌표를 사용하여 특정된다.

정점버퍼의 데이터에 접근(access)하기 위해서는 access할 정점과 야래의 추가 버퍼 매개변수를 알아야 한다. 
- offset - 버퍼의 시작부터 첫번째 버퍼에 대한 데이터 까지의 byte 수
- BaseVertexLocation - 점점 버퍼의 정점을 읽기 전의 각 인덱스에 추가되는 값.

정점 버퍼를 생서하기 전에, ID3D11InputLayout 인터페이스 생성으로 점점 버퍼의 layout을 정의해야 한다. 이것은 ID3D11Device::CreateInputLayout 메서드를 호출하여 완료한다. input-layout object가 생성된 다음, ID3D11DeviceContext::IASetInputLayout을 호출하여 그것을 input-assambler stage 에 바인딩 해야한다. 

정점 버퍼를 생성하기 위해서는 ID3D11Device::CreateBuffer 를 호출한다.

## Index Buffer

인덱스 버퍼는 버텍스 버퍼에 있는 각 정점의 위치를 기록한다. 그러면 GPU 는 버텍스 버퍼의 특정 정점들을 빠르게 찾는데에 인덱스 버퍼를 사용한다. 
인텍스 버퍼는 정점 버퍼에 대한 offset integer 를 포함하고, 더욱 효율적으로 primitives 를 렌더하는데 사용된다. 하나의 인덱스 버퍼는 순자적인 16비트 혹은 32비트의 인덱스들의 세트를 포함한다. 각 인덱스는 정점 버퍼의 정점을 확인하는데 사용된다. 하나의 인덱스 버퍼는 다음의 그림과 같이 시각화 할 수 있다.

![[Pasted image 20221224191613.png]]

인덱스 버퍼에 저장되는 순차적인 인덱스들은 다음의 매개변수와 함께 위치(포함?)한다.
- Offset - 인덱스 버퍼의 기본 주소 의 bype 수. offset은 ID3D11DeviceContext::IASetIndexBuffer 메소드로 공급된다.
- StartIndexLocation - 첫번째 인덱스 버퍼 요소를  
- IASetInexBuffer 에 제공된 기본 주소와 offset의 첫번째 인덱스 버퍼 요소를 지정한다. 시작 location 은 ID3D11DeiceContext::DrawIndexed 또는 ID3D11DeviceContext::DrawIndexedInstaned 메소드에 제공되고 렌더링할 첫번째 인덱스를 나타낸다.
- IndexCount - 렌더링할 인덱스들의 수이다. 그 수는 DrawIndexed 메서드에 제공된다.

인덱스 버퍼의 시작 = 인덱스 버퍼 기본 주소 + Offset(Bytes) + StartIndexLocation x ElementSize(Bytes)

위의 식에서 ElementSize 는 각 인덱스 버퍼 요소의 크기이며 그것은 2 혹은 4 byte 이다.

인덱스 버퍼를 생성하기 위해서는 ID3D11Device::CreateBuffer 를 호출한다.

## Constant Buffer

상수 버퍼는 파이프라인에 shader constant data 를 효율적으로 공급하는 것을 허락한다. 상수 버퍼는 stream-output stage 의 결과를 저장하느데 사용될 수 있다. 개념적으로 상수버퍼는 다음 그림과 같기 때문에 단지 단일 요소 정적 버퍼(single-element vertex buffer)로 보일 수 있다. 

![[Pasted image 20221224195826.png]]

각 요소는 데이터의 형식이 저장되는 것에 따라 결정되는 1에서 4 가지 구성요소 상수를 저장한다.

상수 버퍼는 정점 및 픽셀 쉐이더 에서 사용될 상수를 모아 놓은 버퍼이다. 

상수 버퍼 사용 과정 : cpp 코드 영역에 상부 버퍼 타입의 구조체 정의 -> 세이더에 동일한 포맷으로 상수 버퍼 구조체를 정의 -> 시스템 메모리에서 구조체 변수 생성 및 값 설정 -> 정점 혹은 픽셀 쉐이더에 set 한다.

값 설정 및 set 은 보통 매 프레임 실행 되는 Render() 함수에 적용한다. 그러면 이 Set 시킨 데이터를 설정한 쉐이더에서 사용할 수 있다.

상수 버퍼 사용 이유 : 쉐이더에서 매번 사용되는 상수(여기에서 상수는 하나의 프레임 별로 scene 의 물체가 달라지는 정도말한다) 를 cpp 파일에서 전달해 주어야 한다고 가정할 때, 개별적으로 값을 전달해주는 것은 bandwidth(대역폭)크고 부담이 많다. 따라서 하나의 구조체로 묶어 보내기 위해 constant buffer 개념을 사용한다.
하지만 하나의 상수 버퍼 구조체에 모든 데이터를 넣는 것이 아니라 업데이트 주기에 따라 상수 버퍼를 구성하는 것이 좋다. 예를 들어, 절대 변경되지 않는 고정 데이터, 특정 조건에 따라 변하는 데이터, 매 프레임마다 변하는 데이터와 같은 방식으로 나누는 것이 효율적이다.  ^1bccd9


constant buffer 의 생성시 다음의 주의사항 2가지가 존재한다.
1. default heap 이 아닌 upload heap 에 생성해야 한다.(그래야 cpu 가 버퍼의 내용을 갱신할 수 있다. default heap은 cpu 가 생성 불가.) 
->https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_heap_type
(D3D12 에서는 heap type 을 정할 수 가 있는데 D3D11 에서는 constant buffer 를 생성할 때 기본으로 upload heap 에 생성이 되는 건가.)
3. 크기는 256 byte의 배수여야 한다.

장면의 물체의 특성을 저장하는 특성상, 같은 종류의 상수 버퍼를 여러개 사용해야 하는 경우가 생기는데 장면의 물체가 n 개이면 이 종류의 상수 버퍼가 n 개 필요하다. 
shader-contant 버퍼를 생성하려면 ID3D11Device::CreateBuffer 를 호출하고, D3D11_BIND_FLAG 열거형의 맴버인 D3D11_BIND_CONSTANT_BUFFER 를 지정한다.

상수 버퍼는 다른 어떠한 bind flag도 포함 될 수 없는 오직 하나의 bind flag 를 사용한다. shader-contant buffer 를 파이프 라인에 바인딩하기 위해서 다음의 메소드중 하나를 호출하다.
ID3D11DeviceContext::GSSetConstantBuffers
ID3D11DeviceContext::PSSetConstantBuffers
ID3D11DeviceContext::VSSetConstantBuffers

shader 로부터 shader-constant buffer 를 읽기 위해서 HLSL load funtion을 사용한다. 각 shader stages는 15개의 shader-constant buffer 까지 허용한다. 상수 버퍼는 업데이트 및 전송이 빈번하기 때문에 처리하기에 효율적인 메모리 정렬이 되어 있어야 한다. 따라서 상수 버퍼를 구성하는 구조체는 16 바이트 정렬이 되어 있어야 한다 따라서 전체 크기는 16배수가 되며, 최대 4096x16 바이트 가질 수 있다.
(하나의 구조체를 정의할 때는 12 바이트를 정의한다고 나머지 4바이트가 자동 패딩 된다고 생각하지말고 애초에 16바이트로 정의할 것 https://woo-dev.tistory.com/271)

### 상수 버퍼의 갱신 (update)

상수 버퍼를 upload heap 에서 생성하였다면, cpu에서 상수 버퍼 에 자료를 올릴 수 있다. (cpu 에서 update 가능) 그 절차는 다음과 같다.

- Map() 메서드를 사용하여 상수 버퍼의 subresource 주소를 얻는다. (자료를 올리기 위해서 자원 자료를 가리키는 포인터를 얻어야 한다. 이때 ComPtr 의 Get() 메서드는 buffer 의 주소를 얻는데 쓰이고 Map 메서드로 얻은 주소는 buffer의 저장공간 주소이다.)
- memcpy를 이용하여 subresource 의 자원을 BYTE* type 의 변수에 복사한다.
