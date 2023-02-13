input assembler stage 에 대한 input buffer data 를 설명하는 input-layout object 객체를 생성한다.

# Syntax

```c++
HRESULT CreateInputLayout(
  [in]            const D3D11_INPUT_ELEMENT_DESC* pInputElementDescs,
  [in]            UINT                            NumElements,
  [in]            const void*               pShaderBytecodeWithInputSignature,
  [in]            SIZE_T                          BytecodeLength,
  [out, optional] ID3D11InputLayout**             ppInputLayout
);
```

1. pinputElementDescs
	input assembler stage 의 input data 유형 의 배열이다. 각 유형은 [[D3D11_INPUT_ELEMENT_DESC]]에 의해 정해진다.

2. NumElements
	input element 의 배열에 있는 input data 유형의 수

3. pShaderBytecodeWithInputSignature
	컴파일된 쉐이더를 가리키는 포인터. 컴파일된 쉐이더 코드는 element의 배열에서 유효성 검사가 된 input signature 를 포함한다. 

4. BytecodeLength
	컴파일된 쉐이더의 크기

5. ppInputLayout
	생선된 input layout object를 가리키는 포인터. 다른 입력 매개변수들을 유효성 검사하기 위해서는 NULL 로 설정하고 메서드가 S_FALSE 를 반환하는지 확인한다.


# Remarks

input layout object는 생성된 이후에 draw API 가 호출되기 전, 반드시 input assembler stage 에 바인딩 되어야 한다.
input layout object 가 shader signature 로 생성된 때, input layout object 는 동일한 input signature로 생성(semantics 포함)된 다른 shader 를 사용하여 다시 쓰일 수 있다. 
이것은 동인한 입력을 사용하는 많은 쉐이더로 작업을 할 때 input layout object의 생성을 단순화 한다.

input layout 선언의 data type 이 shader input signature 의 data type 과 다른 경우, CreateInoutlayout 은 컴파일 동안 경고를 발생시킨다. 해당 경고는 단순히 register 에서 데이터를 읽을 때 재해석(reinterprete) 될 수 있다는 사실에 주의하기 위한것이다. You may either disregard this warning (if reinterpretation is intentional) or make the data types match in both declarations to eliminate the warning.