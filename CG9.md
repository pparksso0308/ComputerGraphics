# Programming with OpenGL Part 6 : Three Dimentions

## Objectives
* 보다 정교한 3차원 예제 개발
  - Sierpinski gasket : a fractal
* 숨겨진 표면 제거 소개

## Three-dimensional Applications
* OpenGL에서 2차원 애플리케이션은 3차원 그래픽의 특수한 경우
* 3D로 이동
  - 큰 변화 없음
  - vec3, glUniform3f 사용
  - primitives(원료)가 렌더링되거나 숨겨진 표면 제거가 사용되는 순서를 걱정해야 함
  
## Sierpinski Gasket (2D)
1.삼각형에서 시작
![image](https://user-images.githubusercontent.com/35838519/56703616-b0040000-6744-11e9-99a2-45a4ad6f7903.png)
2.측면의 이등분선을 연결하고 중앙 삼각형을 제거
![image](https://user-images.githubusercontent.com/35838519/56703636-c447fd00-6744-11e9-93df-9dbaffe3cd88.png)
3.반복

* Example : Five Subdivisions
![image](https://user-images.githubusercontent.com/35838519/56703663-e5105280-6744-11e9-991b-07b222ac8387.png)

## The Gasket as a fractal
* 채워진 영역(검은색)과 둘레(채운 삼각형 주위의 모든 선의 길이)를 고려하십시오.
* 계속 세분화하면서 그 영역은 0이 되고 경계는 무한대로 감
* 이것은 평범한 기하학적 물체가 아니다.
  - 2차원도 아니고 3차원도 아니다.
* 프랙탈(프랙탈 차원) 객체다.

## Gasket Program 
```cpp
#include <GLFW/glfw3.h>

/* initial triangle */

point2 v[3] ={point2(-1.0, -0.58), 
           point2(1.0, -0.58), 
           point2 (0.0, 1.15)};

int n; /* number of recursive steps */
```

## Draw one triagle
```cpp
void triangle( point2 a, point2 b, point2 c)

/* display one triangle  */
{
      static int i =0;

      points[i] = a; 
      points[i+1] = b;  
      points[i+2] = c;
      i += 3;
}
```
## Triagle Subdivision
```cpp
void divide_triangle(point2 a, point2 b, point2 c, int m)
{
/* triangle subdivision using vertex numbers */
    point2 ab, ac, bc;
    if(m>0)
    {
        ab = (a + b)/2;
        ac = (a + c)/2;
        bc = (b + c)/2;
        divide_triangle(a, ab, ac, m-1);
        divide_triangle(c, ac, bc, m-1);
        divide_triangle(b, bc, ac, m-1);
    }
    else(triangle(a,b,c));
 /* draw triangle at end of recursion */
}
```
## Initialization / Display
```cpp
/* Initialization */
    vec2 v[3] = {point2(……
    .
    .
    divide_triangles(v[0], v[1], v[2], n);
    .
    .


/* display */
    glClear(GL_COLOR_BUFFER_BIT);
    glDrawArrays(GL_TRIANGLES, 0, NumVertices);

```

## Moving to 3D
We can easily make the program three-dimensional by using 
 ```cpp
 point3 v[3]
 ```
and we start with a tetrahedron(사면체)

## 3D Gasket
* 우리는 네 개의 면을 각각 세분할 수 있다.
![image](https://user-images.githubusercontent.com/35838519/56703889-eee68580-6745-11e9-99ff-b63a8479d22a.png)
![image](https://user-images.githubusercontent.com/35838519/56703891-f0b04900-6745-11e9-8def-ebbabc2d10d4.png)
* 4개의 더 작은 사면체에서 나오는 꽉찬 사면체를 중앙에서 제거하는 것처럼 나타남
* 2D 예와 코드가 거의 동일함

## Almost Correct
삼각형은 프로그램에 지정된 순서대로 그려지기 때문에 앞 삼각형이 항상 뒤에 있는 삼각형 앞에 렌더링되는 것은 아니다.
![image](https://user-images.githubusercontent.com/35838519/56703962-4d136880-6746-11e9-8700-a43c593f9780.png)

## Hidden- Surface Removal
* 우리는 다른 표면 앞에 있는 표면만 보고 싶다.
* OpenGL은 z-buffer 알고리즘이라는 숨겨진 표면 방법을 사용하는데, 이 알고리즘은 객체가 렌더링될 때 깊이 정보를 저장하여 이미지에만 표시되도록 한다.

## Using the z-buffer algorithm
* 알고리즘은 지오메트리가 파이프라인을 따라 이동할 때 깊이 정보를 저장하기 위해 추가 버퍼인 z-버퍼를 사용한다.
* It must be Enabled (사용 가능해야 함) : glEnable(GL_DEPTH_TEST)
* It must be Cleared (삭제 가능해야 함) : glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)

## Surface과 Volume 하위 버전
* 우리의 예에서, 우리는 면을 각각으로 나누었다.
* 또한 같은 중간점을 사용하여 볼륨을 나눌 수 있다.
* 중간점은 각 정점에 하나씩 네 개의 더 작은 사면체를 정의한다.
* 이 사면체만 보관하면 가운데 볼륨이 제거된다.

## Volume Subdivision
![image](https://user-images.githubusercontent.com/35838519/56704341-b9429c00-6747-11e9-93f2-10df15fc8d92.png)




