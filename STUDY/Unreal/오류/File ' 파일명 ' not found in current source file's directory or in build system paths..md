Unreal Engine의 new c++ class 가 아닌 VS 에서 새로운 헤더파일을 생성한 경우 해당 헤더파일에서 VS 의 다른 헤더파일을 인식하지 못한다.

uproject 의 Generate visual studio files 로 해결되지 않음.

Intermediate 와 Saved 파일, sln 파일 삭제 후  Generate visual studio file 실행 으로 해결되지 않음

.vcxproj.filters 에 정상적으로 해당 파일이 경로가 지정되어 있음. (파일의 구조적인 문제는 없음)

