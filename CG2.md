# Image Formation
이미지 형성

## Objectives
* 기본 영상 개념
* 영상 형성을 위한 물리적 기반
  - 빛
  - 색
  - 통찰력
* 합성 카메라 모델
* 기타 모델

## Image Formation
컴퓨터 그래픽스에서, 물리적 영상 시스템에 의해 이미지가 형성되는 방법과 유사한 과정을 사용하여 2차원 이미지를 형성함
(예) 카메라, 망원경, 현미경, 인간의 눈

## 이미지를 형성하는 요소
* 물체(Objects)
* Viewer
* 광원(Light source(s))
* 빛이 화면안의 물체와 상호 작용하는 방식을 제어하여 이미지를 형성함
* 물체, 보는 사람, 광원은 독립적
![image](https://user-images.githubusercontent.com/35838519/56595320-62df3b80-6629-11e9-88de-4fc3f9e10d9f.png)

## 빛
* 빛은 우리의 시각 시스템에서 반응을 일으키는 전자기 스펙트럼의 일부
* 일반적으로 약 350-750 nm 범위의 파장
* 긴 파장은 빨간색, 짧은 파장은 파란색으로 표시됨

## Ray tracing and Geometric optics
이미지를 형성하는 한 가지 방법은 점 광원으로부터 어떤 광선이 카메라의 렌즈로 들어가는지를 찾는 것이다. 하지만 빛의 각 광선은 흡수되거나 무한대로 이동하기 전에 물체와의 다중 상호 작용을 가질 수 있다
![image](https://user-images.githubusercontent.com/35838519/56595487-b5205c80-6629-11e9-835a-c40c52f5dbf1.png)

## Luminance and Color Images (휘도와 색깔)
*  휘도이미지
  - 일정한 넓이를 가진 광원 또는 빛의 반사체 표면의 밝기를 나타내는 양
  - 단색
  - 회색 값을 가짐
  - 흑백 필름 작업과 유사함
* 컬러 이미지
  - 색조, 채도 및 밝기의 지각 속성 있음
  - 가시 스펙트럼의 모든 주파수를 일치시키지 않아도 됨
  
##  Three-Color Theory (삼색이론)
* 인간 시각 시스템에는 두 가지 유형의 센서를 가지고 있음
  - rods(간상체) (예) 단색, 야간 투시경
  - cones(추상체)
  - 색 감각
    -> 3가지 종류의 cones
    -> 3가지 값(삼자극치)가 뇌로 보내짐
![image](https://user-images.githubusercontent.com/35838519/56596304-4c39e400-662b-11e9-8862-9d9cef6a73d2.png)

* 3가지 원색을 조합하기만 하면 됨
  - 원색 3개만 필요
  
## additive color and subtractive color
* additive color
  - 원색(빨강, 초록, 파랑)을 더해서 색을 만듦
  - (예) LCDs, OLED, projection systems
* subtractive color
  - 청록색, 마젠타, 노란색으로 백색광을 필터링하여 색을 만듦
  - (예) Light-material interactions, printing, negative film

## Pinhole Camera
* 빛이 작은 구멍을 통과해 어떤 물체에 닿으면 그 물체 표면에서 거꾸로 된 상을 만드는 현상을 이용하여 그 상을 보는 장치
![image](https://user-images.githubusercontent.com/35838519/56596439-87d4ae00-662b-11e9-9000-9a141879b435.png)
* 삼각법을 사용하여 (x, y, z) 점의 투영을 찾을 수 있음
![image](https://user-images.githubusercontent.com/35838519/56596526-ac308a80-662b-11e9-86e1-70b089df556f.png)

## Synthetic Camera Model
* 합성 카메라 모델 
 ![image](https://user-images.githubusercontent.com/35838519/56596629-da15cf00-662b-11e9-9bd3-c41efbcd2cdd.png)

* 장점
  - 사물, 보는 사람, 광원 분리함
  - 2차원 그래픽스는 3차원 그래픽스의 특별한 경우
  - 간단한 소프트웨어 API로 연결됨
  - (예) 객체, 조명, 카메라, 속성 지정 등 구현에 따라 이미지가 결정되도록 함
  - 빠른 하드웨어 실행으로 이어짐

## Global vs Local Lighting
* 각 개체의 색이나 음영을 독립적으로 계산할 수 없음
  - 어떤 물체는 빛으로부터 차단되기 때문
  - 빛은 물체에서 물체로 반사될 수 있기 때문
  - 어떤 물건은 반투명일 수도 있기 때문
  ![image](https://user-images.githubusercontent.com/35838519/56596765-1ba67a00-662c-11e9-81b9-704650569d61.png)

## Why not ray tracing?
* ray tracing은 좀 더 물리적으로 기초적인 것 같은데, 그래픽 시스템을 디자인할 때 왜 사용하지 않을까?
  - 단순한 점 소스를 가진 다각형, 이차 곡면과 같은 단순한 객체에 대해서 쉽게 가능
  - 원칙적으로 그림자 및 다중 반사와 같은 전역 조명 효과를 생성할 수 있지만, ray tracing은 느리고 대화형 응용 프로그램에는 적합하지 않음
  - GPU를 사용한 ray tracing은 실시간에 가까움
  
## Ray Tracing
특정 객체 한 지점에서의 색을 결정하기 위해서는 조명으로부터의 직접적인 빛뿐만 아니라 다른 객체로부터 반사나 굴절된 빛 혹은 드리워진 그림자의 영향까지도 고려해야 하는데 ray tracing 방식은 시점에서부터 거꾸로 (현실에서처럼 광원으로부터 빛을 추적하자면 무수히 많은 필요 없는 계산까지 해야 하므로) 반사나 굴절되는 빛을 역 추적해 나감으로써 객체 서로 간에 주고받는 빛의 영향을 계산해 객체 표면색상을 결정하게 된다.

