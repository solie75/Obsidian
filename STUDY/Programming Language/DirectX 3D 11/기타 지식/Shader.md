## Vertex Shader

<span style="color: yellow">주로 버텍스 버퍼의 정점들을 3D 공간으로 변형시키도록 작성된 작은 프로그램</span>이다. 화면에 그려질 모든 볼 수 있는 픽셀들에 대해 GPU에서 동작한다. 처리해야할 각 정점에 대해 GPU가 버텍스 쉐이더를 호출한다. 예를 들어 만 개의 폴리곤으로 이루어진 모델하나를 그리기 위해 매 프레임마다 버텍스 쉐이더 프로그램이 만 번 실행된다.

## Pixel Shader

<span style="color: yellow">사용자가 그리는 폴리곤들의 색상을 입히는 것이 작성되는 작은 프로그램</span>이다. 화면에 그려질 모든 볼 수 있는 픽셀들에 대해 GPU에서 동작한다. 색상 입히기, 텍스쳐 입히기, 빚 처리, 그리고 대부분의 폴리곤의 면에 보여지게 될 모든 효과들은 픽셀 쉐이더 프로그램에 의해 처리된다. 