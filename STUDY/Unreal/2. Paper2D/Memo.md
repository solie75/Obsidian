1. Setting
- create game project
create new game project -> ctrl + n -> new Level -> Empty Level -> Create button

- create default folder
Content Folder -> r click -> new folder -> name 'Maps'

- save level 
Maps Folder-> ctrl + s -> Save Level As -> name 'DepMap' in folder 'Maps'

- set default level
menu Edit -> Project Setting -> Maps & Modes -> Default Maps -> Set 'Editor StatUp Map' to  DevMap & Set 'Game Default Map' to DevMap
(this setting make default Level when open this project)

2. defference between Unreal and Unity
Unity : 높은 자유도, 직관적. 예를 들어 빈 게임 객체를 생성한뒤 Mesh Filter, MeshRender, Box Collider 를 사용자 지정할 수 있다.  컴포넌트가 핵심
Unreal : 빈 객체를 생성할 수 없다. 객체를 생성할 때 지정된 부모 클래스를 지정해 줘야 한다. 상속 구조가 핵심

3. Sprite vs Texture
- Add Background Texture
Content Folder -> r click -> New Folder -> name 'Background\_Tex' -> Move png Image(name 'BG') to 'Background_Tex' Folder
draging & droping the Texture to viewport was not working. but if drag & drop to shape, the shape was rapping with 'Background_Tex'

- Change default setting value from 3D setting to 2D setting
'BG' -> r click -> Sprite Actions -> Apply Paper 2D Texture Settings

- Create Sprite
'BG' -> r click -> Sprite Actions -> Create Sprite -> then created  "BG_Sprite"
'BG_Sprite' Drag & Drop to ViewPort

- Set lighting to Original ver.
LIght calculation has been applied to the default screen.  so BG_Sprite in Viewport more bright than original texture.
if want to make "BG_Sprite" looks like original
Way 1 : Place Actors -> Visual Effects -> Drag & Drop Post Process Volume to viewport
PostProcessVolume -> Details -> Bloom -> Set intensity to 0
PostProcessVolume -> Details -> Post Process Volume Settings -> Set Infinite Extent (Unbound) to True
Way 2 : Edit -> Project Settings -> Defualt Setting -> ~

- result
Texture : cover the skin of Shapes
Sprite : Created with PostSpriteActor by it self

\*  if resource in content browser has star marking that means not saved
\* ctrl + shift + s : Save All

4. Filpbook

- make filpbook
move Knight animation Textures to Content folder
create Sprite for all of animation textures
Select Sprite from 'down_attack_01_Sprite' to 'down_attack_08_Sprite' -> r click -> create flipbook -> name 'down_attack'
-> drag & drop 'down_attack' to viewport
-> Set BG_Sprite Transform to ( 0, 0, 0 ) (location)
-> Set 'down_attack' to (0, 10, -420) (Location) & (5, 5, 5) (Scale)
-> checikng knight's animation

down_attack double click -> Details -> sprite -> Set 'Frames Per Second' to desired value
create flipbooks for any animation

make filpbook for any animation
-> move  All filpbooks to Knight folder

- make PaperFilpbookActor
Content Folder -> new Folder (name : Blueprints) -> r click -> Blueprint Class -> All Classes -> PaperFilpbookActor -> name BP_Knight
open BP_Knight window -> Details -> Sprite -> Set Source Filpbook to 'down_attack'

5. Paper Charactor

- Add Camera for knight
Open BP_knight -> Components -> Add SpringArm (name Spring arm) under Render Component -> Add Camera (name camera) under the SpringArm -> Set Camera rotate in the direction of looking straight at the knight 
Camera -> Detail -> Camera Setting -> set Projection Mode to Orthographic and Set Ortho Width to 2048

- Set distance between Camera and knight
springArm -> Details -> Camera -> Set distance value to Target Arm Length

