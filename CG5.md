# Programming with OpenGL Part 2 : Complete Programs

## Objectives
* 첫 번째 프로그램 전체 작성
  - 셰이더 소개
  - 표준 프로그램 구조 소개
* 단순 보기
  - 3차원 시청의 특수한 사례로서의 2차원 시청
* 초기화 단계 및 프로그램 구조

## program Structure (프로그램 구조)
* 대부분의 OpenGL 프로그램은 다음과 같은 기능으로 구성된 유사한 구조를 가지고 있음
  - main()
    -> callback 함수를 지정
    -> 필요한 속성으로 하나 이상의 창을 킴
    ->이벤트 loop 입력(마지막 실행 문)
  - init()
    -> 상태 변수를 설정 (예) 보기, 특성
  - itShader()
    -> shader를 읽고, 컴파일하고 연결함

## Immediate Mode Graphics
* 정점에 의해 지정되는 형상
  - (예) 공간에서의 위치(2차원 또는 3차원), 점, 선, 원, 다각형, 곡선, 표면
* Immediate mode
  - 응용 프로그램에 정점이 지정될 때마다 그 위치가 GPU로 전송됨
  - 기존 스타일은 glVertex를 사용함
  - CPU와 GPU 간의 병목 현상 발생시킴
  - OpenGL 3.1에서 제거됨

## Retained Mode Graphics (보존 모드 그래픽)
* 모든 정점 및 속성 데이터를 배열에 배치
* 즉시 렌더링 되도록 GPU에 배열 보내기
* 대부분 괜찮지만 문제는 또 다른 렌더링이 필요할 때 마다 배열을 보내야 함
* 여러 렌더링을 위해 GPU에 배열을 보내고 저장하는 것이 더 좋음

## Display
* 데이터를 얻으면 간단한 함수 호출로 렌더링을 시작할 수 있음
  - (예) glDrawArrays(GL_TRIANGLES, 0, 3);
* 배열은 정점 배열을 포함하는 버퍼 객체

## Vertex Arrays
* 정점에는 많은 속성을 가질 수 있음 (예) 위치, 색, 텍스처 좌표
* 정점 배열로 이러한 데이터를 저장

## Vertex Array Object
* 모든 정점 데이터(위치, 색상 등)를 묶음
* 버퍼의 이름을 가져온 후 바인딩함
```CPP
Glunit abuffer;
glGenVertexArrays(1, &abuffer); 
glBindVertexArray(abuffer); //  -> VBO 간에 전환 가능

```
* 이 시점에서 우리는 현재 정점 배열을 가지고 있지만 내용은 없음

## Buffer Object
* 버퍼 객체를 통해 대량의 데이터를 GPU로 전송할 수 있음
* 데이터를 생성, 바인딩 및 식별이 필요함
 ```CPP
 Gluint buffer;
glGenBuffers(1, &buffer);
glBindBuffer(GL_ARRAY_BUFFER, buffer);
glBufferData(GL_ARRAY_BUFFER,sizeof(points), 		points, GL_STATIC_DRAW);
```
* 현재 정점 배열의 데이터가 GPU로 전송됨

## Initialization
: 정점 배열 객체 및 버퍼 객체를 init()에서 설정할 수 있음
: 선명한 색상과 기타 OpenGL 매개 변수를 설정할 수 있음
: 초기화의 일부로 shader를 설정할 수 있음 (예) 읽기, 컴파일, 링크
먼저 몇가지 다른 문제를 생각해보자

## Coordinate Systems
* 점 단위는 응용 프로그램에 의해 결정되며, 객체, 세계, 모델 또는 문제 좌표라고 불림
* 일반적으로 뷰 사양은 객체 좌표에도 있음
* 결국 픽셀은 윈도우 좌표로 생성됨
* OpenGL은 응용 프로그램에 표시되지 않지만, shader에서 중요한 일부 내부 표현을 사용함

## OpenGL Camera
* OpenGL은 음의 z 방향을 가리키는 객체 공간의 원점에 카메라를 배치함
* 기본 보기 부피는 원점에 길이가 2인 옆면을 중심으로 한 상자
 ![image](https://user-images.githubusercontent.com/35838519/56673895-0c8aff00-66f4-11e9-8cd1-93767d1eeab3.png)


## Orthographic Viewing
* 기본 직교 시각에서, 점은 z축을 따라 평면 z=0에 투영됨
![image](https://user-images.githubusercontent.com/35838519/56673918-17459400-66f4-11e9-8295-ca9a3a9128af.png)
![image](https://user-images.githubusercontent.com/35838519/56673926-19a7ee00-66f4-11e9-869d-8eddef461955.png)
 
## Viewports
* 이미지에 대해 전체 창을 사용하지 않음, glViewport(x, y, w, h)
* 픽셀 단위의 값(창 좌표)
![image](https://user-images.githubusercontent.com/35838519/56673965-288ea080-66f4-11e9-8af6-3c534cd8197c.png)

## Transformations and Viewing
* OpenGL에서 투영은 투영 행렬(변환)에 의해 수행됨
* 변환 함수는 좌표계 변경에도 사용됨
* 이전 3.0 OpenGL에는 더 이상 사용되지 않는 일련의 변환 함수가 있음
* 3가지 방법: Application code, GLSL functions, vec.h and mat.h
