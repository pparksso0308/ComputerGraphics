# Models and Architectures

## Objectives
* 그래픽 시스템의 기본 사항 학습
* 파이프라인 아키텍처 소개
* 대화형 그래픽 시스템의 소프트웨어 구성 요소 검사

## 이미지 형성(Image formation) 복습
* 합성 카메라 모델(synthetic camera model)을 모방하여 그래픽스 하드웨어와 소프트웨어를 설계할 수 있는가?
  - 응용 프로그램 프로그래머 인터페이스(API)로 가능
    ->사물, 자재, 보는 사람, 광원을 지정하는 것이 필요함
* API는 어떻게 구현되는가?

## Physical Approaches (물리적 접근법)
* ray tracing : 투영 중심에서 물체에 흡수되거나 무한대로 사라질 때까지 광선을 따름
  - 전역 효과 처리 가능(다중 반사, 반투명 물체)
  - 느림
  - 전체 데이터베이스가 항상 사용가능해야 함
![image](https://user-images.githubusercontent.com/35838519/56598609-c40a0d80-662f-11e9-8803-649632ad6e51.png)

* radiosity : 에너지 기반 접근법
  - 매우 느림

## Practical Approach (실용적인 접근법)
* 응용 프로그램에서 생성된 순서대로 개체를 한 번에 하나씩 처리함, local lighting만 고려할 수 있음
* 파이프 라인 구조
![image](https://user-images.githubusercontent.com/35838519/56598743-10ede400-6630-11e9-9f04-275522ea25de.png)
*  그래픽 카드에서 모든 단계를 구현할 수 있음

## Vertex Processing
* 파이프 라인의 많은 작업은 객체 표현을 하나의 좌표계에서 다른 좌표계로 변환하는 것
(예) 객체 좌표, 카메라 좌표, 화면 좌표
* 모든 좌표 변경은 행렬 변환과 같음
* vertex processor는 vertex의 색도 계산함
![image](https://user-images.githubusercontent.com/35838519/56598841-41ce1900-6630-11e9-8542-2f2923222276.png)

## Projection
* 3D 뷰어와 3D 객체를 결합하여 2D 이미지를 생성하는 프로세스
  - 투시 투영 : 모든 투영기가 투영 중심에서 만남
  - 평행 투영 : 투영기들이 평행하고 투영 중심이 투영 방향으로 대체됨
![image](https://user-images.githubusercontent.com/35838519/56598910-67f3b900-6630-11e9-8ea2-01c002acd33b.png)

## Primitive Assembly(원시 집합체)
: clipping(자르기), rasterizer가 수행되기 전에 정점은 기하학적 객체로 수집되어야 함
(예) 선분, 다각형, 곡선과 표면
![image](https://user-images.githubusercontent.com/35838519/56599039-aa1cfa80-6630-11e9-8ba6-69e16bba6c73.png)

## clipping(가위질)
* 실제 카메라가 전 세계를 볼 수 없는 것처럼, 가상 카메라는 세계 또는 객체 공간의 일부만 볼 수 있음
* 볼륨 내에 있지 않은 개체는 장면에서 clipped out된 상태라고 함
![image](https://user-images.githubusercontent.com/35838519/56599090-d173c780-6630-11e9-967c-248f29c18a8b.png)
![image](https://user-images.githubusercontent.com/35838519/56599101-d5074e80-6630-11e9-8f4c-3d7ccf41e988.png)

## rasterization(벡터 또는 윤곽선 데이터를 비트맵으로 바꾸는 과정)
* 만약 객체가 clipped out되지 않으면, 프레임 버퍼의 적절한 픽셀에 색상이 할당되어야 함
* rasterizer는 각 객체에 대해 fragment set을 생성함
* fragment는 프레임 버퍼에 위치를 갖고 있고, 색상 및 깊이 속성인 "잠재적 픽셀"임
* 정점 속성은 rasterizer에 의해 객체 위에 덧붙여짐
![image](https://user-images.githubusercontent.com/35838519/56599153-f7996780-6630-11e9-80d1-7aa49ec8d378.png)

Fragment Processing
* fragment는 프레임 버퍼에서 해당 픽셀의 색상을 결정하기 위해 처리됨
* 색상은 정점 색상의 텍스처 맵핑이나 덧붙여지는 방식으로 결정될 수 있음
* fragment은 카메라에 가까이 있는 다른 fragment에 의해 차단될 수 있음 (예) 숨겨진 표면 제거
![image](https://user-images.githubusercontent.com/35838519/56599198-0c75fb00-6631-11e9-8740-43d6526fb141.png)

## The Programmer’s Interface
* 프로그래머는 소프트웨어 인터페이스를 통해 그래픽스 시스템을 봄
(예) API(Application Programming Interface)
![image](https://user-images.githubusercontent.com/35838519/56599267-2e6f7d80-6631-11e9-96eb-970d435e59b4.png)

## API 내용
* 이미지를 형성하기 위해 필요한 항목을 지정하는 함수 (예) 객체, 보는 사람, 광원, 재료
* 기타 정보 (예) 마우스 및 키보드 등의 장치에서 입력, 시스템의 기능

* Object Specification
* 대부분의 API는 다음과 같은 제한된 기본 요소 집합을 지원함
  - 점(0D object)
  - 선(1D object)
  - 다각형(2D object)
  - 일부 곡선 및 표면 (예) 2차 함수, parametric 다항식
* 모든 것은 공간이나 정점의 위치를 통해 정의된다.

## example(old style)
 ![image](https://user-images.githubusercontent.com/35838519/56599424-7e4e4480-6631-11e9-8f43-88568148cd93.png)

## example(GPU based)
* 기하학적 데이터를 배열에 넣기
```cpp
vec3 points[3];
points[0] = vec3(0.0, 0.0, 0.0);
points[1] = vec3(0.0, 1.0, 0.0);
points[2] = vec3(0.0, 0.0, 1.0);
```
* GPU에 배열 보내기
* 삼각형으로 렌더링하도록 GPU에 지시

## 카메라 사양
* 자유도 6도 (예) 렌즈 중앙 위치, 방향)
* 렌즈
* 필름 크기
* 필름 평면의 방향
![image](https://user-images.githubusercontent.com/35838519/56599619-e56bf900-6631-11e9-9539-eb45226207e4.png)

## 빛과 재질
* 빛의 종류
  - 점 광원 VS 분산 광원
  - 스포트 라이트
  - 근거리 및 원거리 광원
  - 색상 특성

* 재질 특성
  - 흡수(Absorption) : 색상 속성
  - 산란(scattering) (예) 확산(Diffuse), 반사(specular)