- Change parent class for apply possess
BP_Knight -> Class Settings -> Details-> Class Options -> Set Parent Class to 'Papaer Character'
BP_Knight -> Class Defaults -> Details -> Sprite -> set Source Flipbook to 'down_attack'
// -> Details -> Transform -> Scale -> (5.0, 5.0, 5.0)

DevMap -> Select BP_Knight -> Details -> Pawn -> Set Auto Possess Player to 'Player0'
(Auto Possess Player : Determines which PlayerController, if any, should automaticaly possess the pawn when the level starts of when the pawn is spawn. 레벨이 시작되거나 폰이 스폰될 때 어떤 PlayerController가 폰을 자동으로 소유해야 하는지 결정합니다.) ^3ad4bd

=> So if start simulator, Camera focus Knight. but knight was falling
because of BP_Knight  inherited 'Paper Character'. Charater Class was applied laws of p
hysics

-> BP_Knight -> Defailts -> Physics -> Character Movement (General Settings) -> Set Grabity Scale to 0 => Knight no longer falling

6. input mapping

- change player faceing direction with 'keyboard event' node
BP_Knight -> Event Graph -> create KeyBoad event (w, a, s, d) node
r click -> create Set Flipbook 4 times and match each Keybroard event node -> Set appropriate flipbook value to 'New Flipbook' in 'Set Flipbook' node
BP_Knight Component -> Sprite -> drag & drop to event graph -> create Get Sprite Node -> link to each 'Target' in 'Set Flipbook' node 

Edit -> Project Settings -> Engine -> Input -> bindings -> Axis Mappings -> Add
![[Pasted image 20240513174408.png]]
https://www.unrealengine.com/en-US/blog/input-action-and-axis-mappings-in-ue4

click Keyboard mark and push key that you want

result :  Knight's flipbook is displayed according to the key value that entered

- change player faceing direction with input Axis node
BP_Knight -> Event Graph -> delete W, S, A, D node -> r click -> select MoveUp and Move Right-> create InputAxis MoveUp and Input Axis MoveRight

\* inputAsix MoveUp
pressing W key -> InputAxis MoveUP return 1
pressing S key -> inputAxis MoveUP return -1
no pressing any key and pressing other keys -> inputAxis MoveUp return 0

InputAxis MoveUP node -> Branch (condition :  Axis Value in 'IputAxis MoveUp' -> == with 0) -> Branch (condition : Axis Value in 'InputAxis MoveUp' -> > with 0) -> true link to Set Flipbook node for up_idle and false link to Set Flipbook node for down_idle

InputAxis MoveRight node -> Branch (condition :  Axis Value in 'IputAxis MoveRight' -> == with 0) -> Branch (condition : Axis Value in 'InputAxis MoveUp' -> > with 0) -> ( true link to Set Flipbook node for side_idle -> Set Relative Rotation node -> set (0, 0, 0) to New Rotation ) and to ( false link to Set Flipbook node for side_idle -> Set Relative Rotation node -> set (0, 0, 180) to New Rotation (for Left/Right Inversion) )

7. Macro
in 6, classifying Axis Value through branches is made more concise using Macro.

Axis Value in Input Axis Move -> input in Compare Float -> ' > ' link to Set Flipbook for up_idle and ' < ' link to Set Flipbook for down_idle

- what is deference between function and macro?
One of the biggest differences between a function and a macro is that you can place latent nodes in a macro, but not in a function.
Functions allow you to override their functionality in child Blueprints.
https://docs.unrealengine.com/4.27/en-US/ProgrammingAndScripting/Blueprints/BestPractices/

- how to make macro
Level Blueprint Editor-> My Bluprint tap -> Macros -> Add button 

8. Animation Update

- Add Enumeration for Facing Direction
DevMap -> Contetn Browser -> Blueprints -> r click -> blueprint -> Enumeration (name EDirection) -> Add Enumator 4 times -> Set Display Name (Up, Down, Left, Right ) to each element

DevMap -> MyBlueprint -> Variables Add -> name Direction Set type to EDirection

