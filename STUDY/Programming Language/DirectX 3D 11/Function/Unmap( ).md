ID3D11DeviceContext::Unmap

리소스에 대한 포인터를 무효화 하고 해당 리소스에 대한 GPU 의 접근을 다시 활성화 한다.

# Syntax

```c++
void Unmap(
  [in] ID3D11Resource *pResource,
  [in] UINT           Subresource
);
```

# Parameters

1. pResource
ID3D11Resource 인터페이스를 가리키는 포인터

2. Subresource
mapping 이 해제 될 subresource

# Return value

 none

# Remarks

어떻게 mapping 을 해제하는 지는 다음을 보라.
[[How to. Use dynamic resource]]