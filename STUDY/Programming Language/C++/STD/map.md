map 에서 동일한 키가 이미 존재하는 경우에는 새로운 요소가 추가되지 않고, 이미 있는 요소의 값이 업데이트 된다.
std::map 은 각 키에 대해 유일한 값을 가지도록 설계되어 있기 때문이다.

```c++
#include <iostream>
#include <map>
#include <string>

int main() {
    std::map<std::wstring, int> mMap;

    // 요소 추가
    mMap.insert(std::make_pair(L"Alice", 100));
    mMap.insert(std::make_pair(L"Bob", 200));

    // 이미 있는 키에 대한 요소 추가 시 업데이트됨
    mMap.insert(std::make_pair(L"Alice", 300));

    // 맵 내용 출력
    for (const auto& pair : mMap) {
        std::wcout << L"Name: " << pair.first << L", Value: " << pair.second << std::endl;
    }

    return 0;
}
```

위의 코드는 결국 기존의 Alice 키에 해당하는 value 가 300 이 된다.