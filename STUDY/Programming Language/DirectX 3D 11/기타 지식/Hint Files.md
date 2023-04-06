hint file 은 C++ Browsing Database Parser에 의한 코드의 스킵을 야기하지 않는 macro 를 갖는다. Visual Studio C++ project를 열 때, 그 parser 는 프로젝트의 각 소스파일을 분석하고 모든 식별자에 대한 정보로 데이터베이스를 구축(bulid)한다. IDE 는 그 정보로 Class View(클래스 보기) 브라우저 및 Navigation Bar(코드 검색 기능)을 지원한다. 

C++ Browsing Database Parser 는 짧은 시간에 많은 양의 parse 를 할 수 있는 fuzzy parser 이다. 빠른 이유 중 하나는 블록의 내용을 스킵하기 때문이다. 예를들어, 함수의 위치(location)과 매개변수(Parameter)만을 기록하고 그 내용은 무시한다. 특정 매크로는 블록의 시작과 끝을 결정하는데 사용되는 heuristic 에 문제(issue)를 일으킬 수 있다. 이러한 문제는 코드의 영역이 부적적하게 기록되는 것을 야기한다.

스킵된 코드영역은 여러가지의 방법으로 나타낼 수 있다.

- Class View(클래스 보기), Go To (이동), Navigation Bar (탐색 표시줄) 에서 누락된(Missing)  유형과 함수.
- Navigation Bar 의 잘못된(incorrect) 범위
- 이미 정의된 함수에 대한 Create Declaration/Definition (선언/정의 작성) 제안

hint file 은 C/C++ 매크로 정의와 같은 똑같은 구문을 사용하는 user-customizable hint 를 포함한다. Visual C++ 은 대부분의 프로젝트에 대해 효율적인  built-in hint file 을 포함한다. 그러나, 고유한 힌트 파일을 만들어 프로젝트에 맞게 parser 를 향상(개선)할 수 있다.그러나 고유한 힌트 파일을 만들어 프로젝트에 맞게 파서를 개선할 수 있습니다.

! inportant

hint file을 수정하거나 추가하는 경우, 변경 사항이 적용되려면 추가적인 절차를 진행해야 할 필요가 있다.
-   In versions before Visual Studio 2017 version 15.6: Delete the .sdf file and/or VC.db file in the solution for all changes.
-   In Visual Studio 2017 version 15.6 and later: Close and reopen the solution after adding new hint files.

## Scenario

```c++
#define NOEXCEPT noexcept
void Function() NOEXCEPT
{
}
```

hint file 없이 'Function' 은 Class View, Go To, Navigation Bar 에 보이지 않는다. 다음의 매크로 정의를 hint file 에 추가한 뒤에 parser 는 'noexcept' 매크로를 이해하고 대체하며 그 함수를 정확히 parse(구문 분석) 한다.

```c++
// cpp.hint
#define NOEXCEPT
```

  
## Disruptive Macros

parser 를 disrupt 하는 두 가지의 매크로가 있다.

- 함수를 장식하는 키워드를 캡슐화 하는 매크로

```c++
#define NOEXCEPT noexcept
#define STDMETHODCALLTYPE __stdcall
```

이러한 타입의 매크로는 hint file 에 해당 매크로의 이름만이 필요하다.

```c++
//cpp.hint
#define NOEXCEPT
#define STDMETHODCALLTYPE
```

- 짝이 맞지 않는 괄호를 포함한 매크로

```c++
#define BEGIN {
```

이러한 타입의 매크로는 매크로의 이름과 내용 모두 hint file 에서 필요하다.

```c++
// cpp.hint
#define BEGIN {
```

## Editor Support

Starting in Visual Studio 2017 version 15.8 there are several features to identify disruptive macros:

- 파서가 건너뛴 영역 내부에 있는 매크로는 강조 표시된다.
- 강조 표시된 매크로를 포함하는 힌트 파일을 만들거나 기존 힌트 파일이 있는 경우 힌트 파일에 매크로를 추가하는 빠른 작업이 있다.
 ![[Pasted image 20230406210104.png]]