- Set Direction Enum value for input key value
![[Pasted image 20240514145700.png]]
InputAxis Move Up ->
Event Graph -> Compare Float ' > '  -> Set Direction node (set Direction to Up)
Event Graph -> Compare Float ' < '  -> Set Direction node (set Direction to Down)
InputAxis MoveRight ->
Event Graph -> Compare Float ' > '  -> Set Direction node (set Direction to Right)
Event Graph -> Compare Float ' > '  -> Set Direction node (set Direction to Left)


- Add Function for Animation Update
DevMap -> MyBlurprint -> Functions Add -> name Updateanimation
UpdateAnimation  -> Switch on EDirection (Set Direction to 'Selection')
copy from Get Sprite node to Set Relative Rotatio node in Event Graph
and paste to Update Animation
' Up ' in Switch on EDirection link to Set FlipiBook for up_idle
' Down ' in Switch on EDirection link to Set FlipiBook for down_idle
' Right ' in Switch on EDirection link to Set FlipiBook for right_idle
' Left ' in Switch on EDirection link to Set FlipiBook for left_idle

- Add Move Animation
if none of W, A, S or D is pressed,  Set bKeyboardPressed to false. Otherwise Set bKeyboardPressed to true
![[Pasted image 20240514145500.png]]
add move animation branchs
![[Pasted image 20240514150705.png]]

9. Attack Animation (animation is played just once)
Add valueables (name : bAttack, type : boolean)
edit -> project settings -> engine -> input -> Action Mappings Add  (name Attack) -> Set Attack Key to Spacebar

![[Pasted image 20240514154000.png]]
![[Pasted image 20240514154125.png]]

10. State pattern

- refactoring for Set Flipbook node
create 'Select' node -> Set Direction value to 'Index' -> input pin r click -> 'change pin type' to 'paper flipbook' -> Select Asset according to Direction value
![[Pasted image 20240514155055.png]]
![[Pasted image 20240514161045.png]]

- Add EState (Enumeration)
Attack, Idle, Move

\- Update Animation
![[Pasted image 20240514180408.png]]
use 'Switch on Estate' node and Select node about boolean

![[Pasted image 20240514180706.png]]

- Add Update Input func
![[Pasted image 20240514230824.png]]

![[Pasted image 20240514230853.png]]


11. Move

- way 1  : use SetActorLocation ( Set current position + move value to current position)
Add function (name : Update Logic)
![[Pasted image 20240515023208.png]]

apply Update Logic func
![[Pasted image 20240515023237.png]]

- 'Sweep's  explain in SetActorLocation  node
Whether we sweep to the destination location k, triggering overlaps along the way and stopping short of the target if blocked by something. Only the root component is swept and checked for blocking collision , child components move without sweeping. If collision is off, this has no effect.

if Sweep checked, actor can't through collider object

- way 2 : use Character Movement Component
\- DevMap -> Select Knight -> Details -> Physics -> Charactoer movement (General Settings -> Set Gravity Scale to 1 )
\- place Actors -> drag & drop Blobking volume to under the knight
\- Update Logic -> Get Charactor Movement node -> 'Add Input Vector' node (Unlike way 1, which directly calculates the position after movement, only the vector for the direction of movement is input)

Set Update Logic for move to right
![[Pasted image 20240515145821.png]]

print current acceleration and velocity
![[Pasted image 20240515150504.png]]

speed is increased according to acceleration
![[Pasted image 20240515145758.png]]

- Add Movement Input node
Setting to 'World Vector (5.0, 5.0, 5.0)' in 'Add Input Vector node' equal Setting two input value, which 'World Direction (1.0, 1.0, 1.0)' and 'Scale Value (5.0)' in 'Add Movement Input node'

Unlike using 'Set Actor Location node', this node dose not require any additional Delta Time Calculations

- update 'Update Logic' func to both right and left are possible
![[Pasted image 20240515182853.png]]

- Add Up and Down Moving
BP_Knight-> Components -> Character Movement  -> Details -> Character Movement (General Settings) -> Set 'default land movement mode' to Flying

