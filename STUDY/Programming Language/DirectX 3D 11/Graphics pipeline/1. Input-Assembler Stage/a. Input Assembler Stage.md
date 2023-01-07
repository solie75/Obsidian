Direct3D 10 이상의 버전에서 API 는 stage 에 대한 pipeline을 분리시켰다. pipe line의 첫번째 stage는 input Assembler (IA) stage 이다. 

input assembler stage의 목적은 유저가 채운 buffer 로부터 [[Primitives]] data를 읽는 것과 다른 pipeline stage 에서 사용될 primitices로 데이터를 assemble 하는 것이다 . 
IA stage 는 정점들을 몇가지 다른 primitive types (line list, triangle strips, primiticve with adjacency 등)로 어셈블 할 수 있다.
새로운 primitive type (인접한 line list 혹은 인접한 triangle list 와 같은) geometry shader 를 지원하기 위해 추가되어 왔다. 

Adjacency information 은 geometry shader 로만 응용프로그램에 나타난다.
예를 들어 인접성이 포함된 삼각형으로 geometry shader가 호출될 경우, input data는 각 삼각형의 3개의 정점과 매 삼각형마다 adjacency data 에 대한 3개의 정점을 포함한다.

input-assembler stage 가 output adjacency data 를 요청받을 때 input data 는 adjacency data 를 포함해야 한다. This may require providing a dummy vertex (forming a degenerate triangle), or perhaps by flagging in one of the vertex attributes whether the vertex exists or not. #해석필요 또한 이것은 비록 degenerate geometry의 도태가 rasterizer stage 에서 일어날지라도 geometry shader에게 검출[감지]되고 다뤄져야 한다.
 [what is degenerate geometry](https://www.nas.nasa.gov/publications/software/docs/cart3d/pages/degen_ex.html)

assembling primitives 가 진행되는 동안, IA 의 두번째 목적은 shader 를 더욱 효과적으로 만드는 데에 도움을 주기 위해서 [system-generated value](HLSL#^6755d8). <span style="color:yellow ">System-ganerated values는 semantic 이라고도 불리는 text string 이다. </span> 총 세가지의 shader stages 는 common shader core 로 구성되며 그 shader core 는 system-generated values (such as a primitive id, an instance id, or a vertex id)를 사용하기에 shader stage 는 오직 미리 가공되지(processed) 않은 그 primitives, instances, vertices 에 대해 processing을 줄일 수 있다.

[pipeline-block diagram](https://learn.microsoft.com/en-us/windows/win32/direct3d10/d3d10-graphics-programming-guide-pipeline-stages) 에서 볼 수 있듯 , IA stage 가 메모리()로부터 데이터를 읽을 때, 그 데이터는 vertex shader stage 로 출력된다.

