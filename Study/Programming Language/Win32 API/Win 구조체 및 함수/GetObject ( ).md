GDI Object 에 대한 정보를 구한다. 오브젝트 타입에 따라 적절한 구조체를 선언하고 그 구조체의 포인터를 lpvObject 인수로 전달해 주면 구조체에 조사된 정보를 채워준다.
이 정보들은 일반적으로 오브젝트를 생성할 때 지정한 정보와 동일하다.

# 구문
```c++
int GetObject(HGDIOBJ hgdiobj, int cbBuffer, LPVOID lpvObject);
```
1. hgdiobj : 조사하고자 하는 GDI Objet 의 핸들, 비트맵, 브러쉬, 팬, 폰트 등의 핸들
2. cbBuffer :  버퍼에 기록할 정보의 크기, sizeof(lpvObject)값이다.
3. lpvObject : 오브젝트의 정보를 리턴받을 구조체의 포인터, 핸들의 타입에 따라 사용되는 구조체가 달라진다. 이 인수를 NULL  로 주면 필요한 버퍼의 크기를 리턴한다.

# 반환값

조사된 정보의 크기를 리턴해 준다. lpvObject 인수가 NULL 이면 필요한 버퍼의 크기를 리턴하며 실패시 0을 리턴한다.