- Add Deceleration
BP_Knight-> Components -> Character Movement  -> Details -> Character Movement : Flying -> Set Braking Deceleration Fluing to 2048

- decrease sliding when change face direction
BP_Knight-> Components -> Character Movement  -> Details -> Character Movement (General Setting) -> Set 'Max Acceleration' to 100000 (As the value increase, sliding decrease)

- decrease sliding when stop
BP_Knight-> Components -> Character Movement  -> Details -> Character Movement : Flying -> Set Braking Deceleration Fluing to 100000 (As the value increase, sliding decrease)

12. Lay out Inheritnce struct 
Create Class Name BP_Creature -> Move Update Logic func  and Update Animtion func in BP_Knight to BP_Creature

<\BP_Creature Event Graph>
![[Pasted image 20240516124826.png]]
{fix Monster Attack -> InputAction Attack, Delete Print Text}

<BP_Creature Update Logic>
![[Pasted image 20240516124859.png]]

<BP_Creature Update Animation>
![[Pasted image 20240516124936.png]]

- Create Class -> name BP_Monster
monster dosen't require to input Key value

<BP_Monster Event Graph>
![[Pasted image 20240516125112.png]]

<BP_Monster Update Animation>
in the existing 'Update Animation', the 'Set Flipbook node' sets Flipbook by hard coding. However, this is a cumbersome task to do every time you add a class.
Threrfore the values to ve entered in parts that must be used publicly are managed as data.

13. Data Table
- Add DataTable
Bluprints Folder -> Add new folder name data -> Add Structure file name Creature_Animation_Structure -> init data of CreatureData
![[Pasted image 20240517094740.png]]
Data Folder -> Add Data Table (in r click -> Miscellaneous) -> Set 'Pick Row Struecture' to 'CreatureData' -> name Creature_Animation_DataTable
![[Pasted image 20240517094802.png]]

- Set DataTable
open CreatureDataTable -> Add twise -> Set 'Row Name' to Knight and to Skeleton -> Set Row Editor

- use DataTable
BP_Creature -> Add two Variables, (name ClassName, type Name) and (name CreatureAniData type Creature Animation Structure)
BP_Knight -> Event Graph -> Event BeginPlay -> Set Class Name node (Set Class Name to Knight)
BP_Creature -> Event Graph -> Event BeginPlay -> Set Creature Ani Data
![[Pasted image 20240517100105.png]]
BP_Knight -> Event Graph -> Call Parent BeginPlay
![[Pasted image 20240517103325.png]]

BP_Creature-> UpdateAnimation
![[Pasted image 20240517100513.png]]

- Add Skeleton Class
Blueprints folder -> BP_Monster -> r click -> Create Child Blueprint Class -> name BP_Skeleton

BP_Skeleton -> Event Graph
![[Pasted image 20240517103420.png]]

BP_Monster -> Event Graph
![[Pasted image 20240517103447.png]]


14. Collision decision
 BP_Knight -> Add Box Collideion Under the Sprite  -> name Attack Collider
![[Pasted image 20240517154636.png]]

rename Attack Event node to 'Event BeginAttack' -> Create Function (name Process Attack) -> Add Process Attack Func Between 'Update Animation Func Node' and 'Delay node'
![[Pasted image 20240517175034.png]]

Set Process Attack func
![[Pasted image 20240517175053.png]]

In simulation, When Knight was Attack, count of object class that monster or child of monster in attack range is printed.

- Create Structure of Creature Default Data
Blueprints folder -> Data folder -> Create Structure (name Creature Default structure) -> Creature DataTable (Pick Row Structure : Creature_Default_Structure, name : Creature_Default_DataTable)

Creature_Default_Structure
![[Pasted image 20240517214521.png]]

Creature_Default_DataTable
![[Pasted image 20240517214553.png]]

- Set Creature Default Data to variable
![[Pasted image 20240517215344.png]]

