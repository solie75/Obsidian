# AimGraph

### 1 단계 : Motion Matching Node

Motion Matching System 에서 포즈 선택을 (다루기) 위한 AnimGraph 의 첫번재 노드.

주요 구성 요소
1.  A Pose Search Database : 노드가 Selection(애니메이션 모음) 을 만드는데 사용될 애니메이션 데이터를 포함한다. 게다가, Pose Search Database 에셋 은 Selection 을 만드는데 사용될 포즈나 궤적 데이터를 정의하는 Pose Search Schema Asset 를 사용한다. 
2. A Pose History Node : Animation Data 를 query 하거나 match 하는 데 사용될 pose 와 character Trajectory data 를 포함한다.

해당 예제 에서 Pose Search Database asset 은 Update Motion Matching Function 에 설정되어 있고 'Details' -> 'On Update' 에서 볼 수 있다.

필수적인 입력 인자 외에도, 주목할 만한 기능이 몇개 있다.

Motion Matching node 에서 'Blend Time' float 입력 값을 사용하여 동적으로 blend time 을 변경할 수 있다. 이것은 다른 종류의 애니메이션 끼리 blend 시에 다른 속도가 필요할 경우 유용하다. 해당 예시에서 Blend Time 입력 값은 Get_MMBlendTime 함수로 설정한다. 

Motion Matching Node 는 또한 노드에 적용되는 두개의 AnimNode 함수를 갖는다. 

1. On Update : Update_MotionMatching 함수를 호출.
2. On Motion Matching State Update : Update_MotionMatching_PostSelection 함수를 호출.

Update_MotionMatching 함수는 Chooser Asset 을 평가(evaluating)하고 Motion Matching Database 배열을 반환한다. 이것은 Locomotion data 를 여러 database 로 분리하고 Chooser 가 Animation Data 의  주어진 프레임에서 검색을 할 수 있는 Motion Matching Node 를 

 This allows us to separate our locomotion data into multiple databases and allows the Chooser to filter the pool of animation data the Motion Matching node can search in any given frame, to only relevant animations.

chooser is tool that use when we want to dynamically select a specific asset based on input data 

chooser 자체가 아직 베타 기능이라 일단은 스스로 애니메이션을 만들고 후에 RPG 프로젝트를 만들 때 한번 분석해 보자.