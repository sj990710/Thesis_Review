# YOLO v3(+v1 +v2)
**YOLO v3: an incremental improvement**   
Review.LSJ, 2023.08.25   
## Abstract 

> confidene(신뢰도 점수) : bbox가 객체를 포함하는지에 대해 신뢰성이 있는지, bbox가 얼마나 정확한지 반영

> Class probability  Pr(Class_i  |Object)  : 1. grid cell이 객체를 포함할 때 해당 개체가 i번째 클래스일 확률

> IoU : Intersection over Union

![image](https://github.com/sj990710/Thesis_Review/assets/127752372/4a0e4ffd-9d13-48f5-b4cd-d493294b0ac2)
# YOLO (You Only Look Once: unified real-time object detection)

## Introduction
* 기존의 2-stage 모델의 느린 속도를 보완하기 위해 만들어진 1-stage 모델

* object detection을 classification이 아닌 regression으로 접근

## Unified Detection (통합탐지)

* 이미지를 통째로 넣음으로써 global(상황이나 주제의 모든 부분을 포함)한 특성을 반영하며 detection
![image](https://github.com/sj990710/Thesis_Review/assets/127752372/6309213a-8b1d-4ed4-84b0-dd7febf337aa)
* Input image를 S x S 칸의 grid cell로 분할

* 각 grid cell들은 Bbox와 각 box들에 대한 confidence 및 class probability 예측
*  Confidence = Pr(Object) * IoU
*  각 grid cell 당 가장 큰 확률값을 갖는 하나의 클래스만 예측(YOLO는 20개의 class를 지원)
*  NMS(Non-maximal suppression)을 통해 detection 완료

## Network Design
*  24개의 convolutional layer(feature 추출)와 2개의 fully connected layer(예측)로 구성
*  중간중간 1 x 1 conolution layer를 사용 feature map의 demension 감소
*  output = 7 x 7 x 30 = (S x S x (5 * B + C))

## Training
*  주로 leaky ReLU 사용. 마지막 layer만 linear activation function 사용
*  SSE(Sum-squared Error)를 이용해 최적화
*  대부분의 grid cell은 confidence score가 0 -> 모델 불균형
*  λ_coord (= 5)와 λ_noobj (= 0.5)의 파라미터를 추가 -> bbox의 좌표 예측 손실 증가, 신뢰도 예측 손실 감소
*  Loss Function
 ![image](https://github.com/sj990710/Thesis_Review/assets/127752372/f353dec8-b17d-4c6d-a8e4-7d055b443693)

## Inference
*  이미지 한 장 98(7*7*2)개의 바운딩 박스 및 각 박스에 대한 클래스 확률 예측
*  하나의 객체를 여러 grid cell들이 detection 하는 경우 NMS을 통해 해결->mAP 2~3% 상승

## 한계점
*  1개의 grid cell이 1개의 객체만을 detection-> 공간적 제약 -> 새 떼와 같이 많은 수의 작은 객체들 detection 잘 못함
*  새로운 aspect ratio에 대한 탐지가 어려움
*  손실함수 SSE가 BB 크기와 관계 없이 가중치를 동일하게 취급한 문제를 해결하지 못함