- Set 'Init Data' Func
![[Pasted image 20240517220421.png]]
- Set Hp
![[Pasted image 20240519144015.png]]

- Add Function (name OnDamaged in BP_Creature)
![[Pasted image 20240519144131.png]]

- Add Function (name Process Attack in BP_Creature)
![[Pasted image 20240519144316.png]]

- Call Process Attack Func between UpdateAnimation and Delay
![[Pasted image 20240519144409.png]]

- Call Begin Attack
![[Pasted image 20240519151634.png]]

- Add Attack Collider (Up, Side, Down)
![[Pasted image 20240519151702.png]]
![[Pasted image 20240519151720.png]]

15. Hp Gauge

- Create HP Bar 
\- Blueprints Folder -> Add Folder (name UI) -> r click -> User Interface -> Widget Blueprint -> User Widget -> Select button -> name WBP_HpBar
\- Change Fill Screen to Custom -> Set Width to 200, Set Height to 25
\- Palette -> Common -> drag & drop Progress Bar
\- Hierarchy tab -> rename Progress Bar to HpProgressBar
\- Hierarchy -> HpProgressBar -> Details -> Check to 'Is Variable'

- Set Hp Bar to BP_Creature
\- BP_Creature -> Components tab -> Add -> Select Widget -> name HpBarWidget
\- BP_Creature -> Components tab -> select HpBarWidget -> Details -> User Interface -> Set Widget Class  to 'WBP_HpBar'
\- HpBarWidget -> Details -> User Interface -> Set 'Draw SIze' to 200,10

- Set Hp bar Percent

Way 1 : Set Hp Percent with 'Set Percent Node' to WBP_HpBar Directly
\- In 'On Damaged Func', "HpBarWidget" in Component is frame for widget that to be implemented. Fill that frame with "WBP_HpBar class" using "Get User Widget Object Node". And cast 'class of returned' to 'WBP_HpBar'.
\- Get Hp Progress Bar -> Set Percent 
![[Pasted image 20240520020437.png]]

Way 2 : 

\2-1 :  bind to function
\- details -> progress -> Percent -> Bind -> Select Create binding
\- function is created that bind to percent automatically

2-2 : bind to variable
\- WBP_HpBar -> Graph -> Variables -> Add -> name HpRatio, type Float\ -> Set Default Value to 1
\- WBP_HpBar -> Designer -> progress -> percent -> bind -> Select HpRatio
![[Pasted image 20240520022602.png]]

16. FSM

BP_Monster -> Add AttackCollider (Up, Down, Side)
Add 'Update AI' Function
![[Pasted image 20240520220449.png]]

link Update AI to Event Tick -> Parent : Tick -> Update AI

17.  Controller
A PlayerController is the interface between the Pawn and the human player controlling it.
if use The Way that input Function and Ai belong to class, it is more difficult that managing. So Manage the input Function and Ai apart from class with controller.

Menu Bar -> Window -> Select World Setting

