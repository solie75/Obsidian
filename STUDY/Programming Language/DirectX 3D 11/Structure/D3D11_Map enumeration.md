CPU 에서 읽기 및 쓰기를 위해 접근할 resource 를 선별한다. 응용프로그램은 이러한 flag 들을 하나 이상 결합 할 수 있다.

# Syntax

```c++
typedef enum D3D11_MAP {
  D3D11_MAP_READ = 1,
  D3D11_MAP_WRITE = 2,
  D3D11_MAP_READ_WRITE = 3,
  D3D11_MAP_WRITE_DISCARD = 4,
  D3D11_MAP_WRITE_NO_OVERWRITE = 5
} ;
```

# Constants

1. D3D11_MAP_READ
리소스가 reading을 위해 매핑됨. 그 리소스는 read access로 생성되었다.(see [D3D11_CPU_ACCESS_READ](https://learn.microsoft.com/en-us/windows/win32/api/d3d11/ne-d3d11-d3d11_cpu_access_flag)).

2. D3D11_MAP_WRITE
리소스가 writing을 위해 매핑됨. 그 리소스는 write access로 생성되었다.
access (see [D3D11_CPU_ACCESS_WRITE](https://learn.microsoft.com/en-us/windows/win32/api/d3d11/ne-d3d11-d3d11_cpu_access_flag)).

3. D3D11_MAP_READ_WRITE
리소스가 reading 과 writing 을 위해 매핑됨. 그 리소스는 read 와 write access 로 생성되었다. (see [D3D11_CPU_ACCESS_READ and D3D11_CPU_ACCESS_WRITE](https://learn.microsoft.com/en-us/windows/win32/api/d3d11/ne-d3d11-d3d11_cpu_access_flag)).

4. D3D11_MAP_WRITE_DISCARD
리소스가 writing 을 위해 매핑됨. 리소스의 지난 contents 은 식별되지 않는다. 리소스는 write access 로 생성되었다. (See [D3D11_CPU_ACCESS_WRITE](https://learn.microsoft.com/en-us/windows/win32/api/d3d11/ne-d3d11-d3d11_cpu_access_flag) and [D3D11_USAGE_DYNAMIC](https://learn.microsoft.com/en-us/windows/win32/api/d3d11/ne-d3d11-d3d11_usage)).

5. D3D11_MAP_WRITE_NO_OVERWRITE
리소스가 wriring 을 위해 매핑됨. 리소스의 현재 존재하는 content 가 overwirte 되지 않을 수 있다. 이 flag 는 오직 vertex buffer 와 index buffer 에게만 유효하다. resource 는 write access 로 생성되었다.  (see [D3D11_CPU_ACCESS_WRITE](https://learn.microsoft.com/en-us/windows/win32/api/d3d11/ne-d3d11-d3d11_cpu_access_flag)).

# Remarks

## Meaning of D3D11_MAP_WRITE_NO_OVERWRITE

D3D11_MAP_WRITE_NO_OVERWRITE 은 응용 프로그램이 input assembler 단계에서 사용중인 data 에 write 하지 않는 다는 의미이다. 

