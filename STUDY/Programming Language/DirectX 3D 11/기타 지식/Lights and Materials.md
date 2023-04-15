light은 scene 의 오브젝트들을 그리는데 사용된다. light 가 사용가능 하면, Direct3D 는 다음 항목을 사용한 조합에 근거하여 각 오브젝트를 이루는 정점의 색을 계산한다.
- 관련된 texture map의 현재의 material 색 과 텍셀
- (특정되었다면) 정점의 diffusse 색과 specular 색
- scene 에서 광원 에 의해 생성되는 조명의 색과 강도, 또는 그 scene 의 주변광의 수준

Direct3D 의 light 와 material 을 사용하는 경우, Direct3D가 조명(illumination)의 세부사항을 처리하도록 허용된다. 원한다면 스스로 조명을 수행할 수도 있다.

lighting 과 material 을 어떻게 활용하는지에 따라 랜더링 되는 scene 의 모습이 매우 다르다. material 은 표면의 빛 반사의 방식을 정의한다. Direct light 와 ambient light 는 반사되는 빛을 정의한다. light 가 사용이 가능하다면 사용자는 scene 을 렌더링할 때 material을 사용하여야 한다. scene을 렌더링하는데 light 가 필수적인것은 아니지만 빛이 없이 렌더링된 scene 의 세부사항들은 보이지 않을 수 있다. 기껏해야 빛이 없는 상태의 렌더링은 결과적으로 오브젝트의 실루엣만 보이게 될 수 있다. 이것은 대부분의 사용자의 목적에 부합하지 않는다.

## Direct Light vs Ambient Light

Direct Light 는 말그대로 방향성을 가진다. 즉, Direct Light 는 항상 방향과 색을 갖고 있으며 Gouraud shading 과 같은 shading 알고리즘의 요소가 된다. IDirect3DDevice9::SetLight 메서드를 호출하여 direct light 에 대한 조명 매개변수 세트를 만들 수 있다. Ambient light 는 장면의 어디에나 존재 하여 장면을 전체적으로 채우고 있는 일반적인 조명으로 생각할 수 있다. Ambient light 는 위치와 방향이 없이 오직 색과 강도만을 가진다. D3DRS_AMBIENT 를 State 의 매개변수로 지정하고 원하는 RGBA 색상을 Value 매개변수로 지정하여 IDirect3DDevice9::SetRenderState 메서드를 호출하는 것으로 ambient light 의 수준을 설정할 수 있다. 

## Direct3D Light Model

