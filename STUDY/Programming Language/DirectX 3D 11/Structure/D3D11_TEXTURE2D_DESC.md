# Syntax

```c++
typedef struct D3D11_TEXTURE2D_DESC { 
	UINT Width; 
	UINT Height; 
	UINT MipLevels; UINT ArraySize; 
	DXGI_FORMAT Format; 
	DXGI_SAMPLE_DESC SampleDesc; 
	D3D11_USAGE Usage; 
	UINT BindFlags; 
	UINT CPUAccessFlags; 
	UINT MiscFlags; 
} D3D11_TEXTURE2D_DESC;
```

#### 1. Width
texel 단위로 텍스쳐의 너비. The range is from 1 to D3D11_REQ_TEXTURE2D_U_OR_V_DIMENSION (16384). For a texture cube-map, the range is from 1 to D3D11_REQ_TEXTURECUBE_DIMENSION (16384). However, the range is actually constrained by the [feature level](https://learn.microsoft.com/en-us/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro) at which you create the rendering device.
#### 2. Height
1과 동일한 특징을 같는 텍스쳐의 높이

#### 3. MipLevel
텍스쳐에서 가장 높은 mipmap level 의 수. See the remarks in [D3D11_TEX1D_SRV](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/ns-d3d11-d3d11_tex1d_srv). Multisampling 된 텍스쳐를 사용하려면 1을 [[Subresources]] subtexture 의 모든 설정을 generate 하기 위해서는 0을 설정한다.

#### 4. ArraySize
texture array 내의 texture 수. The range is from 1 to D3D11_REQ_TEXTURE2D_ARRAY_AXIS_DIMENSION (2048). For a texture cube-map, this value is a multiple of 6 (that is, 6 times the value in the **NumCubes** member of [D3D11_TEXCUBE_ARRAY_SRV](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/ns-d3d11-d3d11_texcube_array_srv)), and the range is from 6 to 2046. The range is actually constrained by the [feature level](https://learn.microsoft.com/en-us/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro) at which you create the rendering device.

#### 5. Format
Texture 형식 (see [DXGI_FORMAT](https://learn.microsoft.com/en-us/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)).

#### 6. SampleDesc
텍스쳐에 대한 multisampling 매개변수를 특정하는 구조체.  See [DXGI_SAMPLE_DESC](https://learn.microsoft.com/en-us/windows/desktop/api/dxgicommon/ns-dxgicommon-dxgi_sample_desc).

#### 7. Usage
텍스쳐가 어떻게 일히고 작성될지를 식별하는 값. D3D11_USAGE_DEFAULT 가 가장 흔히 쓰인다. 

#### 8. BindFlags
pipeline stage 에 바인딩 되기 위한 flag. 해당 flag 는 bitwise OR 로 조합 될 수 있다.

#### 9. MiscFlags
일반적이지 않은 다른 옵션들을 식별하는 flags. Use 0 if none of these flags apply. These flags can be combined by using a bitwise OR. For a texture cube-map, set the [D3D11_RESOURCE_MISC_TEXTURECUBE](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/ne-d3d11-d3d11_resource_misc_flag) flag. Cube-map arrays (that is, **ArraySize** > 6) require feature level [D3D_FEATURE_LEVEL_10_1](https://learn.microsoft.com/en-us/windows/desktop/api/d3dcommon/ne-d3dcommon-d3d_feature_level) or higher.

# Remark

This structure is used in a call to [ID3D11Device::CreateTexture2D](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createtexture2d).

In addition to this structure, you can also use the [CD3D11_TEXTURE2D_DESC](https://learn.microsoft.com/en-us/previous-versions/windows/desktop/legacy/jj151700(v=vs.85)) derived structure, which is defined in D3D11.h and behaves like an inherited class, to help create a texture description.

The device places some size restrictions (must be multiples of a minimum size) for a subsampled, block compressed, or bit-format resource.

The texture size range is determined by the [feature level](https://learn.microsoft.com/en-us/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro) at which you create the device and not the Microsoft Direct3D interface version. For example, if you use Microsoft Direct3D 10 hardware at feature level 10 ([D3D_FEATURE_LEVEL_10_0](https://learn.microsoft.com/en-us/windows/desktop/api/d3dcommon/ne-d3dcommon-d3d_feature_level)) and call [D3D11CreateDevice](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) to create an [ID3D11Device](https://learn.microsoft.com/en-us/windows/desktop/api/d3d11/nn-d3d11-id3d11device), you must constrain the maximum texture size to D3D10_REQ_TEXTURE2D_U_OR_V_DIMENSION (8192) when you create your 2D texture.