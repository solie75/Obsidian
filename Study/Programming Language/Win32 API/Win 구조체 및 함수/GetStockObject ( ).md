## Stock

사전적 의미 : 재고품, 저장품, 주식 자본
미리 만들어져서 가지고 있는 것을 보통 'Stock'이라고 한다. 

## Stock Object

응용 프로그램은 API 함수를 사용해서 개발에 필요한 다양한 자원(Resource)을 운영체제로부터 제공받는다. 이때 자원은 내용 혹은 속성이 바뀌지 않는 것이 많은데 매번 응용 프로그램이 요청할 때마다 운영체제가 해당 자원을 구성(메모리에 할당)해서 전달하는 것은 매우 비효율적이다.
따라서 <span style="color:orange">운영체제는 사용빈도가 높고 내용이 바뀌지 않는 자원들을 미리 만들어서 가지고 있는데 이것을 'Stock Resource' 혹은 'Stock Object' 하고 한다.</span>
#StockObject

## GetStockObject

GetStockObject 함수는 'Stock Object'의 핸들 값을 얻을 때 사용한다. 이 함수로 얻을 수 있는 'Stock Object'는 GDI Object와 관련된 자원이고 그 중에 사용 빈도가 높은 속성인 Brush, Pen, Font, Palette에 대한 핸들값을 얻는다.

- 원형
```c++
HGDIOBJECT GetStockObject(int fnObject);
```
이 함수는 한 개의 매개변수를 갖으며 fnObject 변수에 상숫값을 명시하면 그 상숫값에 해당하는 'Stock Object'의 핸들 값이 HGDIOBJ 자료형으로 반환된다. 

- 반환 값
<span style="color:orange">GetStockObject 함수로 얻은 핸들값은 DeleteObject 나 CloseHandle을 사용해서 제거하면 안된다.</span>

