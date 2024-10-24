bOrientRotationToMovement 와 bUseControllerDesiredRotation 는 모두 UCharacterMovementComponent 에 속해있다.

# bOrientRotationToMovement

해당 플래그가 true 이면 캐릭터는 이동방향에 따라 자동으로 회전한다.
이동하는 방향으로 캐릭터의 앞면이 향하도록 한다. 

일반적으로 3인칭 액션 게인이나 RPG 에서 캐릭터가 자연스럽게 이동방향으로 회전하도록 설정할 때 사용된다.

- bUseControllerDesiredRotation 플래그 와의 상호작용
	- bUseControllerDesiredRotation  이 false 인 경우 : 캐릭터는 bOrientRotationToMovement 의 영향을 받아 이동 방향으로 회전한다.
	- bUseControllerDesiredRotation 이 true 인 경우 : 캐릭터는 Controller 의 방향을 따르므로 bOrientRotationRoMevement 의 영향을 받지 않는다. 


# bUseControllerDesiredRotation

해당 플래그가 true 이면 캐릭터는 플레이어의 컨트롤러가 원하는 방향으로 회전한다.
플레이어가 컨트롤러 입력을 통해 설정한 방향에 캐릭터가 맞춰지면, 이동 방향과 상관없이 회전한다.

일반적으로 1인칭 슈팅게인 또는 컨트롤러 입력에 따라 캐릭터가 특정 방향으로 회전해야 하는 게임에서 사용된다.
캐릭터가 플레이어의 시점 또는 조준 방향으로 회전하도록 하여 더 직관적인 조작을 제공할 수 있다.



상호 작용 경우의 수

- bOrientRotationToMovement 가 true 이고 bUseControllerDesiredRotation 가 false 인 경우
	- 캐릭터는 이동방향으로 회전 컨트롤러의 방향은 무시.
- bOrientRotationToMovement 가 true 이고 bUseControllerDesiredRotation 가 true 인 경우
	- bUseControllerDesiredRotation 이 우선 적용된다. 즉, 캐릭터는 컨트롤러가 지정한 방향으로 회전하며 이동 방향은 고려되지 않는다.
- bOrientRotationToMovement 가 false 이고 bUseControllerDesiredRotation 가 false 인 경우
	- 캐릭터의 회전은 기본 회전 방식에 따라 진행되며, 이동 방향이나 컨트롤러 방향에 따라 회전하지 않는다.
- bOrientRotationToMovement 가 false 이고 bUseControllerDesiredRotation 가 true 인 경우
	- 캐릭터는 컨트롤러가 지정한 방햐으로 회전한다. 이동 방향과는 상관 없이 컨트롤러 입력에 따라 회전한다.