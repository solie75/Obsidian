Actor 나 Component 가 물리적 상호작용을 할 때 어떤 방식으로 충돌을 처리할 지 결정하는 중요한 개념.
Collision Channer 은 특정 물체가 다른 물체와 어떻게 충돌할지, 또는 충돌을 무시할지를 결정하는 방식이다.

- Collision Channel : 각 오브젝트가 충돌할 수 있는 그룹을 정의하는 채널. 각 오브젝트는 자신이 속하는 채널에 따라 충돌을 감지하거나 무시할 수 있다.
- Response : 충돌할 때의 동작을 정의.
	- Block : 충돌을 감지하고 상호작용을 차단한다. 물리적인 반응을 일으킨다.
	- Overlap : 충돌을 감지하지만 상호작용을 차단하지는 않는다. 이벤트는 발생하지만 물리적 충돌은 일어나지 않는다.
	- Ignore : 충돌을 무시하고 아무런 반응도 일어나지 않는다.

### 대표적인 Collision Channel

1. EEC_WorldStatic : 정적인 월드 오브젝트, (벽, 건물 등)
2. EEC_WorldDynamic : 움직일 수 있는 오브젝트, (NPC, 이동하는 플렛폼)
3. ECC_Pawn : 플레이어 캐릭터나 NPC 와 같은 폰에 대한 채널
4. ECC_Visibility : 라인 트레이스와 같이 가시성 체크에 사용되는 채널. (총을 쏠 때 벽이나 물체가 시야를 가로막는지 여부를 판단할 때 사용)
5. ECC_Camera : 카메라와 관련된 충돌.

### 커스텀 Collision Channel

ECC_GameTraceChannel1, ECC_GameTraceC
hannel2 과 같은 채널은 사용자의 필요에 맞게 새로 정의할 수 있는 게임 프레이스 채널이다. 언리얼에서 최대 18 개의 커스텀 채널을 정의할 수 있다.


### Collision Channel 설정

각 오브젝트의 Collision 설정에서 정의 한다. 
