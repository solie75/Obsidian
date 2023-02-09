임의의 길이를 반환(return) 하는 데 사용되는 인터페이스로 IUnknown 인터페이스를 상속받는다. 


ID3DBlob 은 또한 다음의 메소드를 갖는다.
## Methods

1. GetBufferPointer : blob의 데이터를 가리키는 포인터를 검색한다.
2. GetBufferSize : blob의 데이터의 크기를 byte 로 검색한다.


## Remark

ID3DBlob 은 [**D3DCreateBlob**](https://msdn.microsoft.com/en-us/library/ff728672(v=vs.85)) 함수를 호출하여 얻어(obtain)진다. D3DCreateBlob 은 D3dcompiler.lib 또는 D3dcompiler_nn.dll 에 포함된다.

ID3DBlob 은 D3DComon.h 헤더 파일에서  ID3D10Blob 인터페이스로 타입 정의 된다. 
```c++
typedef ID3D10Blob ID3DBlob;
```
ID3DBlob 은 version-neutral 이며 Direct3D 의 모든 버전에서 사용 될 수 있다.

Blob 은 data buffer로 사용될 수 있다. Blob 은 vertex, adhacency, 그리고 mesh 최적화와 operation 로딩 도중에 material information 을 저장하는데 사용될 수 있다. 또한 이러한 object 들은 object code를 반환하거나 compile vertex, geometry, pixel shader와 같은 API 의 error message 를 반환한다. 

※ blob 은 Binary Large Object 의 약자로 바이너리 형태로 용량이 큰 객체를 저장하는 데 사용된다.