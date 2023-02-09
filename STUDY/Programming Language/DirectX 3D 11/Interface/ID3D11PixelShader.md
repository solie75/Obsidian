A pixel-shader interface manages an executable program (a pixel shader) that controls the pixel-shader stage.

## Inheritance

The ID3D11PixelShader interface inherits from the ID3D11DeviceChild interface.

## Remarks

The pixel-shader interface has no methods; use HLSL to implement your shader functionality. All shaders in are implemented from a common set of features referred to as the common-shader core..

To create a pixel shader interface, call [ID3D11Device::CreatePixelShader](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader). Before using a pixel shader you must bind it to the device by calling [ID3D11DeviceContext::PSSetShader](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader).

This interface is defined in D3D11.h.

## Reference

https://learn.microsoft.com/en-us/windows/win32/api/d3d11/nn-d3d11-id3d11pixelshader