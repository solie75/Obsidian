buffer resource 는 요소(element)로 그룹화 된 fully typed data의 collection 이다. 버퍼를 사용하여 위치 벡터(position vector), 법선 벡터(normal vector), 정점 버퍼의 텍스쳐 좌표, 인덱스 버퍼의 인덱스, 디바이스 상태 등의 다양한 데이터 값을 저장할 수 있다. 하나의 버퍼 요소(buffer element)는 1개 부터 4개 까지의 구성요소로 구성된다. 버퍼 요소는 packed data 값 (예를 들어 R8G8B8A8 surface value), 단일 8비트 정수, 또는 4개의 float point 값 들을 포함할 수 있다. 

하나의 버퍼는 구조화되지 않는 리소스로 생성된다. 구조화 되지 않은 상태로 생성되기 때문에 버퍼는 어떠한 mipmap level 도 포함할 수 없고 읽을 때 필터링 될 수 있으며 또한 멀티 샘플링 될 수 없다.

## Vertex Buffer

정점 버퍼는 형상을 정의하는데 사용되는 정점 데이터를 포함한다. 정점 데이터는 위치 좌표, 색 데이터, 텍스져 좌표, 법선 데이터 등을 포함한다.

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

각 요소는 데이터의 형식이 저장되는 것에 따라 결정되는 1에서 4 가지 구성요소 상수를 저장한다. shader-contant 버퍼를 생성하려면 ID3D11Device::CreateBuffer 를 호출하고, D3D11_BIND_FLAG 열거형의 맴버인 D3D11_BIND_CONSTANT_BUFFER 를 지정한다.

상수 버퍼는 다른 어떠한 bind flag도 포함 될 수 없는 오직 하나의 bind flag 를 사용한다. shader-contant buffer 를 파이프 라인에 바인딩하기 위해서 다음의 메소드중 하나를 호출하다.
ID3D11DeviceContext::GSSetConstantBuffers
ID3D11DeviceContext::PSSetConstantBuffers
ID3D11DeviceContext::VSSetConstantBuffers

shader 로부터 shader-constant buffer 를 읽기 위해서 HLSL load funtion을 사용한다. 각 shader stages는 15개의 shader-constant buffer 까지 허용한다. 각 buffer 는 4096까지의 상수(constant)를 hold 한다.

