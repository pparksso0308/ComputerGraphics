## Programming with OpenGL Part 5: More GLSL

## Objectives
* 애플리케이션에 셰이더 연결
  - Reading
  - Compiling
  - Linking
* 정점 특성
* 균일 변수 설정(Uniform)
* 응용 프로그램 

## Linking Shaders with Application
* shaders를 읽음
-> shaders를 compile함
-> 프로그램 객체를 만듬
-> 모든것을 다 link함
-> 어플리케이션에 있는 변수들을 shaders에 있는 변수들과 link함 (vertes attributes, uniform variables등)

## program Object
* shaders를 포함한다.
  - 여러 쉐이더를 포함할 수 있음
  - 기타 GLSL functions
```cpp
GLuint myProgObj;
myProgObj = glCreateProgram();
 /* define shader objects here */
glUseProgram(myProgObj);
glLinkProgram(myProgObj);
```

## Reading a Shader
* Shader가 프로그램 개체에 추가되고 컴파일됨
* 셰이더를 통과하는 일반적인 방법은 glShaderSource() 함수를 사용하여 null로 종단 처리된 문자열로 지정됨
* 셰이더가 파일에 있다면, 우리는 그 파일을 문자열로 변환하기 위해 판독기를 쓸 수 있다.

## Shader Reader
```cpp
#include <stdio.h>

static char*
readShaderSource(const char* shaderFile)
{
    FILE* fp = fopen(shaderFile, "r");

    if ( fp == NULL ) { return NULL; }

    fseek(fp, 0L, SEEK_END);
    long size = ftell(fp);  
        fseek(fp, 0L, SEEK_SET);
    char* buf = new char[size + 1];
    fread(buf, 1, size, fp);

    buf[size] = '\0';
    fclose(fp);

    return buf;
}
```

## Adding a Vertex Chader
```cpp
GLuint vShader;
GLunit myVertexObj;
GLchar vShaderfile[] = “my_vertex_shader”;
GLchar* vSource = 
        readShaderSource(vShaderFile);
glShaderSource(myVertexObj, 1, &vSource, NULL);
myVertexObj = glCreateShader(GL_VERTEX_SHADER);
glCompileShader(myVertexObj);
glAttachObject(myProgObj, myVertexObj);
```

## Vertex Attributes
* 정점 속성이 셰이더에 지정됨
* 링커가 테이블을 구성함 
* 애플리케이션은 테이블에서 인덱스를 가져와 애플리케이션 변수에 연결할 수 있음
* 균일한 변수에 대한 유사한 프로세스

## Vertex Attribute Example
```cpp
#define BUFFER_OFFSET( offset )   
   ((GLvoid*) (offset))

GLuint loc = 
  glGetAttribLocation( program, "vPosition" );
glEnableVertexAttribArray( loc );
glVertexAttribPointer( loc, 2, GL_FLOAT, 
    GL_FALSE, 0, BUFFER_OFFSET(0) );
```

## Uniform Variable Example
```cpp
GLint angleParam;
angleParam = glGetUniformLocation(myProgObj, 
     "angle");
/* angle defined in shader */

/* my_angle set in application */
GLfloat my_angle;
my_angle = 5.0 /* or some other value */

glUniform1f(angleParam, my_angle);
```

## Double Buffering
* 균일한 변수의 값을 업데이트하면 응용 프로그램을 애니메이션화할 수 있는 기회가 열린다.
* 부분적으로 다시 그려지는 프레임 버퍼가 표시되지 않도록 해야 함
* 뒤 버퍼로 그리기
* 전면 버퍼로 표시
* 업데이트 완료 후 버퍼 교체
```cpp
glfwSwapBuffers(window);
```
## Attribute and Varying
* GLSL 1.5 속성 및 다양한 한정자가 내외 한정자로 대체됨
* 애플리케이션 변경 불필요
* 정점 셰이더 예:
```cpp
#version 1.4
attribute vec3 vPosition;
varying vec3 color;

#version 1.5
in vec3 vPosition;
out vec3 color;
```

## Adding Color
* 응용 프로그램에서 색상을 설정하면 정점 특성 또는 변경 빈도에 따라 균일한 변수로 셰이더에 보낼 수 있다.
* 각 정점에 색상을 연결하자.
* 위치와 동일한 크기의 배열 설정
* 정점 버퍼 객체로 GPU로 보내기

## Setting Colors
```cpp
typedef vec3 color3;
color3 base_colors[4] = {color3(1.0, 0.0. 0.0), ….
color3 colors[NumVertices];
vec3 points[NumVertices];

//in loop setting positions

colors[i] = base_colors[color_index]
points[i] = ……. 
```
## Setting Up Buffer Object
```cpp
//need larger buffer

glBufferData(GL_ARRAY_BUFFER,sizeof(points)+sizeof(colors), NULL, GL_STATIC_DRAW);

//load data separately

glBufferSubData(GL_ARRAY_BUFFER, 0, 
   sizeof(points), points);
glBufferSubData(GL_ARRAY_BUFFER, sizeof(points), sizeof(colors), colors);
```

## Second Vertex Array
``cpp
// vPosition, vColors in vertex shader

loc = glGetAttribLocation(program, “vPosition”);
glEnableVertexAttribArray(loc);
glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 0, BUFFER_OFFSET(0));

loc2 = glGetAttribLocation(program, “vColor”);
glEnableVertexAttribArray(loc2);
glVertexAttribPointer(loc2, 3, GL_FLOAT, GL_FALSE, 0, BUFFER_OFFSET(sizeofpoints));
```

## Vertx Shader Applications
* Moving vertices
  - Morphing : 모핑은 하나의 형체가 전혀 다른 이미지로 변화하는 기법이다. 즉 두 개의 서로 다른 이미지나 3차원 모델 사이의 변화하는 과정을 서서히 나타내는 것을 모핑이라 한다.
  - Wave motion
  - Fractals : 프랙탈 또는 프랙털은 일부 작은 조각이 전체와 비슷한 기하학적 형태를 말한다. 이런 특징을 자기 유사성이라고 하며, 다시 말해 자기 유사성을 갖는 기하학적 구조를 프랙탈 구조라고 한다.
* Lighting
  - More realistic models
  - Cartoon shaders

## Wave Motion Vertex Shader
```cpp
in vec4 vPosition;
uniform float xs, zs // frequencies 
uniform float h; // height scale
void main()
{
  vec4 t = vPosition;
  t.y = vPosition.y 
     + h*sin(time + xs*vPosition.x)
     + h*sin(time + zs*vPosition.z);
  gl_Position = t;
}
```

## Praticle System
```cpp
in vec3 vPosition;
uniform mat4 ModelViewProjectionMatrix;
uniform vec3 init_vel;
uniform float g, m, t;
void main(){
	vec3 object_pos;
	object_pos.x = vPosition.x + vel.x*t;
	object_pos.y = vPosition.y + vel.y*t 
       + g/(2.0*m)*t*t;
	object_pos.z = vPosition.z + vel.z*t;
	gl_Position = 
  	ModelViewProjectionMatrix*		vec4(object_pos,1);
}
```

## Pass through Fragment Shader
```cpp
/* pass-through fragment shader */

in vec4 color;
void main(void)
{
     gl_FragColor = color;
}
```

