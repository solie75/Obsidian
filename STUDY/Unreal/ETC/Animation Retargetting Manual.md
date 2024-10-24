### Retargetting

Add Folder 'Content/ Pistol_Animation/ IK'
- In IK
	- r click -> Animation -> IK Rig -> IK Rig
	- Name 'IK_Pistol_Idle_WithShakingLegs'
- In 'IK_Pistol_Idle_With_LegShaking'
	- Details -> Preview Mesh -> Set 'Preview Skeletal Mesh' to 'Pistol_Idle_with_Shaking_Legs_'
- pelvis -> r click -> Select ' Set Retarget Root'

![[Pasted image 20240811185838.png]]
- In IK
	- r click -> Animation -> IK Rig -> IK Rig
	- Name 'IK_Pistol_Idle_WithShakingLegs_Manny'
![[Pasted image 20240811190017.png]]

- In IK
	- r click -> Animation -> IK Rig -> IK Retargeter
	- Name 'RTG_Pistol_Idle_With_ShakingLegs'

- In 'RTG_Pistol_Idle_With_ShakingLegs'
	- Details -> Source -> Set 'Source IKRig Asset' to 'IK_Pistol_Idle_With_ShakingLegs'
	- Details -> Target -> Set 'Target IKRig Asset' to 'IK_Pistol_Idle_With_ShakingLegs_Manny'
![[Pasted image 20240811201029.png]]

