
# Open Animation Sequencer

### 1. **Unreal Engine 5.4 열기**

먼저 Unreal Engine 5.4를 실행하고 프로젝트를 엽니다.

### 2. **Sequencer 오픈 방법**

#### 방법 1: **Window 메뉴를 통한 오픈**

1. **상단 메뉴에서 `Window` 선택**:
    - Unreal Editor 상단의 메뉴 바에서 `Window`를 클릭합니다.
2. **`Cinematics` 메뉴에서 `Sequencer` 선택**:
    - `Window` 메뉴에서 `Cinematics` 옵션을 찾습니다.
    - `Sequencer`를 클릭하면 Sequencer 창이 열립니다.

#### 방법 2: **레벨 시퀀스를 통해 오픈**

1. **레벨 시퀀스 생성**:
    
    - `Content Browser`에서 마우스 오른쪽 버튼을 클릭하여 컨텍스트 메뉴를 엽니다.
    - `Animation` 또는 `Cinematics` > `Level Sequence`를 선택하여 새로운 Level Sequence 애셋을 생성합니다.
    - 이 애셋에 이름을 지정하고, 원하는 위치에 저장합니다.
2. **Level Sequence 열기**:
    
    - `Content Browser`에서 방금 생성한 또는 기존의 Level Sequence 애셋을 더블 클릭합니다.
    - 이로 인해 Sequencer 탭이 열리고, 타임라인과 관련된 UI가 표시됩니다.

#### 방법 3: **시퀀서 액터를 통해 오픈**

1. **시퀀서 액터 추가**:
    
    - `Place Actors` 패널에서 `Cinematic` 카테고리로 이동하여 `Level Sequence Actor`를 선택합니다.
    - 이를 레벨에 배치합니다.
2. **Sequencer 연결**:
    
    - 레벨에 배치된 `Level Sequence Actor`를 선택한 후, `Details` 패널에서 연결할 시퀀스를 선택하거나 새로 생성합니다.
    - 그런 다음 `Open Sequencer` 버튼을 클릭하면 Sequencer 창이 열립니다

## 시퀀서에 캐릭터 추가하기

먼저 레벨에 캐릭터를 추가합니다. [콘텐츠 브라우저(Content Browser)](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/content-browser-in-unreal-engine)에서 에셋으로 이동한 다음 레벨로 드래그하면 됩니다.다음으로 시퀀스를 열고 캐릭터를 선택한 다음, **트랙 추가(+)** 를 클릭하고 **액터를 시퀀서로(Actor to Sequencer) > 'SKM_Manny2' 추가(Add SK_Mannequin)** 를 선택합니다. 그러면 시퀀스에 캐릭터를 참조하는 트랙이 추가됩니다.

![[Pasted image 20240820232344.png]]

## 캐릭터에 애니메이션 적용하기

애니메이션 트랙의 **애니메이션 추가(+)** 를 클릭합니다. 그러면 캐릭터의 스켈레톤과 호환되는 사용 가능한 애니메이션이 모두 나열됩니다. 이중 하나를 선택하여 시퀀스에 추가합니다.
![[Pasted image 20240820232357.png]]

애니메이션이 추가되면 **플레이(Play)** 를 클릭하여 시퀀스를 미리 봅니다. 현재 엔드포인트를 지나서도 계속 진행되어야 하는 애니메이션이라면 클립의 가장자리를 드래그해서 확장합니다.

![[Pasted image 20240820232408.png]]

![[Pasted image 20240820232414.png]]
만약 애니메이션이 적용되지 않는다면 헤드셋 이모티콘을 누른다.


![[Pasted image 20240821000323.png]]