빠른 작업 중 하나를 실행한 뒤 , parser 는 hint file 의 영향을 받는 file 을 다시 구문분석(reparser)한다

- 기본적으로 문제의 매크로는 하이라이트 되지만 그 색상을 바꿀 수 있음
By default, the problem macro is highlighted as a suggestion. The highlight can be changed to something more noticeable, such as a red or green squiggle. Use the **Macros in Skipped Browsing Regions** option in the **Code Squiggles** section under **Tools** > **Options** > **Text Editor** > **C/C++** > **View**.

## Display Browsing Database Errors

The **Project** > **Display Browsing Database Errors** menu command displays all the regions that failed to parse in the **Error List**. The command is meant to streamline building the initial hint file. However, the parser can't tell if the cause of the error was a disruptive macro, so you must evaluate each error. Run the **Display Browsing Database Errors** command and navigate to each error to load the affected file in the editor. Once the file is loaded, if any macros are inside the region, they're highlighted. You can invoke the Quick Actions to add them to a hint file. After a hint file update, the error list is updated automatically. Alternatively, if you're modifying the hint file manually you can use the **Rescan Solution** command to trigger an update.

## Architecture

Hint files relate to physical directories, not the logical directories shown in **Solution Explorer**. hint file 을 적용시키기 위해서 hint file 을 project 에 추가할 필요는 없다. parsing system은 source file 을 parse 할 때에만 hint file 을 사용한다.
모든 hint file 은 'cpp.hint' 로 명명되야한다. 많은 디렉터리가 hint file 을 포함할 수 있지만 특정 디렉터리에서는 오직 하나의 hint file 만이 있을 수 있다.

Your project can be affected by zero or more hint files. If there are no hint files, the parsing system uses error recovery techniques to ignore indecipherable source code. Otherwise, the parsing system uses the following strategy to find and gather hints.


## Search Order

The parsing system searches directories for hint files in the following order.

-   The directory that contains the installation package for Visual C++ (**vcpackages**). This directory contains a built-in hint file that describes symbols in frequently used system files, such as **windows.h**. Consequently, your project automatically inherits most of the hints that it needs.
    
-   The path from the root directory of a source file to the directory that contains the source file itself. In a typical Visual Studio C++ project, the root directory contains the solution or project file.
    
    The exception to this rule is if a _stop file_ is in the path to the source file. A stop file is any file that is named **cpp.stop**. A stop file provides additional control over the search order. Instead of starting from the root directory, the parsing system searches from the directory that contains the stop file to the directory that contains the source file. In a typical project, you don't need a stop file.

### Hint Gathering

A hint file contains zero or more _hints_. A hint is defined or deleted just like a C/C++ macro. That is, the `#define` preprocessor directive creates or redefines a hint, and the `#undef` directive deletes a hint.

The parsing system opens each hint file in the search order described earlier. It accumulates each file's hints into a set of _effective hints_, and then uses the effective hints to interpret the identifiers in your code.

The parsing system uses these rules to accumulate hints:

-   If the new hint specifies a name that isn't already defined, the new hint adds the name to the effective hints.
    
-   If the new hint specifies a name that is already defined, the new hint redefines the existing hint.
    
-   If the new hint is an `#undef` directive that specifies an existing effective hint, the new hint deletes the existing hint.
    

The first rule means that effective hints are inherited from previously opened hint files. The last two rules mean that hints later in the search order can override earlier hints. For example, you can override any previous hints if you create a hint file in the directory that contains a source file.

