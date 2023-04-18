값을 특정 범위 내에 가둬놓고 싶을 때 사용하는 함수.

```c++
#include <iostream>
// important to include algorithm library
#include <algorithm>

using namespace std;
// Range [10-20]
int low = 10, high = 20;

int main()
{
	// Array of 5 having elements to be clamped
	int arr[5] = {8, 12, 20, 25, 5};
	// printing the array after clamped
	cout<<"Numbers before Clamp: ";
	for(int i = 0; i < sizeof(arr); i++)
	{
		cout << arr[i] << " ";
	}
	cout << endl;

	// clamp the elements at every index of array
	for (int i = 0; i < 5; i++)
	{
		arr[i] = std::clamp(arr[i], low, high);
	}
	// printing the array after clamped
	cout << "Numbers after Clamp: "
	for(int i = 0; i < 5; i++)
	{
		cout << arr[i] < " ";
	}
	cout<<endl;
	
}
```

```result
Numbers before Clamp: 8 12 20 25 5
Numbers after Clamp: 10 12 20 20 10
```

