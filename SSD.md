# SSD (2016)
**Single Shot Multi Detector**   
Review.LSJ, 2023.08.08   
## Abstract   
* region proposal(영역 제안) 과정을 생략하는 1-stage 모델
* 1-stage 모델임에도 불구하고 2-stage 모델인 Faster R-CNN 능가하는 정확도
* Single Deep Neural Network(단일 깊은 신경망)을 사용
* 6개의 서로다른 scale의 Multi Feature Map과 각 Feature Map의 각 cell마다 서로 다른 scale과 aspect ratio를 가진 Default Box 사용   
  > Default Box : Faster R-CNN 모델의 Anchor Box와 유사하지만 다양한 해상도를 갖는 feature map 활용
  
  > Anchor box : 이미지에서 객체의 위치와 크기를 탐지하기 위해 사용되는 사전 정의된 박스
## Introduction   
* 기존의 모델들은 속도와 정확도 간 trade-off를 통해 하나만을 살림
* Faster R-CNN보다 빠르고 YOLO보다 정확
* VGG-16 모델을 통해 인풋 이미지를 처리하여 저해상도에서도 높은 정확도를 보임
  > VGG-16 : Visual Geometry Group에서 개발한 이미지 인식 모델로 16 개의 계층으로 이루어짐
* VGG-16 모델을 기반으로 한 early-stage(특성 추출 파트)에 auxiliary structure(객체 탐지 파트)가 추가된 구조
  
## The Single Shot Detector(SSD)

### Model
![image](https://github.com/sj990710/Thesis_Review/assets/127752372/70e17146-3356-480d-936a-ef4d812c268e)
* 사전 학습된 합성곱 신경망인 base network에 보조 네트워크,Extra Feature Layers(추가적인 합성곱 계층들)을 이어붙임

![image](https://github.com/sj990710/Thesis_Review/assets/127752372/29b14993-ea8e-4f9d-a47e-0dffa3c4b0b3)
* 각 feature map에서 3x3 conv filter를 통해 bounding box의 class와 offset 예측
  > feature size = 3x3x바운딩박스 개수x(class+offset)

### Multi-Scale Feature Maps For Detection
* 7x7 사이즈의 feature map만을 사용했던 YOLO 1과 달리 다양한 사이즈의 feature map을 통해 다양한 크기의 객채 인
* 38x38, 19x19, 10x10, 5x5, 3x3, 1x1 6종류의 사이즈
* 작은 feature map에서는 큰 객체, 큰 feature map에서는 작은 개체 인식

### Convolutional Predictors For Detection
* 각 feature map에서 3x3 filter를 사용해 객체 인식
* m x n x p의 feature map이 있으면, 3 x 3 x p개 convolutional filter는 default box 좌표에 해당하는 offset과 class score를 생성
### Default Boxes And Aspect Ratios  
![image](https://github.com/sj990710/Thesis_Review/assets/127752372/96ddccba-063c-49e7-a15c-c46455c92c76)
* 각 feature map의 cell에 3x3 크기의 (c+4) x k개의 filter 적용 => (c+4) x k x m x n 출력
  > 4:offset, c: class score, k: default box 수
## Training(PASCAL VOC 데이터 셋. class 수: 20개)
![image](https://github.com/sj990710/Thesis_Review/assets/127752372/2dc5d624-0f5d-4119-a692-cfcddf382b31)

### Matching strategy
* ground truth와 default box를 미리 매칭 시켜 IOU가 0.5를 넘는 bounding box만 loss 계산에 사용
### Training objective 
![image](https://github.com/sj990710/Thesis_Review/assets/127752372/78bbf2b0-a7eb-4246-98ee-c92d2195c184)

### Choosing scales and aspect ratios for default boxes 

### Hard negative mining 

## Experimental Results   

### PASCAL VOC2007   

### Model Analysis   

