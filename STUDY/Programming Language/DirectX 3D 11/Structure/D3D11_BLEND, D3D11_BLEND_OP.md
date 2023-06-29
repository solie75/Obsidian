# D3D11_BLEND

Blend factors, which modulate values for the pixel shader and render target.

```c++
typedef enum D3D11_BLEND {
  D3D11_BLEND_ZERO = 1,
  D3D11_BLEND_ONE = 2,
  D3D11_BLEND_SRC_COLOR = 3,
  D3D11_BLEND_INV_SRC_COLOR = 4,
  D3D11_BLEND_SRC_ALPHA = 5,
  D3D11_BLEND_INV_SRC_ALPHA = 6,
  D3D11_BLEND_DEST_ALPHA = 7,
  D3D11_BLEND_INV_DEST_ALPHA = 8,
  D3D11_BLEND_DEST_COLOR = 9,
  D3D11_BLEND_INV_DEST_COLOR = 10,
  D3D11_BLEND_SRC_ALPHA_SAT = 11,
  D3D11_BLEND_BLEND_FACTOR = 14,
  D3D11_BLEND_INV_BLEND_FACTOR = 15,
  D3D11_BLEND_SRC1_COLOR = 16,
  D3D11_BLEND_INV_SRC1_COLOR = 17,
  D3D11_BLEND_SRC1_ALPHA = 18,
  D3D11_BLEND_INV_SRC1_ALPHA = 19
} ;
```

D3D11_BLEND_ZERO : The blend factor is (0, 0, 0, 0). No pre-blend operation.
D3D11_BLEND_ONE : The blend factor is (1, 1, 1, 1). No pre-blend operation.
D3D11_BLEND_SRC_COLOR : The blend factor is (Rₛ, Gₛ, Bₛ, Aₛ), that is color data (RGB) from a pixel shader. No pre-blend operation.
D3D11_BLEND_INV_SRC_COLOR : The blend factor is (1 - Rₛ, 1 - Gₛ, 1 - Bₛ, 1 - Aₛ), that is color data (RGB) from a pixel shader. The pre-blend operation inverts the data, generating 1 - RGB.

# Remark

  
- what is 'blend factor' in DirectX 11
In DirectX 11, the term "blend factor" refers to a parameter used in the blending stage of the graphics pipeline. The blending stage determines how the output color from a pixel shader is combined with the existing color already stored in the render target. The blend factor is a vector that represents a color, and it is typically used to modulate the output color before it is blended with the destination color.

The blend factor can be set using the ID3D11DeviceContext::OMSetBlendFactor method in the DirectX 11 API. It takes a four-component vector as input, with each component ranging from 0.0 to 1.0. The components of the blend factor vector control the blending operation for the corresponding color channels (red, green, blue, and alpha).

The blend factor is commonly used in scenarios where you want to apply custom blending operations or special effects. By modifying the blend factor, you can control the transparency, intensity, or color modulation of the rendered objects. The actual blending operation is determined by the blend state object set on the device context using the ID3D11DeviceContext::OMSetBlendState method.

Overall, the blend factor provides flexibility in controlling how the output color is blended with the existing color in the render target, allowing for various visual effects in DirectX 11 graphics programming.


- what is 'No pre-blend operation' in D3D11_BLEND
In DirectX 11, the D3D11_BLEND enumeration is used to specify the blend operation for a particular render target in the blend state object. One of the possible values in this enumeration is D3D11_BLEND_ONE, which represents "No pre-blend operation" or "No blending."

When you set the blend state for a render target to D3D11_BLEND_ONE, it means that there is no blending operation performed on the output color from the pixel shader. The output color is directly written to the render target without any modification or combination with the existing color.

In other words, with D3D11_BLEND_ONE, the pixel shader's output color completely overwrites the existing color in the render target, disregarding any previous pixel values. This can be useful in situations where you want to completely replace the existing color with the output color, such as when rendering opaque objects or performing certain post-processing effects.

It's important to note that the blending operation can be further controlled by other blend state properties, such as the blend factors, blend equations, and per-channel blending options. However, when the blend operation is set to D3D11_BLEND_ONE, these properties have no effect, and the output color is directly written to the render target without any modification.

# D3D11_BLEND_OP

RGB or alpha blending operation.

```c++
typedef enum D3D11_BLEND_OP {
  D3D11_BLEND_OP_ADD = 1,
  D3D11_BLEND_OP_SUBTRACT = 2,
  D3D11_BLEND_OP_REV_SUBTRACT = 3,
  D3D11_BLEND_OP_MIN = 4,
  D3D11_BLEND_OP_MAX = 5
} ;
```

D3D11_BLEND_OP_ADD : Add source 1 and source 2.