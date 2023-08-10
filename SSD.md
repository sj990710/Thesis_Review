# SSD (2016)
**Single Shot Multi Detector**   
Review.LSJ, 2023.08.08   
## Abstract   
* Region proposal(영역 제안) 과정을 생략하는 1-stage 모델
* 1-stage 모델임에도 불구하고 2-stage 모델인 Faster R-CNN 능가하는 정확도
* Single Deep Neural Network(단일 깊은 신경망)을 사용
* 6개의 서로다른 scale의 Multi Feature Map과 각 Feature Map의 각 cell마다 서로 다른 scale과 aspect ratio를 가진 Default Box 사용   
  > Default Box : Faster R-CNN 모델의 Anchor Box와 유사하지만 다양한 해상도를 갖는 feature map 활용
  
  > Anchor box : 이미지에서 객체의 위치와 크기를 탐지하기 위해 사용되는 사전 정의된 박스
## Introduction   
* 기존의 모델들은 속도와 정확도 간 trade-off 를 통해 하나만을 살림
* Faster R-CNN보다 빠르고 YOLO보다 정확
* VGG-16 모델을 통해 인풋 이미지를 처리하여 저해상도에서도 높은 정확도를 보임
  > VGG-16 : Visual Geometry Group에서 개발한 이미지 인식 모델로 16 개의 계층으로 이루어짐
* VGG-16 모델을 기반으로 한 Early-Stage(특성 추출 파트)에 auxiliary structure(객체 탐지 파트)가 추가된 구조
## The Single Shot Detector(SSD)   

## Model
![image](https://github.com/sj990710/Thesis_Review/assets/127752372/70e17146-3356-480d-936a-ef4d812c268e)

## Training   

## Experimental Results   

## PASCAL VOC2007   

## Model Analysis   