For a depiction of how hints are gathered, see the [Example](https://learn.microsoft.com/en-us/cpp/build/reference/hint-files?view=msvc-170#example) section.

## Syntax

You create and delete hints by using the same syntax as the preprocessor directives to create and delete macros. In fact, the parsing system uses the C/C++ preprocessor to evaluate the hints. For more information about the preprocessor directives, see [#define Directive (C/C++)](https://learn.microsoft.com/en-us/cpp/preprocessor/hash-define-directive-c-cpp?view=msvc-170) and [#undef Directive (C/C++)](https://learn.microsoft.com/en-us/cpp/preprocessor/hash-undef-directive-c-cpp?view=msvc-170).

The only unusual syntax elements are the `@<`, `@=`, and `@>` replacement strings. These hint-file specific replacement strings are only used in _map_ macros. A map is a set of macros that relate data, functions, or events to other data, functions, or event handlers. For example, `MFC` uses maps to create [message maps](https://learn.microsoft.com/en-us/cpp/mfc/reference/message-maps-mfc?view=msvc-170), and `ATL` uses maps to create [object maps](https://learn.microsoft.com/en-us/cpp/atl/reference/object-map-macros?view=msvc-170). The hint-file specific replacement strings mark the starting, intermediate, and ending elements of a map. Only the name of a map macro is significant. Therefore, each replacement string intentionally hides the implementation of the macro.

`#define` _hint-name_ _replacement-string_  
`#define` _hint-name_ `(` _parameter_, ...`)`_replacement-string_

- A preprocessor directive that defines a new hint or redefines an existing hint. After the directive, the preprocessor replaces each occurrence of _hint-name_ in source code with _replacement-string_.  
- The second syntax form defines a function-like hint. If a function-like hint occurs in source code, the preprocessor first replaces each occurrence of _parameter_ in _replacement-string_ with the corresponding argument in source code, and then replaces _hint-name_ with _replacement-string_.

`@<`

- A hint-file specific _replacement-string_ that indicates the start of a set of map elements.

`@=`

- A hint-file specific _replacement-string_ that indicates an intermediate map element. A map can have multiple map elements.

`@>`

- A hint-file specific _replacement-string_ that indicates the end of a set of map elements.

`#undef` _hint-name_

- The preprocessor directive that deletes an existing hint. The name of the hint is provided by the _hint-name_ identifier.

`//` _comment_

- A single-line comment.

`/*` _comment_ `*/`

- A multiline comment.

# 예제

다음은 hint 들이 hint file 들로부터 쌓이는 방식을 보여준다. 이 예시에서는 Stop file(중지 파일) 이 사용되지 않는다.

다음의 그림은 VS c++ project 의 실제 directory 몇개를 보여준다.

## Hint File Directories

![[Pasted image 20230406215725.png]]

## Directories and Hint File Contents

다음의 리스트는 hint file 을 포함한 project 의 directory 와 포함된 hint file 의 내용을 보여준다. 디렉터리 힌트 파일 의 많은 힌트 중 일부만 `vcpackages`나열된다. 
- vcpackages
```c++
//cpp.hint
#define _In_ 
#define _In_opt_ 
#define _In_z_ 
#define _In_opt_z_ 
#define _In_count_(size)
```
- Debug
```c++
//cpp.hint
#undef _In_ 
#define OBRACE { 
#define CBRACE } 
#define RAISE_EXCEPTION(x) throw (x) 
#define START_NAMESPACE namespace MyProject { 
#define END_NAMESPACE }
```
- A1
```c++
//cpp.hint
#define START_NAMESPACE namespace A1Namespace {
```
- A2
```c++
//cpp.hint
#undef OBRACE
#undef CBRACE
```

# Effective Hints

다음의 table list 는 project의 소스 파일에 효과적인 hint 들이다.

- Source File : A1_A2_B.cpp
- Effective hints:
```c++
// vcpackages (partial list)
#define _In_opt_
#define _In_z_
#define _In_opt_z_
#define _In_count_(size)
// Debug...
#define RAISE_EXCEPTION(x) throw (x)
// A1
#define START_NAMESPACE namespace A1Namespace {
// ...Debug
#define END_NAMESPACE }
```
These notes apply to the preceding list:

-   The effective hints are from the `vcpackages`, `Debug`, `A1`, and `A2` directories.
    
-   The **#undef** directive in the `Debug` hint file removed the `#define _In_` hint in the `vcpackages` directory hint file.
    
-   The hint file in the `A1` directory redefines `START_NAMESPACE`.
    
-   The `#undef` hint in the `A2` directory removed the hints for `OBRACE` and `CBRACE` in the `Debug` directory hint file.
