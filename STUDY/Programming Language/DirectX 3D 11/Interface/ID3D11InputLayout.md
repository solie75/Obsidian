input layout은 메모리에 배치된 vertex data 를 어떻게 graphics pipeline 의 input assembler stage 에 공급할 것인지에 대한 정의를 보유하고 있다.

## Ingeritance

ID3D11DeviceChild 인터페이스를 상속 받았다.

## Remarks

To create an input-layout object, call [ID3D11Device::CreateInputLayout](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createinputlayout). To bind the input-layout object to the [input-assembler stage](https://learn.microsoft.com/en-us/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage), call [ID3D11DeviceContext::IASetInputLayout](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout).

## Reference

https://learn.microsoft.com/en-us/windows/win32/api/d3d11/nn-d3d11-id3d11inputlayout