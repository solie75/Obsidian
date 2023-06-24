이미지는 32포멧으로 포토샵으로 바꿔줄 것


BLENDFUNCTION 구조체에 정보를 체우고 AlphaBlend 에 채운다.
AlphaBlend();

TransparentBlt 의 경우 특정 RGB 값을 복사하기를 거부하는 것이고 AlphaBlend 의 경우에는 
( 목적지 RGB * ( 1.f - 소스 Alpha) + 소스 RGB * ( 소스 Alpha) )처리 한다. 

AlphaBlend 의 경우 어떠한 색상을 코드 차원에서 차단하는 것이 아니라  bmp 파일 차원에서 이미지 채널 알파를 가지고 있어 알파값을 가지고 이 알파가 누끼딴형태로 저장되어 있으면 해당 비트 값에 따라 AlphaBlend 함수가 조합하여 나타낸다. 