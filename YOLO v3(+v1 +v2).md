# YOLO v3(+v1 +v2)
**YOLO v3: an incremental improvement**   
Review.LSJ, 2023.08.25   
## Abstract 

> confidene(신뢰도 점수) : bbox가 객체를 포함하는지에 대해 신뢰성이 있는지, bbox가 얼마나 정확한지 반영

> Class probability  Pr(Class_i  |Object)  : 1. grid cell이 객체를 포함할 때 해당 개체가 i번째 클래스일 확률

> IoU : Intersection over Union

![image](https://github.com/sj990710/Thesis_Review/assets/127752372/4a0e4ffd-9d13-48f5-b4cd-d493294b0ac2)

## Introduction
**YOLO**
* 기존의 2-stage 모델의 느린 속도를 보완하기 위해 만들어진 1-stage 모델

* object detection을 classification이 아닌 regression으로 접근

## Unified Detection (통합탐지)

* 이미지를 통째로 넣음으로써 global(상황이나 주제의 모든 부분을 포함)한 특성을 반영하며 detection
![image](https://github.com/sj990710/Thesis_Review/assets/127752372/6309213a-8b1d-4ed4-84b0-dd7febf337aa)
* Input image를 S x S 칸의 grid cell로 분할

* 각 grid cell들은 Bbox와 각 box들에 대한 confidence 및 class probability 예측
*   Pr(Object) * IoU