Direct3D에서 빛이 표면에서 반사되는 경우 light 의 색은 해당 표면과 수학적 상호작용을 거쳐 화면에 표시되는 최종적인 색상을 생성한다. [이때 쓰이는 알고리즘](https://learn.microsoft.com/en-us/windows/win32/direct3d9/mathematics-of-lighting) 

Direct3D 의 light model 은 direct light 와 ambient light 로 나뉜다. 각 light 는 다른 특성을 가지고 다른 방식으로 표면의 material 과 상호작용한다. ambient light 는 너무 많이 산란되어 방향과 출저가 불분명하고 그에 따라 어디에나 존재하지만 낮은 빛 강도를 갖는다. 결국 ambient light 의 수준은 scene 내의 빛을 생성하는 오브젝트와 완전히 독립적으로 Ambient light 는 specular relfection 에 영향을 끼치지 않는다.
Direct light 는 Scene 내의 빛을 생성하는 오브젝트로부터의 것으로 언제나 색상과 강도를 갖고 지정된 방향으로 이동한다. Direct light 는 specular highlight 를 생성하기 위해 표면의 material 과 상호작용하며 그 방향은 Gouraud shading 과 같은 shading 알고리즘에 사용된다. direct light 가 반사될 때 그것은 ambient light 의 수준(강도) 에 영향을 끼치지 않는다. direct light 를 생성하는 광원은 scene 을 비추는 방식에 영향을 끼치는 다양한 특성들을 가지고 있다.
추가적으로, 폴리곤의 material 은 폴리곤이 자신이 받는 빛을 반사하는 방식에 영향을 미치는 특성을 가지고 있다. material 이 ambient light 을 반사하는 방식을 설명하는 단일 반사율 특성을 설정해야 하고 , material의 specular    와 diffuse 의 반사율을 정하는 독립적인 특성을 설정해야 한다. [[Lights and Materials#^8334af]]

## Color values for Lights and Materials

Direct3D 에서는 최종적인 생상을 결정하기 위하여 조합되는 red, green, blue 그리고 alpha(투명도) 의 구성요소 측면에서 색상을 설명한다. D3DCOLORVALUE 구조체는 각 구성요소에 대한 값을 포함하도록 정의된다. 각 맴버는 floating-point 값으로 보통 0.0 에서 1.0 의 범위를 갖는다. 비록 light 와 material 모두 같은 색상을 설명하는 구조체를 사용한다고 하더라고 각각이 조금은 다르게 값이 사용된다.

광원의 색상 값은 광원이 방출하는 특정한 색의 양을 표시한다. light 는 alpha 값을 사용하지 않고 오직 red, green, blue 값과 관련된다. 세가지 색상을 혼합하여 화면으로 출력하며 각각이 완전히 켜지는 1.0의 값 부터 완전히 꺼지는 0.0 사이의 값을 가진다. red 와 , green, blue 가 모두 1.0 인 경우 white light 이고 모두 0.0 인 경우 전혀 발광하지 않는다. 

반면 material 에서 색상 값은 해당 material 로 렌더링된 표면에서 반사된 빛을 구성하는 색 값의 양을 나타낸다. 예를 들어 R(1.0), G(1.0), B(1.0), A(1.0) 인 material 은 들어오는 모든 빛을 반사하고 R(0.0), G(1.0), B(0.0), A(1.0) 은 모든 녹색 빛만을 반사한다. material 은 다양한 효과를 생성하기 위해서 여러 반사율 값을 가지고 있다. 추가적인 정보는 다음과 같다. [Light Type](https://learn.microsoft.com/en-us/windows/win32/direct3d9/light-types ) [Light Properties](https://learn.microsoft.com/en-us/windows/win32/direct3d9/light-properties)

# Light Types

light 유형의 특성은 사용되는 광원의 유형을 정의한다. light 유형은 D3DLIGHT 구조체의  Type 맴버인 D3DLIGHTTYPE enum 의 값을 사용하여 설정한다. Direct3D 에는 3가지 의 Light 유형이 있다.

### 1. Point Light

point light 는 scene 에서 하나의 방향성을 갖지 않고 위치와 색상을 가진다. 
point light 는 다음의 그림과 같이 모든 방향으로 동일한 빛을 발산한다.
![[Pasted image 20230413193937.png]]
point light 는 감쇠와 범위에 영향을 받고 정점 별로 mesh 를 비춘다. lighting 중에 Direct3D 는 world space 상의 point light의 위치 와 light 가 비추는 정점을 사용하여 light 의 방향과 light가 이동할 거리에 대한 벡터를 도출한다. 둘다 vertex normal 과 함께 illumination of the surface에 대한 빛의 기여하는 정도를 계산하는데 사용된다.

### 2. Directional Light

directional light 는 색과 방향만을 갖고 위치는 갖지 않는다. Directional light 는 parallel light(평행광) 을 발산한다. 이것은 directional light 에 의해 생성된 모든 light 는 같은 방향으로 scene 을 통과(through)한다는 것이다. Directional light 는 감쇠와 범위의 영향을 받지 않기 때문에 Direct3D 가 정점 색상을 계산할 때 (개발자가) 지정한 방향 또는 색상만 고려된다. 조명 요소의 수가 적기 때문에 사용하는데에 있어서 계산에 드는 소모가 가장 적다.

### 3. SpotLight

Spotlight 는 빛을 방출하는 색상, 위치, 방향을 가진다. spotlight 로 부터 방출되는 light 는 다음의 그림과 같이 밝은 내부 원뿔과 어두운 외부 원뿔로 구성되며 내부에서 외부로 갈수록 점차 어두워진다.

![[Pasted image 20230414003503.png]]

Spotlight 는 감소, 감쇠, 범위에 영향을 받는다. 빛이 각 정점으로 이동하는 거리 뿐만 아니라 이러한 요소들도 scene 의 오브젝트에 대한 lighting effect를 계산할때 관련된다(be figured in). 각 정점에 대한 이러한 효과의 계산은 spotlight 가 Direct3D 의 모든 light 중 가장 계산적으로 시간을 많이 소모하게 만든다.

D3DLIGHT9 구조체는 spotlight 에서만 사용되는 세 개의 맴버를 포함한다. 이러한 맴버(Falloff, Theta, Phi) 는 Spotlight 의 내부 혹은 외부 원뿔의 큰 정도와 내부 원뿔에서 외부 원뿔 까지의 빛의 강도 변화 방식을 제어한다. 

Theta 값은 spotlight의 내부 원뿔의 radian angle 이고 phi 값은 spotlight 의 외부 원뿔의 radian angle 이다. Falloff 값은 내부 원뿔의 바깥 가장자리와 외부 원뿔의 안쪽 가장자리의 빛의 강도가 감소하는 정도를 제어한다.
대부분의 응용프로그램은 Falloff 를 1.0 으로 설정하여 두 원뿔이 고르게 발생하게 하지만 필요에 따라 다른 값을 설정할 수 있다.

다음의 그림은 맴버 값 간의 관계와 spotlight 의 내부 및 외부 원뿔에 영향을 미치는 방법을 보여준다.

![[Pasted image 20230414010301.png]]

spotlight 는 두 개의 부분으로 구성된 빛의 원뿔을 발산한다. : 빛은 내부 원뿔에서 가장 빛나고 외부 원뿔의 바깥은 존재하지 않으며 두 영역 사이에서 빛의 강도가 약해진다. 이러한 유형의 감쇠를 일반적으로 falloff 라고 한다.

정점이 받는 빛의 양은 내부 원뿔 혹은 외부 원뿔 중 어디에 위치하는 지에 근거한다. Direct3D 는 spotlight의 방향 벡터 (L) 와 빛에서 정점으로의 빛 벡터 (D) 의 내적을 계산한다. 이 값은 두 벡터 사이의 코사인과 같다. 그리고 정점이 내부 원뿔 혹은 외부 원뿔 중 어디에 위치 할지를 결정하기 위한 light 의 원뿔 각도와 비교할 수 있는 정점 위치의 지표 역할을 한다. 다음의 그림은 D 와 L 두 벡터의 관계를 보여준다.

![[Pasted image 20230414013710.png]]

시스템은 이 값을 spotlight의 내부와 외부 원뿔 사이의 각의 코사인 값과 비교한다. light 의 D3DLIGHT9 구조체 에서 Theta 와 Phi 맴버는 내부 및 외부 원뿔에 대한 총 원뿔 각도를 나타낸다. 감쇠는 정점이 조명의 중심에서 더 멀어짐에 따라 발생하므로 런타임은 코사인 계산을 하기 전에 원뿔 각도를 반으로 나눈다.

L 과 D 벡터의 내적이 외부 원뿔 각도의 코사인 이하인 경우 정점은 외부 원뿔 바깥에 있고 빛을 받지 않는다. L과 D 의 벡터의 내적이 내부 원뿔 각도보다 큰 경우 정점은 내부 원뿔에 있고 거리에 따른 감쇠를 고려하여 빛의 양은 최대로 받게된다. 점점이 두 영역 사이의 어딘가에 있는 경우 falloff 는 다음의 방정식으로 계산된다.

$$I_f = (\frac{Cos(Alpha)-Cos(Phi/2)}{Cos(Theta/2)-Cos(Phi/2)})^{p}$$

- $I_f$ 는 falloff 이후의 빛의 강도이다.
- Alpha 는 vector L 과 D 의 사이의 각도이다.
- Theta 는 내부 원뿔의 각도이다.
- Phi 는 외부 원뿔의 각도이다.
- p 는 falloff 이다.

이 공식은 falloff를 설명하기 위해 정점에서의 빛의 강도를 조정하는 0.0 에서 1.0 사이의 값을 생성한다. 빛에서 정점 까지의 거리에 따른 감쇠도 적용된다. 다음의 그래프는 다른 falloff 값이 falloff 곡선에 어떤 영향을 미치는지를 보여준다.

![[Pasted image 20230414022525.png]]

실제 lighting 에 대한 다양한 falloff 값의 효과는 미묘하며 1.0 이외의 falloff 값으로 falloff 곡선을 형성하는 것으로 약간의 성능 저하를 발생시킨다. 이 값은 보통 1.0 으로 세팅한다.

# Light Properties

Light Properties 는 광원의 유형과 색상을 설명한다. 사용되는 light의 유형에 따라 light 는 감쇠와 범위에 대한 속성을 가질 수 있다. 그러나 모든 light 의 타입이 모든 속성을 사용하는 것은 아니다. Direct3D 는 D3DLIGHT9 구조체 를 사용하여 광원의 모든 종류에 대한 light properties 의 정보를 전달한다.

### 1. Light Attenuation

감쇠는 범위 속성에서 지정된 최대 거리에 가까워질 수록 빛의 강도가 감소하는 방식을 제어한다. 3개의 D3DLIGHT9 구조체 맴버가 light attenuation 을 나타낸다. : Attenuation0, Attenuation1, Attenuation2. 이러한 맴버들은 0.0 부터 무한대 까지의 범위의 floating-point 값을 포함하여 light attenuation 을 제어한다. 일부 응용프로그램은 Attenuation 1 맴버를 1.0 으로 다른 맴버를 0.0으로 설정하여 1/D 로 변경되는 빛의 강도를 생성한다. 여기에서 D 는 광원에서 정점 까지의 거리이다. 광원의 빛의 최대 강도는 빛의 범위 내에서 (1/빛의 범위) 로 감소한다. 일반적으로 응용프로그램은 Attenuation0 을 0.0 으로, Attenuation1 를 상수 값으로, Attenuation 2 를 0으로 설정한다.

더욱 복잡한 attenuaion 효과를 얻기 위하여 attenuation 값을 조합할 수 있다. 또는 정삼 범위 외의 값으로 설정하여 낯선(일반적이지 않은) 감쇠 효과를 만들 수도 있다. 그러나 attenuation 값이 음수인것은 허용되지 않는다. [참조](https://learn.microsoft.com/en-us/windows/win32/direct3d9/attenuation-and-spotlight-factor)

### 2. Light Color

Direct3D 에서 Light 는 시스템의 lighting 계산에서 독립적으로 사용되는 세 색상(: diffuse color, amvient color, specular color)을 발산한다. 각각은 렌더링에 사용되는 최종 색상을 생산하기 위해 현재 material 의 상대와 상호작용하는 Direct3D lighting 모듈에 의해 통합된다. diffuse color 는 현재 material 의 diffuse  반사율 속성과 상호작용하고 specular color 는 material 의 specular 반사율 속성과 상호작용한다. Direct3D 가 이러한 색상들을 적용하는 방법에 대한 자세한 내용은 [다음을 참조](https://learn.microsoft.com/en-us/windows/win32/direct3d9/mathematics-of-lighting)

C++ 응용프로그램에서 D3DLIGHT9 구조체는 위의 세 색상(diffuse, ambient, specular) 에 대한 세개의 맴버를 포함한다. 각 맴버는 발산하는 빛의 색상을 정의하는 D3DCOLORVALUE 구조체이다.

시스템의 계산에 가장 무겁게 적용되는 색의 유형은 diffuse color 이다. 가장 흔한 diffuse color 는 white(R:1.0, G:1.0, B:1.0) 이다. 하지만 요구되는 효과의 필요에 따라 색을 만들 수 있다. 예를 들어 벽난로에 red light 를 사용하거나 신호등의 초록불에 green light 를 사용할 수 있다.

일반적으로 light color 구성 요소를 0.0 에서 1.0 사이의 값으로 설정하지만 필수적인 것은 아니다. 예를 들어 white 보다 밝은 light 를 구성하기 위해서 모든 구성 요소를 2.0 으로 설정할 수 있다. 이 유형의 설정은 상수가 아닌 감쇠 설정을 사용할때 특히 유용할 수 있다.

direct3D 가 RGBA 값을 사용하지만 Alpha color 구성요소는 사용되지 않는 것을 주의해야 한다.

일반적으로 material color 는 lighting에 사용된다. 그러나 material color (emissice, ambient, diffuse, specular) 이 diffuse 또는 specular 정점 색상으로 재정의 될 수 있다. 이것은 SetRenderState 를 호출하고 다음의 표에 나열된 device state 변수를 설정하여 수행된다.

![[Pasted image 20230414033803.png]]

알파/ 투명도 값은 언제나 diffuse 색상의 alpha channel 에서만 온다.
fog 값은 언제나 specular color 의 alpha channel 에서만 온다.
D3DMATERIALCOLORSOURCE 는 다음의 값을 같는다.
- D3DMCS_MATERIAL : material color 가 source로 사용된다.
- D3DMCS_COLOR1 : diffuse 정점 색상이 source 로 사용된다.
- D3DMCS_COLOR2 : Specular vertex 색상 은 source 로 사용된다.


### 3. Light Direction

light 의 방향 속성은 오브젝트에서 발산되는 light가 world space 에서 이동하는 방향으로 결정한다. Direction 은 directional light 와 spotlight 로만 사용되고 vector 로 설명된다.

light 의 D3DLIGHT9 구조체의 Direction 맴버 내의 light direction 을 설정한다. Direction 맴버는 D3DVECTOR 유형이다. 방향 벡터는 scene 내의 light 의 위치와는 상관없이 논리적 원점 으로부터의 거리로 설명된다. 그러므로 양의 z 축을 따라 scene을 장면을 똑바로 가리키는 spotlight는 어느 위치에 정의 되었는지에 상관없이 <0,0,1> 방향 벡터를 갖는다. 마찬가지로 방향이 <0, -1, 0> 인 directional light를 사용함으로서 scene에 직접적으로 비치는 햇및을 시뮬레이션 할 수 있다. 분명히, 좌표 축을 따라서 light 를 만들 필요는 없다. 값을 혼합 또는 일치시켜 더욱 흥미로운 각도에서 빛나는 light를 생성할 수 있다. 

note : light 의 방향 벡터의 정규화할 필요는 없지만 언제나 magnitude 를 갖는지 항상 확인해야한다. 다시말해 <0, 0, 0> 인 방향벡터를 사용해서는 안된다.

### 4. Light Position

light position 은 D3DLIGHT9 구조체의 position 맴버에서 D3DVECTOR 구조체를 사용하여 설명된다. x, y, z 좌표는 world space 에 존재한다고 가정한다. Directional light 는 position 속성을 사용하지 않는 유일한 유형의 light 이다.

### 5. Light Range

light의 range 속성은 scene 내의 mesh 가 더이상 해당 오브젝트로부터 발산되는 light 를 받지 않는 world space 의 거리를 결정한다. range 맴버는 world space 내의 light의 최대 범위를 나타내는 floating point 값을 포함한다. directional light는 range property 를 사용하지 않는다.

# Material

^8334af

Material 은 포리곤이 빛을 반사하거나 또는 빛을 3D Scene를 향해 발산 하는 방식을 말한다. Material 의 요소들은 material의 diffuse reflection(분산 반사),  ambient reflection (환경반사), light emission (빛의 방출), specular highlight (반사광) 와 같은 특성들을 세부적으로 정한다. 이러한 세부 사항을 전달할 때에 각 속성은 반사 속성을 제외하고 RGBA 
색으로 설명된다.

### Diffuse and Ambient Reflection

material 에서 Diffuse 와 Ambient 속성은 material 이 scene 에서 diffuse light 또는 ambient light 를 반사하는 방법을 설명하낟. 대부분의 scene 은 ambient light 보다 diffuse light 를 더 많이 포함하고 있기 때문에 diffuse reflection 은 색을 결정하는데 가장 큰 역할을 한다. 게다가 diffuse light 는 
방향성을 갖기 때문에 diffuse light 에 대한 입사각은 반사의 전체적인 강도에 영향을 준다. diffuse reflection 은 light 가 vertex parallel 에서 vertex normal 로 strike 할 때 가장 크다. 각도가 증가함에 따라 diffuse 반사의 효과가 줄어든다. 다음의 그림과 같이 반사되는 빛의 양은 들어오는 빛의 과 vertex normal 사이의 각의 코사인이다. 

![[Pasted image 20230414121709.png]]

ambient 반사는 방향이 없다. ambient 반사는 렌더링된 오브젝트의 색상에 대해 영향이 적다. 하지만 전반적인 색상에 영향을 주며, material 에서 반사되는 diffuse light 가 적거나 없을 때 뚜렷하다. material 의 ambient 반사는 IDrect3DDevice9::SetRenderState 메서드를 D3DRS_AMBIENT flag 를 하여 호출함으로서 scene 에 대해 ambient light 를 설정하는것으로 영향을 받는다.

Diffuse 와 ambient 반사는 오브젝트의 인지되는 색상을 결정하는데에 같이 작동한다. 그리고 그 둘은 보통 동일한 값이다. 예를 들어 파란 수정과 같은 오브젝트를 랜더하기 위해서 diffuse 와 ambient light 의 파란 구성요소만을 반사하는 material 이 필요하다. 그러나 오직 빨간 빛 만이 있는 방안에서 이전과 같은 수정은 검정으로 나타난다. 그것의 material 가 빨간 빛을 반사하지 못하기 때문이다.

### Emission

material 은 렌더링된 오브젝트가 그 스스로 발광하도록 하는 것에 사용될 수 있다. D3DMATERIAL9 구조체의 Emissive 맴버는 발산된 빛의 색상과 투명도를 설명하는데 사용된다. 예를 들어, emission 은 오브젝트의 색상에 영향을 끼친다. 또한 어두운 material 은 밝게 할 수 있고 발산되는 색상의 한 부분을 가질 수 있다. 

material 의 속성을 사용하여 장면에 빛을 추가하는 계산적인 오버헤드를 일으키지 않고도 오브젝트가 빛을 발산하는 등의 발광을 추가할 수 있다. 파란 수정의 경우, emissive property는 해당 수정을 더 밝게 하는데 유용하지만 scene 의 다른 개체에는 빛을 발하지 않는다. Emissive properties 를 갖는 material은 scene 의 다른 오브젝트에 의해 반사될 수 있는 빛을 내보내지 않는다. 반사될 수 있는 빛을 구현하려면 scene 내에 추가 조명을 배치해야 한다.

### Specular Reflection

Specular reflection 은 오브젝트에 highlight 를 생성한다. D3DMATERIAL9 구조체는 material 의 전반적인 광택뿐 아니라 specular highlight 색상을 설명하는 두개의 맴버를 갖는다. Specular 맴버를 RGBA 색상으로 설정하여 specular highlight 의 색상을 정한다. 가장 흔한 색상은 흰색 또는 밝은 회색이다. Power 맴버에 설정한 값은 Specular 효과를 어떻게 선명하게 하는지를 제어한다.

Specular highlight 는 극적인 효과를 만들 수 있다. 다시 한번 파란 수정을 비유로 들어보자. 더 큰 Power 값은 그 수정을 더욱 빛나게 해줄 더 선명한 specular highlight 를 생성한다. 값이 작을 수록 효과의 범위는 증가하여 수정이 서리낀것 처럼 보이게 하는 둔한 반사가 생성된다. 

완전한 무광을 만들기 위해서 Power 맴버를 0으로 설정하고 Specular 를 검정으로 설정한다. 요구 사항에 맞는 현실적인 모형을 생산하기 위해 다양한 레벨의 반사를 실험한다. 다음 그림은 두개의 동일한 모델을 보여준다. 왼쪽의 specular reflection 은 power 가 10 이고, 오른쪽의 모델에는 specular reflection 이 없다.

![[Pasted image 20230414231855.png]]

### Setting Material Properties

Direct3D 렌더링 디바이스는 한번에 하나의 material 속성들의 집합으로 렌더링 할 수 있다.

c++ 응용프로그램에서, D3DMATERIAL8 구조체를 준비하여 시스템이 사용할 material 속성을 설정한고 IDirect3DDevice9::SetMaterial 메서드를 호출한다.
사용 할 D3DMATERIAL9 을 준비하기 위해서 구조체의 속성 정보를 설정하여 렌더링 하는 도중에 원하는 효과를 만든다. 다음의 코드는 선명한 하얀색 specular highlight 를 가진 퍼플 material 에 대한 D3DMATERIAL9구조체를 설정하는 예시이다. 

```c++
D3DMATERIAL9 mat;

// Set the RGBA for diffuse reflection.
mat.Diffuse.r = 0.5f;
mat.Diffuse.g = 0.0f;
mat.Diffuse.b = 0.5f;
mat.Diffuse.a = 1.0f;

// Set the RGBA for ambient reflection.
mat.Ambient.r = 0.5f;
mat.Ambient.g = 0.0f;
mat.Ambient.b = 0.5f;
mat.Ambient.a = 1.0f;

// Set the color and sharpness of specular highlights.
mat.Specular.r = 1.0f;
mat.Specular.g = 1.0f;
mat.Specular.b = 1.0f;
mat.Specular.a = 1.0f;
mat.Power = 50.0f;

// Set the RGBA for emissive color.
mat.Emissive.r = 0.0f;
mat.Emissive.g = 0.0f;
mat.Emissive.b = 0.0f;
mat.Emissive.a = 0.0f;
```

D3DMATERAL9 구조체를 준비한 후에 그 속성들을 렌더링 디바이스의 IDirect3DDevice9::SetMaterial 메서드를 호출한다. 이 메서드는 준비된 D3DMATERIAL9 구조체의 주소를 인자로 받는다. 디바이스에 대한 material 속성의 업데이트가 필요한 경우 새로운 정보로 IDirect3DDevice9::Setmaterial 을 호출할 수 있다. 다음의 예시 코드는 코드에서 이러한 형태가 어떻게 나타나는지를 보여준다.

```c++
// This code example uses the material properties defined for 
// the mat variable earlier in this topic. The pd3dDev is assumed
// to be a valid pointer to an IDirect3DDevice9 interface.
HRESULT hr;
hr = pd3dDev->SetMaterial(&mat);
if(FAILED(hr))
{
    // Code to handle the error goes here.
}
```

Direct3D device 를 생성할 때, 현재 material 은 자동적으로 다음의 표가 나타내는 default 값으로 설정된다.

![[Pasted image 20230414233711.png]]

# Reference

https://learn.microsoft.com/en-us/windows/win32/direct3d9/materials

https://www.3dgep.com/texturing-lighting-directx-11/#Light_Properties