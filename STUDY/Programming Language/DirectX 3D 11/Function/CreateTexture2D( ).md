
ID3D11Device::CreateTeture2D  method

## Syntax

```c++
HRESULT CreateTexture2D( 
	[in] const D3D11_TEXTURE2D_DESC *pDesc, 
	[in, optional] const D3D11_SUBRESOURCE_DATA *pInitialData, 
	[out, optional] ID3D11Texture2D **ppTexture2D 
);
```

## Parameters

1. const D3D11_TEXTURE2D_DESC *pDesc : 2D 텍스쳐 리소스를 설명하는 D3D11_TEXTURE2D_DESC 구조체 를 가리키는 포인터이다. 런타임 시에 다른 호환 형식으로 해석될 수 있는 유형없는 리소스를 생성하기 위해서 텍스쳐 설정 구조체에 유형 없음을 명시해야 한다. mipmap level 을 자동적으로 생성하기 위해서는 mipmap level 을 0으로 설정한다.
2. const D3D11_SUBRESOURCE_DATA *pInitialData : <span style ="color:yellow">2D 텍스쳐 리소스에 대한 subresource 를 묘사하는 D3D11_SUBRESOURCE_DATA 구조체 의 배열을 가리키는 포인터이다.</span> 응용 프로그램은 IMMUTABLE resources(생성할때 초기화 되어 수정할 수 없고 GPU로 읽을 수 만 있는 리소스) 를 생성할 때  pInitialData에 대해 NULL을 명시할 수 없다. 만약 리소스가 multysampled 된다면, pInitialData 는 반드시 Null 이어야하다. Multisampled resource 는 생성될 때 데이터로 초기화 할 수 없기 때문이다. 만약 pInitialData 에 아무것도 전달하지 않는다면, 그 리소그에 대한 메모리의 초기화 내용은 undefined 이다. 이러한 경우, 그 리소스가 읽히기 전에 다른 어떤 방법으로 그 리소스의 내용을 작성해야 한다 
3. ID3D11Texture2D **ppTexture2D : <span style="color:Yellow">텍스쳐를 생성하는 것에 대한 ID3D11Texture2D 인터페이스를 가리키는 포인터를 받는 버퍼를 가리키는 포인터.</span> 다른 입력 매개변수를 유효성 감하기 위해서는 이 매개변수를 NULL 로 설정한다.(만약 다른 입력 매개면수의 유효성 검사가 통과되면 이 메서드는 S_FAILE을 반환한다.)

## Return value

HRESULT

## Remarks

CreateTexture2D 는 몇 개의 2D subresource 를 2D 텍스쳐 리소스가 함유될 수 있는 2D 텍스쳐 리소스를 생성한다. 그 텍스쳐의 수는 texture description 으로 특정된다. 하나의 resource 의 모든 texture 는 모두 format, size, mipmap level 이 같아야 한다.

