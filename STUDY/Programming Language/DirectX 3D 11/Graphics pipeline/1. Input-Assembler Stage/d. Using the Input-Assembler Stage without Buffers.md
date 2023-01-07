만약 shader가 buffer를 요구하지 않는다면 buffer 를 생성하고 바인딩하는 것은 필수적이지 않다. 해당 section 은 단일 삼각형을 그리는 간단한 점점 및 픽셀 shader 의 예를 포함한다. 


## Vertex Shader

자신이 소유한 정점들을 생성하는 vertex shader 를 선언할 수 있다고 예를 들 때

```c++
struct VSIn
{
	uint vertexId : SV_VertexID;
};

struct VSOut
{
	float4 pos : SV_Position;
	float4 color : Color;
};

VSOut VSmain(VSIn input)
{
	VSOut output;

	if(inout.vertexId == 0){
		output.pos = float4(0.0, 0.5, 0.5, 1.0);
	}
	else if (input.vertexId == 2){
		output.pos = float4(-0.5, -0.5, 0.5, 1.0);
	}
	else if (input.vertexId == 1){
		output.pos = float4(-0.5, -0.5, 0.5, 1.0);
	}

	output.color = Clamp(output.pos, 0, 1);
}
```

## Pixel Shader

```c++
struct PSIn
{
	float4 pos : SV_Position;
	linear float4 color : color;
};

struct PSOut
{
	float4 color : SV_Target;
};

PSOut PSmain(PSIn input)
{
	PSOut output;
	output.color = input.color;

	return output;
}
```

## Technique

technique 는 rendering pass 의 collection 이다. 이 때 컬렉션은 적어도 하나의 pass 를 간져야 한다.  

```c++
VertexShader vsCompiled = CompileShader( vs_4_0, VSmain() );

technique10 t0
{
    pass p0
    {
        SetVertexShader( vsCompiled );
        SetGeometryShader( NULL );
        SetPixelShader( CompileShader( ps_4_0, PSmain() ));
    }  
}
```

## Application Code

```c++
m_pD3D11Device->IASetInputLayout( NULL );

m_pD3D11Device->IASetPrimitiveTopology( D3D_PRIMITIVE_TOPOLOGY_TRIANGLELIST );
    
ID3DX11EffectTechnique * pTech = NULL;
pTech = m_pEffect->GetTechniqueByIndex(0);
pTech->GetPassByIndex(iPass)->Apply(0);

m_pD3D11Device->Draw( 3, 0 );
```