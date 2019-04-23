# What is Computer Graphics?

## Objectives
* In this lecture, we explore what computer graphics is about and survey some application areas
* We start with a historical introduction

## Computer Graphics
컴퓨터 그래픽은 컴퓨터로 이미지를 만드는 모든 측면을 다룬다.
* 하드웨어
* 소프트웨어
* Applications

## Example
* Where did this image come from?
![image](https://user-images.githubusercontent.com/35838519/56588396-370a8880-661e-11e9-9ff3-c6193d7f5fb0.png)

* What hardware/software did we need to produce it?

## Preliminary Answer (예비 답변)
* Application : 이 물체는 애니메이션이 돔 환경(플란타륨)에서 보여질 수 있도록 화가가 태양을 재현한 것이다.
* 소프트웨어 : 모델링 및 렌더링을 위한 Maya, OpenGL을 기반으로 구축된 Maya
* 하드웨어 : 모델링 및 렌더링을 위한 그래픽 카드가 있는 PC

## Basic Graphics System
![image](https://user-images.githubusercontent.com/35838519/56588587-9cf71000-661e-11e9-9e07-62b3b3978cff.png)

Input Devices -> Image formed in frmae buffer -> Output Device

## Computer Graphics : 1950~1960
* 컴퓨터 그래픽은 컴퓨팅의 초기 시대로 거슬러 올라간다.
    - 펜 플롯터
    - A/D 변환기를 사용하여 컴퓨터에서 서예 CRT로 이동하는 간단한 디스플레이
* 비용 
    - 느리고, 비싸며, 신뢰할 수 없음

## Computer Graphics : 1960 ~ 1970
* wireframe graphics
  - 오직 선만 그림
  ![image](https://user-images.githubusercontent.com/35838519/56592725-2eb64b80-6626-11e9-9441-fa0c1815c17f.png)

* sketchpad
  - Ivan Sutherland(이반 서덜랜드)의 MIT 박사 논문
    -> 사람과 기계의 상호작용의 가능성을 인식함  
  - 어떤 것을 보여주고, 사용자가 펜을 움직이면, 컴퓨터는 새로운 모습을 생성하는 과정을 반복함(loop)
  - 또한 Ivan Sutherland는 컴퓨터 그래픽스를 위한 많은 일반적인 알고리즘을 만듦

*	display processor
  - 호스트 컴퓨터가 모습을 재구성하지 않고 특별한 목적 컴퓨터인 display processor(DPU)를 사용함
  - 그래픽스는 display processor의 display list(display file)에 저장됨
  - 호스트는 display list를 컴파일하고 display processor로 보냄
  ![image](https://user-images.githubusercontent.com/35838519/56593042-b8feaf80-6626-11e9-97e9-201190758cb5.png)

* direct view storage tube
  - Tektronix가 만듦
  - 지속적인 재구성이 필요 없음
  - 컴퓨터에 대한 표준 인터페이스
    -> 표준 소프트웨어에 허용됨
    -> Fortran의 Plot3D
  - 상대적으로 저렴함
      ->CAD 커뮤니티용 컴퓨터 그래픽스 사용 가능
      
## Computer Graphics : 1970 ~ 1980년대 
* Raster Graphics
  - 프레임 버퍼 내의 사진 요소(픽셀)의 배열(raster)로서 이미지를 생성함
  - wireframe(선)에서 다각형으로 채워진 이미지를 만들 수 있게 됨

*그래픽스 표준의 시작
  - IFIPS
    -> GKS(유럽인들의 노력, ISO 2D의 표준이 됨)
    -> Core(북아메리카인들의 노력, 3D지만 ISO 표준이 되는 것에 실패함)

* workstations and PCs
  - 더 이상 workstation과 PC를 구별하지 않지만, 역사적으로 다른 뿌리에서 진화함
    ->초기 workstation의 특징
      + 고객-서버 모델 네트워크 연결
      + 높은 수준의 상호 작용
    -> 초기 PC의 특징
      + 사용자 메모리의 일부로 프레임 버퍼가 포함되어 콘텐츠 변경 및 이미지 작성이 용이

## Computer Graphics: 1980-1990
* 컴퓨터 그래픽이 현실성 있게 됨
* 특수한 목적의 하드웨어 (예) 실리콘 그래픽 기하학 엔진
* 산업기반 표준 (예) PHIGS, RenderMan
* 네트워크 그래픽스 (예) X 윈도우 시스템
*HCI(human-computer interface)

## Computer Graphics: 1990-2000 
* OpenGl API
* 컴퓨터 제작 장편 영화(토이 스토리) 성공
* 새로운 하드웨어 기능 (예) Texture mapping, Blending, Ray tracing

Computer Graphics: 2000-
* 포토리얼리즘
* PC의 그래픽카드가 시장을 지배함 (예) Nvidia, ATI
* 게임이 시장의 방향을 결정함
* 영화 산업에서의 컴퓨터 그래픽스 적용 (예) Maya, Houdini
* 프로그램 가능한 파이프라인



