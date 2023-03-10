ID3D11DeviceContext::Map( ) method

subresource 에 포함된 데이터를 가리키는 주소를 얻고, subresource 에 대한 GPU 접근을 부정한다.

# Syntax

```c++
HRESULT Map(
	ID3D11Resource *pResource,
	UINT  Subresource,
	D3D11_MAP  MapType,
	UINT  MapFlags,
	D3D11_MAPPED_SUBRESOURCE  *pMappedResource
)
```

# Parameters

1. $*$pResource
ID3D11Resource 인터페이스를 가리키는 주소

2. Subresource
subresource의 인덱스 주소

3. MapType
resource 에 대한 CPU의 읽고 쓰기의 권한을 특정하는 D3D11_MAP 유형의 값

4. MapFlags
GPU가 바쁠때 CPU 는 무엇을 할지 특정하는 flag. optional 이다.

5. pMappedResource
map 된 subresource에 대한 D3D11_MAPPED_SUBRESOURCE 구조페를 가리키는 포인터. 이것이 NULL 일 때에 관해서는 Remark 참조.


## Remarks

