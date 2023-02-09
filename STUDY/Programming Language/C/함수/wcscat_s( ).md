
# Syntax

```c++
errno_t wcscat_s( 
	wchar_t *strDestination, 
	size_t numberOfElements, 
	const wchar_t *strSource 
);

template <size_t size> 
errno_t wcscat_s( 
	wchar_t (&strDestination)[size], 
	const wchar_t *strSource 
); // C++ only
```

#### 1. strDestination
NULL 로 끝나는 문자열

#### 2. numberOfElements
NULL 종단 문자열 크기

#### 3. strSource
더할 문자열


# Remark

The **`strcat_s`** function appends _`strSource`_ to _`strDestination`_ and terminates the resulting string with a null character. The initial character of _`strSource`_ overwrites the terminating null character of _`strDestination`_. The behavior of **`strcat_s`** is undefined if the source and destination strings overlap.

The second parameter is the total size of the buffer, not the remaining size:

```
char buf[16];
strcpy_s(buf, 16, "Start");
strcat_s(buf, 16, " End");               // Correct
strcat_s(buf, 16 - strlen(buf), " End"); // Incorrect
```

**`wcscat_s`** and **`_mbscat_s`** are wide-character and multibyte-character versions of **`strcat_s`**. The arguments and return value of **`wcscat_s`** are wide-character strings. The arguments and return value of **`_mbscat_s`** are multibyte-character strings. These three functions behave identically otherwise.

If _`strDestination`_ is a null pointer, or isn't null-terminated, or if _`strSource`_ is a `NULL` pointer, or if the destination string is too small, the invalid parameter handler is invoked, as described in [Parameter validation](https://learn.microsoft.com/en-us/cpp/c-runtime-library/parameter-validation?view=msvc-170). If execution is allowed to continue, these functions return `EINVAL` and set `errno` to `EINVAL`.

The versions of functions that have the **`_l`** suffix have the same behavior, but use the locale parameter that's passed in instead of the current locale. For more information, see [Locale](https://learn.microsoft.com/en-us/cpp/c-runtime-library/locale?view=msvc-170).

In C++, using these functions is simplified by template overloads; the overloads can infer buffer length automatically (eliminating the need to specify a size argument) and they can automatically replace older, non-secure functions with their newer, secure counterparts. For more information, see [Secure template overloads](https://learn.microsoft.com/en-us/cpp/c-runtime-library/secure-template-overloads?view=msvc-170).

The debug library versions of these functions first fill the buffer with 0xFE. To disable this behavior, use [`_CrtSetDebugFillThreshold`](https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/crtsetdebugfillthreshold?view=msvc-170).

By default, this function's global state is scoped to the application. To change this behavior, see [Global state in the CRT](https://learn.microsoft.com/en-us/cpp/c-runtime-library/global-state?view=msvc-170).