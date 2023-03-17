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
[[D3D11_Map enumeration]]

4. MapFlags
GPU가 바쁠때 CPU 는 무엇을 할지 특정하는 flag. optional 이다.

5. pMappedResource
map 된 subresource에 대한 D3D11_MAPPED_SUBRESOURCE 구조체가리키는 포인터. 이것이 NULL 일 때에 관해서는 Remark 참조.

# Return value

type : HRESULT

해당 메서드는 MapFlags 매개변수가 D3D11_FLAG_DO_NOT_WAIT 으로 특정되고  GPU 가 resource 를 끝마치지 않았다면 DXGI_ERROR_WAS_STILL_DRAWING 을 반환한다.

해당 메서드는 또한 MapType 매개변수가 CPU 가 읽기 접근하는 것을 허용하고 video card 는 제거 되었을 때 DXGI_ERROR_DEVICE_REMOVED 를 반환한다.

# Remarks

deferred context 에서 Map 을 호출한 경우, MapType 매개변수에 [D3D11_MAP_WRITE_DISCARD](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/ne-d3d11-d3d11_map), [D3D11_MAP_WRITE_NO_OVERWRITE](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/ne-d3d11-d3d11_map)나 혹은 둘다를 전달할 수 있다. 다른 D3D11_MAP_TYPED value 는 deferred context 를 지원하지 않는다.

- note
	Direc3D 11.1  runtime 은 dynamic constant buffer 와 shader resource view 를 [D3D11_MAP_WRITE_NO_OVERWRITE](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/ne-d3d11-d3d11_map) 사용으로 mapping 할 수 있다. Direct3D 11 이전의 runtime 은 vertex buffer 또는 index buffer 에 대한 mapping 이 제한되었다. 

map에 대한 사용 방법은 Use Dynammic resource 참조

## NULL pointers for pMappedResource

pMappedResource 매개변수는 texture 가 D3D11_USAGE_DEFAULT 로 생성되어 제공 될 때에 NULL 이 될 것이다. 그리고 그 API 는 immediate context 로 불린다. 이것은 심지어 D3D11_TEXTURE_LAYOUT_UNDEFINED로 생성되었다고 하더라도default texture 가 mapping 되도록 한다. 이 API 의 호출에 이어, [ID3D11DeviceContext3::WriteToSubresource](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11_3/nf-d3d11_3-id3d11device3-writetosubresource) and/or [ID3D11DeviceContext3::ReadFromSubresource](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11_3/nf-d3d11_3-id3d11device3-readfromsubresource) 를 사용하여 접근 될 수 있다.

## Don't read from a subresource mapped for writing

MapType 매개변수에  [D3D11_MAP_WRITE](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/ne-d3d11-d3d11_map), [D3D11_MAP_WRITE_DISCARD](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/ne-d3d11-d3d11_map), 또는 [D3D11_MAP_WRITE_NO_OVERWRITE](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/ne-d3d11-d3d11_map) 를 전달할 때, 응용 프로그램이 D3D11_MAPPED_SUBRESOURCE 의 맴버인 pData가 가리키는(point) subresource data 를 읽지 않도록 해야한다. 그렇게 하는 것이 상당한 성능 저하를 일으키기 때문이다. pData 가 가리키는 메모리 지역은 pData 가 가리키는 메모리 지역은 PAGE_WRITECOMBINE 으로 할당 될 수 있다. 그리고 응용프로그램은 이러한 메모리에 관련된 모든 제한 사항을 준수해야 한다.

- note
다음의 c++ 코드 또한 메모리를 읽고 성능 패널티를 trigger 할 수 있다.
```c++
// c++
*((int*)MappedResource.pData) = 0;
```
```x86 assembly
// x86 assembly
AND DWORD PTR [EAX], 0
```

이러한 성능 저하를 피하고 싶다면 적절한 최적화 설정과 언어 구조를 사용해야 한다.

