# Programming with OpenGLPart 1: Background

## Objectives
* OpenGL API 개발
* OpenGL 아키텍처
    - 상태 기계로서의 OpenGL
    - 데이터 흐름 기계로서의 OpenGL
* Functions 
    - Types
    - 형식
* 심플 프로그램

## 초기 API의 역사
* IFIPS(1973)는 표준 그래픽스 API를 만들기 위해 두개의 위원회를 결성함
    - GKS(Graphical Kernel System) : 2D이지만 좋은 워크 스테이션 모델 포함
    - Core : 2D와 3D
    - GKS는 IS0와 ANSI standard로 채택됨 
* 3D로 쉽게 확장되지 않음(하드웨어의 뒤떨어진 개발)

## PHIGS and X
* PHIGS (Programmers hierarchical Graphics System)
   - CAD 커뮤니티에서 가져옴
   - 보존된 그래픽스(구조)가 있는 데이터베이스 모델
* X Window System
  - DEC/MIT의 노력
  - 그래픽스가 있는 클라이언트-서버 구조
* PEX\
  - PHIGS와 X를 합친 것
  - 각각의 모든 결함을 포함했기 때문에 사용하기 쉽지 않음

## SGI and GL
- SGI는 하드웨어에 파이프 라인을 구현하여 그래픽스 work station에 혁명을 일으킴 (1982)
- 시스템에 접근하기 위해 응용 프로그램 프로그래머는 GL이라는 라이브러리를 사용함
- GL을 사용하면 3차원 대화형 응용 프로그램을 프로그래밍하는 것이 비교적 간단

## OpenGL
* GL의 성공은 플랫폼 독립적인 API인 OpenGL(1992)로 이어짐
   - 사용하기 쉬움
   - 하드웨어에 충분히 근접하여 뛰어난 성능 제공
   - 렌더링에 집중
   - 윈도우 시스템 종속성을 방지하기 위해 윈도우 설정 및 입력 누락

## OpenGL Evolution
* 원래 ARB(Architectural Review Board)에 의해 제어됨
: SGI, Microsoft, Nvidia, HP, 3DLabs, IBM 등이 포함된 회원
: 현대의 Kronos Group
: 상대적으로 안정적(버전 2.5)
: 구버전과 호환 가능
: 새로운 하드웨어 기능을 반영하며 진화
(예) 3D texture mapping과 texture objects, vertex 와 fragment 프로그램
: 확장을 통해 특정 기능을 플랫폼에 허용함

Modern OpenGL
: CPU가 아닌 GPU를 사용하여 성능 달성
: shaders라는 프로그램을 통해 GPU 제어
: 응용 프로그램의 일은 데이터를 GPU로 보내는 것
: GPU가 모든 렌더링을 수행
 

OpenGL 3.1
: 완전히 shaders 기반
: 기본 shader 없음
: 각 응용 프로그램은 vertex와 fragment shader를 모두 제공해야 함
: 즉시 모드 없음
: 상태 변수 없음
: 대부분의 2.5개 기능이 사용되지 않음
: 이전 버전과의 호환성은 필요하지 않음

다른 버전
- OpenGL ES
: 임베디드 시스템
: Version 1.0 단순화된 OpenGL 2.1
: Version 2.0 단순화된 OpenGL 3.1(Shader 기반)
- WebGL
: ES 2.0의 자바 스크립트 구현
: 최신 브라우저에서 지원됨
- OpenGL 4.x
: 형상 shader 및 tessellator 추가

What About Direct X?
- 장점 : 자원 관리가 용이함, 높은 수준의 기능에 대한 접근
- 단점 : 이전 버전과 호환되지 않는 새 버전, Windows 전용
: 최근 shader의 발전은 OpenGL과의 융합을 이끌고 있음

OpenGL Libraries
- OpenGL core library
: OpenGL32 Window에서 OpenGL32
: 대부분의 유닉스/리눅스 시스템에서 GL(libGL.a)

- OpenGL Utility Library(GLU)
: OpenGL core의 기능을 제공하지만 코드를 다시 쓸 필요가 없음
: legacy code에서만 작동 가능
- Links with window system
: GLX for X window systems
: WGL for Windows
: AGL for Macintosh

GLFW
- OpenGL Utility Library
: 모든 윈도우 시스템에 기능 제공
(예) 두 번의 함수 호출만으로 창 열기, 키보드, 마우스 및 게임 패드에서 입력 받기, 입력 및 이벤트 수신, 여러 개의 창
: 오픈 소스
-> 기타 라이브러리는 OpenGL ES, Vulkan에서 지원

GLEW
: OpenGL 확장 Wrangler Library
: 특정 시스템에서 사용 가능한 OpenGL 확장에 쉽게 접근 가능
: Windows 코드에서 특정 입력 지점을 사용하지 않음
: 응용 프로그램은 glew.h를 include하고 glewInit()를 실행하면 됨

OpenGL 구조
 

OpenGL 함수
- 원형 : 점, 선, 삼각형
- 속성
- 변환 (예) 보기, 모델링
- 제어(GLFW)
- 입력(GLFW)
- 문의

OpenGL 상태
: OpenGL은 state 기계임
: OpenGL의 함수는 두가지 유형으로 구성됨
- primitive 생성
: primitive가 보이는 경우 출력이 발생할 수 있음
: 정점 처리 방법 및 primitive의 외관은 상태에 의해 통제됨
- 상태 변화 (예) 변화 함수, 속성 함수

객체 방향 부족
: OpenGL은 주어진 논리적 함수에 대해 여러 함수가 존재하도록 객체 지향적이지 않음
: C++에서 오버로드 된 함수를 쉽게 만들지만 효율성이 문제임

OpenGL 함수 형태
 

OpenGL #defines
: 대부분의 상수는 포함 파일 gl.h, glu.h 및 glfw.h에 정의되어 있음
-> #include <GLFW/glfw3.h>은 자동으로 다른 것을 포함해야 함
: 파일을 포함하는 것 또한 OpenGL 데이터 유형을 정의함

OpenGL and GLSL
: Shader 기반 OpenGL은 상태 시스템 모델보다 데이터 흐름 모델에 기반함
: 대부분의 상태 변수, 속성 및 관련 이전 3.1 OpenGL 함수는 더 이상 사용되지 않음
: shader에서 동작이 발생함

GLSL
: OpenGL Shading 언어
: C와 같은 행렬 및 벡터 형식(2, 3, 4 차원), 오버로드 된 연산자와 C++와 같은 생성자 사용
: Nvidia의 Cg 및 Microsoft HLSL와 유사함
: 코드는 shader에 소스코드로써 전송됨
: 컴파일, 링크 및 shader의 정보를 얻는 새로운 OpenGL 함수
