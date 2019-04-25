#  Programming with OpenGL Part 4 : Color and Attributes

## Objectives
* 원시 집합 확장(Expanding Primitive Set)
* 색상 추가
* 정점 특성 
* 균일 변수(Uniform Variables)

## OpenGL Primitives(OpenGL 기본값)
![image](https://user-images.githubusercontent.com/35838519/56702137-71b71280-673d-11e9-9488-d9a55aacd0e9.png)

## Polygon Issues
* OpenGL은 삼각형만 표시됨
  - Simple : 가장자리가 교차할 수 없음
  - Convex: 폴리곤에서 두 점 사이의 라인 세그먼트의 모든 점 또한 폴리곤에 있음
  - Flat: 모든 정점이 동일한 평면에 있음
* 응용 프로그램에서는 폴리곤을 삼각형으로 세공해야 함
* OpenGL 4.1에 테셀레이터 포함
![image](https://user-images.githubusercontent.com/35838519/56702200-d6726d00-673d-11e9-94c0-9d8a2548055c.png)

## Polygon Testing
* 단순성과 볼록성을 테스트하는 것이 개념적으로 간단함
* 하지만 시간이 많이 걸린다. 
* 이전 버전은 응용 프로그램에 대한 테스트를 모두 가정함
* 현재 버전은 삼각형만 렌더링함
* 임의 다각형을 삼각측량하는 알고리즘 필요

## Godd and Bad Triagles
* 긴 삼각형이 잘 그려지지 않는다.
![image](https://user-images.githubusercontent.com/35838519/56702318-4c76d400-673e-11e9-83ed-6e663b611ecb.png)

* 정삼각형이 잘 그려진다.
* 최소 각도 최대화
* 구조화되지 않은 점에 대한 지연 삼각 측량

## Triangularization (삼각형화)
* Convex Polygon(볼록한 다각형)
![image](https://user-images.githubusercontent.com/35838519/56702361-8ea01580-673e-11e9-853f-5e768af23b54.png)
* abc로 시작해서 b를 제거한 다음 acd, .......
 
## Non-convex (concave)

## Recursive Division (재귀분업) 
* 가장 왼쪽 정점 찾기 및 분할

## 속성
* 속성이 객체의 모양을 결정한다
  - 색상(점, 선, 폴리곤)
  - 크기 및 폭(점, 선)
  - 스티플 패턴(라인, 폴리곤)
  - 다각형 모드
    -> 채워진 상태로 표시: 솔리드 컬러 또는 스티플 패턴
    -> 가장자리 표시
    -> 정점 표시
* 일부(glPointSize)만 OpenGL 기능 지원

## RGB color
* 각 색상 구성요소는 프레임 버퍼에 별도로 저장
* 보통 버퍼의 구성 요소당 8비트
* 색상 값은 플로트를 사용하여 0.0(없음) ~ 1.0(모두) 범위 또는 서명되지 않은 바이트를 사용하는 경우 0 ~ 255 범위의 범위일 수 있음

## Indexed Color
* 색상은 RGB 값의 표에 대한 색인
* 더 적은 메모리 필요
  - 인덱스는 보통 8비트
  - 지금은 그렇게 중요하지 않다.
    -> 메모리 저렴
    -> 음영에 더 많은 색상이 필요

## Smooth Color
* 기본값은 부드러운 음영
  - OpenGL은 보이는 폴리곤에 걸쳐 정점 색상을 보간한다.
* 대안은 평면 음영이다.
  - 1차 정점 색상 
* 채우기 색 결정
  - 셰이더 처리
  
## Setting Colors
* 색상은 궁극적으로 파편 셰이더에 설정되지만 셰이더 또는 애플리케이션에서 결정 가능
* 애플리케이션 색상: 균일한 변수(다음 강의) 또는 정점 특성으로 정점 셰이더에 전달
*정점 셰이더 색상: 다양한 변수로 파편 셰이더에 전달(다음 강의)
* 파편 색상: 쉐이더 코드를 통해 변경 가능

