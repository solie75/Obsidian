ID3D11DeviceContext::PSSetShaderResources

shader resource 의 배열을 pixel shader stage 에 바인딩한다.

# Syntax

```c++
void PSSetShaderResources(
  [in]           UINT                     StartSlot,
  [in]           UINT                     NumViews,
  [in, optional] ID3D11ShaderResourceView * const *ppShaderResourceViews
);
```

# Parameter

- StartSlot
설정할 처음 슬롯 넘버

- NumViews
설정할 shader resource 의 수

- ppShaderResourceViews
device 에 설정할 shader resource view 의 배열

# Remarks

이 함수의 대상이 되는 resource view 가 이미 rendertarget 과 같은 output slot 에 바인딩 되어 중복 되는 경우. 이 함수는 목적이 되는 shader resource 의 slot 에 NULL 로 채운다.