BP_Knight -> Details -> Pawn -> Set Auto Possess Player  to Player 0 : already setting this with [[STUDY/Unreal/2. Paper2D/Memo#^3ad4bd| Set Controller]]

if several Knight spawned, only one Knight that have controller is working about input or AI.

- The Way that one controller used by two Knights
Add Two Knight Object and name (BP_Knight1, BP_Knight2)
Outliner tab -> Select BP_Knight -> Details ->  Pawn -> Set Auto Possess Player to Player 0
![[Pasted image 20240521104527.png]]

- Move 'Update Input Function' in Knight Class to 'Player Controller'.
\- Delete BP_Knight classes

\- Blueprints Folder -> r click -> Blueprint Class -> Select Game Mode Base -> name BP_GameMode
World Settings tab -> Game Mode -> Set GameMode Override to BP_GameMode


\- Blueprints Folder -> r click -> Blueprint Class -> Select Player Controller -> name BP_Player_Controller
World Settings tab -> Game Mode -> Selected GameMode -> Set Player Controller Class to BP_PlayerController

\- Blueprints Folder -> r click -> Blueprint Class -> Select AIController -> name BP_AIController

\- BP_PlayerController -> Create UpdateInput -> Delete Update Input Func in BP_Knight
![[Pasted image 20240521131631.png]]

\- DepMap -> Place Actors -> drag & drop Player Start to viewport
\- World Setting -> Selected Game Mode -> Set Default Paen Class to BP_Knight / Set Player Controller Class to BP_PlayerControll -> Set Location of Player Start Object

- Set AI Controller
Add Variables (name TargetObject, type BP_Skeleton; name TargetEnemy, type Bp_Knight'; name chaseDirection, type Vector)
Add Update AI Func
![[Pasted image 20240521171937.png]]
Set BP_AIController -> Event Graph
![[Pasted image 20240521172008.png]]

delete Update AI Func in BP_Monster

- Apply AI Controller
BP_Skeleton -> Details -> Pawn -> Set AI Controller Class to BP_AIController

18. Tile
Content Folder -> Sprite Folder -> Add Folder, name TileMap -> Add tile image

tile image -> r click -> Sprite Actions -> Create Tile Set -> name BP_TileSet -> open BP_TileSet
![[Pasted image 20240521172842.png]]
Single Tile Editor tab -> Set Tile Size to 32, 32 (size of one tile)

TileMap Folder -> r click -> Paper 2D -> Select Tile Map, name TileMap
![[Pasted image 20240521173330.png]]

TileMap -> Set 'Active Tile Set' to BP_TileSet
Details -> Setup -> Set Tile Width to 32, Set Tile Height to 32, Set Map Width to 10, Set Map Height to 10
Select Fill -> Select on the far right tile (ice wall) -> L click white boundary area in Beta Preview -> The area have filled by tile (ice wall)
Select Paint -> Select any tile -> L click in that area -> a tile that clicked was change to tile that selected.
make Layer 1 (basic background), Layer 2 (Ice wall)
Layer 1
![[Pasted image 20240522035725.png]]
layer 2
![[Pasted image 20240522035822.png]]

visualize both of Layers

![[Pasted image 20240522035855.png]]

Dev Map -> Delete BG_Background, floor collider
Drag & drop TileMap to viewport -> Set Location 0, 0, 0

TileMap -> Details -> Tile Layer List : several layer can in one  tilemap.
Detail -> Setup -> Separation Per Layer : interval  between each layer
(You can give several background effects by adding different Textures between interval. )

- Apply collision on Tile Map
BP_TileSet -> Select 'Colliding Tiles' : Tile that have collision option is marked with green
BP_TileSet -> Select Ice Wall Tile -> Add Box
( Add Box (Collider is Box), Add Polygon (Collider is custom), Add Circle(Collider is Circle) )

TileMap -> Details -> Collision -> Set Collision Thickness to 200

DevMap -> viewport -> show -> Check Collision : The tile with the collider applied is confirmed to be thick

19. Extracting Tile Map Information
Set moving interval to per tile not float.

TileMap Folder -> r click -> Blueprint Class -> Select PaperTileMapActor -> name BP_TileMap

open BP_TileMap -> Components -> Render Component -> Details -> Tile map -> Set Tile Map to 'TileMap'
Details-> Transform -> Set Scale 5, 1, 5

DevMap -> Outliner -> Delete TileMap
Drag & Drop BP_Tilemap to viewport

BP_TileMap -> Transform -> Set Location 0, 0, 0

In Tile class specify boolean value to each tile according to whether the tile can allow to accessing with player.

- Add function that whether of the player can go specified direction.
BP_TileMap
![[Pasted image 20240523162626.png]]
CanGo Function
![[Pasted image 20240523161633.png]]

20. Tile Level Movement

- Set player location to specified tile position
\- Create 'sGridPos' Structure
have two variables (name X,  type int; name Y type int )

\- Create Vailable in BP_Creature (name GridPos, type sGridPos)

\- Create Function in BP_TileMap (name Get Tile World Pos) with pure
input : two integer (name GridX and GridY)\
output : one Boolean (name Valid), one Vector (name Location)
![[Pasted image 20240525023350.png]]

\- Create Function in BP_Creature (name SetDestination)
![[Pasted image 20240524103756.png]]

\- Call SetDestination Func on BP_Knight
![[Pasted image 20240524103844.png]]

- Create Function that sets value to destination and checks arrive to destination.

\- Update 'Update Input' on PlayerController class  : if not arrived to destination, State can't change from move to idle
![[Pasted image 20240524112719.png]]


\- Create Function name 'HasArrivedToDest' in BP_Creature
![[Pasted image 20240525013930.png]]

\- Update 'Update Logic'
![[Pasted image 20240525014257.png]]

\- Create Function name 'Update Destination'
![[Pasted image 20240525014350.png]]

\- Add Valuable (name bStop, type boolean) in BP_Creature
set bStop
![[Pasted image 20240525014920.png]]
Apply bStop
![[Pasted image 20240525015115.png]]

21. Monster Spawn


\- Add function (name GetRendomEmptyGridPos) with pure
![[Pasted image 20240525032220.png]]

\- Add function (name SpawnCreature)
![[Pasted image 20240525032237.png]]

\- Add input  (name SpawnClass, type BP_Creature with Class Reference)

\- Update 'SetDestination' Func in BP_Creature
![[Pasted image 20240525032352.png]]

\- turn off Creature's Collision
BP_Creature -> Capsule Component -> Details -> Collision -> Set Collision Presets to NoCollision

\- change Player Spawn from 'Event Graph-> BeginPlay -> ; in BP_Knight' to 'BP_TileMap -> Event Graph'

22. Creature Collide
Set Collide Function between Knight and Skeleton.
Change Attack Collide from Collision Box to Tile

- asd
\- Add Variable (name Creatures, type BP_Creature with Array) in BP_Tile
\- Add Creature to Creatures Var
In Spawn Creature Func, Add return value of Spawn Actor Func to Creature Var
![[Pasted image 20240525134932.png]]
-> in BP_Tile Map, Event Graph , Apply Adding to Creature Var on Knight Spawn
![[Pasted image 20240525135033.png]]

\- Create GetCreatureAtGridPos Func
Add input value (Grid X, Grid Y both type is  integer)
Add output value (name bFound type boolean, name Creature type BP_Creature)
![[Pasted image 20240525135544.png]]

\- Update Can Go Func
![[Pasted image 20240525224434.png]]

- Change standard of collision decision from Box Collision to whether Creature be on tile

\- delete Attack Collider Component in BP_Knight
\- delete input of Begin Attack Func (Attack Collider)
\- delete input of Process Attack Func(Attack Collider)
-> Update Process Attack Func
![[Pasted image 20240525233000.png]]

23. Update Monster AI
BP_Monster -> Details -> Pawn -> Set 'Auto Possess AI' to 'Placed in World or Spawned'

Update ' Update AI ' Func about Begin Attack (Attack Collider was deleted.)
![[Pasted image 20240525235751.png]]

Play this level and Delete -> 
Blueprint Runtime Error: "Accessed None trying to read property TargetObject". Node:  Switch on EState Graph:  UpdateAI Function:  Update AI Blueprint:  BP_AIController
![[Pasted image 20240526131424.png]]
Return Value of Get Controlled Pawn Func is Null. Because of Event BeginPlay called before Possessing. For get Possesed Pawn, use Event On Possess instead of Event BeginPlay.
![[Pasted image 20240526134022.png]]

- Update Collision of Skeleton

\- Create 'Get Tile Count To Target' Fuc in BP__Creature
![[Pasted image 20240526145235.png]]

\- Create 'Look At Target'
![[Pasted image 20240526145727.png]]

\- override 'Update Destination' Func at BP_Monster
![[Pasted image 20240526145751.png]]

\- Update Moving Logic in 'Update AI Func' of BP_AIController.
![[Pasted image 20240526150115.png]]

24. Monster Despawn

- Despawn Skeleton
\- Create 'DeSpawn Creature' Func in BP_TileMap
![[Pasted image 20240526153040.png]]

\- Create 'OnDead' Func in BP_Creature
![[Pasted image 20240526153052.png]]

\- Update OnDamaged Func.
![[Pasted image 20240526152709.png]]

-> Skeleton having hp 0 is deleted

- Print Dead Skeleton Num

\- UI Folder -> r click -> User Interface -> Widget Bluprint -> Select User Widget -> name WBP_GameUI

\- WBP_GameUI -> Designer -> Palette -> Panel -> Drag % Drop Canvas Paner  to Viewport
(if Panel has Created, you can't create other UI Object ,out of The Panel)

(* what is anchor)
![[Pasted image 20240526171224.png]]
if you add ui object in canvas the anchor is appear. Anchors are used to define a UI Widget's desired location on a Canvas Panel and maintains that position with varying screen sizes.

\- Add TextBox
palette -> Drag& Drob Text to canvas -> name killCountText

\- Add Variable
Graph -> Add Variables -> name KillCount, type Integer

\- bind Text Box to KillCount Var
Designer -> Hierarchy -> Select KillCountText -> Details -> Appearace -> Color and Opacity -> Select opacity to CreateBinding -> name GetKillCount

\- Add Create Widget Func -> Select Class to WBP_GameUI -> name is changed to Create WBP_GameUI Widget

\- Add Valiables in BP_TileMap -> name GAmeUI, type WBP_GameUI 
![[Pasted image 20240526174907.png]]

\- Update OnDead Func
Add input (name Attacker, type BP_Creature)
![[Pasted image 20240526183316.png]]

\- Set Attacker Value to OnDead Func
![[Pasted image 20240526183707.png]]

![[Pasted image 20240526183831.png]]


25. Mangement Structure

What is GameMode class
defines the rules and settings for a game. It controls the game flow and gameplay mechanics such as spawning of players, scoring, and what happens when a player dies.
https://docs.unrealengine.com/4.26/en-US/InteractiveExperiences/Framework/GameMode/

\- Any class can access to member variables of GameMode class
BP_GameMode -> Add Variables (name TestInGameMode, type integer)
Call Get Func for TestInGameMode
![[Pasted image 20240526193502.png]]

- Game Instance

GameMode is useful to Management. but subordinated to only one 'Level'.
To perform Managing on All Level, using Instance class that upper the Game mode class

Blueprints Folder -> r click -> Blue Print class -> Pick Parent Class -> search Game Instance -> Select -> name BP_GameInstance

open BP_GameInstance -> Edit -> Project Settings -> Maps & Modes -> Game Instance -> Set Game Instance Class to BP_GameInstance

Add Variable (Name UserId, type Integer (single))

Get UserId in BP_Creature
![[Pasted image 20240531150612.png]]

- Casting Functionalization 
Blueprints Folder -> r click -> blueprint -> Blueprint Function Library -> name BP_FunctionLib -> Add Function (name GetBPGameInstance)
![[Pasted image 20240531160534.png]]

Call GetBPGameInstance Func in BP_Creature
![[Pasted image 20240531160757.png]]

26. Component

Add Actor Component (name BP_ActorComp)
Add Scene Component (name BP_SceneComp)
컴포넌트 를 Creature에 추가.
Actor Component 는 Transform 형식을 가지면서 Creature에 단계적으로 종속 되고 Scene Component 는 Character MOvement 와 같이 단순 종속 된다.

BP Actor Comp 를 BP_SpawningPool 로 이름 변경


함수가 어떤 변수를 변형시키지 않는경우 Datils -> Graph -> Check Pure

Q. get actor location 노드 와 get actor transform 의 차이는 무엇인가?

여러 객체의 Event BeginPlay 들에 대해 호출 되는 순서는 어떻게 정해지는가?