vertex shader stage 를 control 하는 Exucutable program(실행 프로그램) 을 관리한다.

## Inheritance

ID3D11VertexShader 인터페이스는 ID3D11DeviceChild 인터페이스를 상송받는다.

## Remarks

vertex shader 인터페이스는 shader의 기능을 시행하기 위한 HLSL 를 사용하는 method 를 갖지 않는다. 모든 shader 는 공통 shader core 라고 하는 공통 기능 집합에서 구현된다.

To create a vertex shader interface, call [ID3D11Device::CreateVertexShader](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader). Before using a vertex shader you must bind it to the device by calling [ID3D11DeviceContext::VSSetShader](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader).

This interface is defined in D3D11.h.

## Reference

https://learn.microsoft.com/en-us/windows/win32/api/d3d11/nn-d3d11-id3d11vertexshader