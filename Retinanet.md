# **Focal Loss for Dense Object Detection**
**Retinanet**  
Review.LSJ, 2023.09.15
## **Abstract**  
* Focal loss 
  > cross entropy loss+(class에 따라 변하는 동적인)scaling factor의 형태  

  > easy exampled의 기여도를 자동으로 dpwn-weight하며, hard example에 대한 가중치를 높혀 학습 집중

## **Introduction**  
* Object Detection 모델은 IoU threshold에 따라 positive/negative sample로 구분 후 이를 활용해 학습  
  * 일반적으로 background의 수가 object보다 훨씬 많기에 **class imbalance** 야기
* Two-stage 계열의 모델(ex.RCNN)의 경우 region proposal 생성 후 classification  
  + 추가로 Online Hard Example Mining 통해 object와 background 비율 조정
* One-stage 계열의 모델(ex.YOLO)의 경우 region proposal 과정 없이 feature map에서 localization  
  + 전체 이미지를 순회하며 sampling 하기에 two-stage 모델에 비해 훨씬 많은 후보 영역 생성함-> class imbalance가 더 심각함  
* 이에 대한 해결책으로 **Loss Function** 제시

# **Main Ideas**  

##  **Focal Loss**  
* one-stage detector 모델에서 foreground와 background 사이에 발생하는 극단적인 class imbalance(ex.1:1000)을 해결하는데 사용  
* 이진 분류에서 사용되는 CE(Cross Entropy) loss function으로부터 비롯됨
  > **CE Loss**
  > ![image](https://github.com/sj990710/Thesis_Review/assets/127752372/a4b7818b-77c5-4e59-a70e-20045253c1e1)
  > * 문제점 : 모든 sample에 대한 예측 결과에 동등한 가중치 부여
  > * 결과 : 쉽게 분류될 수 있는 sample도 큰 loss 유발-> 학습 방해

  > **Balanced Cross Entrioy**  
  이러한 문제를 해결하기 위해 가중치 파라미터인 $α∈[0,1]$를 곱해준 Balnced Cross Entropy 등장
  > ![image](https://github.com/sj990710/Thesis_Review/assets/127752372/1689b901-beae-4186-8251-0179f82cb222)
  > * y = 1 일 때 $α$를, y=-1 일 때 $1-\alpha$를 곱해 positive/negative sample 간 균형 맞춤
  > * 하지만 easy/hard sample에 대해서는 균형을 잡지 못 함
  
  >  **Focal Loss**  
  > ![image](https://github.com/sj990710/Thesis_Review/assets/127752372/c4f4853d-dd02-4514-8e7e-20a51bcfbcfb)  
  > modulating factor $(1-p_t)^𝜸$와 tunable focusing parameter $𝜸$를 CE에 추가한 형태
  

* ![image](https://github.com/sj990710/Thesis_Review/assets/127752372/d2395857-483e-476f-9c54-e106318a5ec8)
* 

## **Retinanet**  
* **Feature Pyramid by ResNet+FPN**  
  * backbone network에 이미지를 입력하여 서로 다른 5개의 scale을 가진 feature pyramid 출력
  + Input : image  
   Process : feature extraction by ResNet+FPN  
   Output : feature pyramid(P3~p7)  
   > Backbone Network : 

   > Feature Pyramid 
* **Classification by Classification subnetwork**  
 * pyramid level 별 feature map을 Classification Subnetwork에 입력  
 * feature map의 각 cell마다 sigmoid activation function 적용  
 * feature map의 channel 수 : KxA (A : 9로 설정)
  > subnetwork의 구성 : 3x3(xC) conv layer - ReLU - 3x3(xKxA) conv layer
* **Bounding box regression by Bounding box regression subnetwork**  
  * pyramid level 별 feature map을 Bounding box regression subnetwork에 입력  
  * feature map이 anchor box 당 4개의 좌표(x,y,w,h)를 encode하도록 channel 수 조정  
  * feature map의 channel 수 : 4xA

## **Inference**  
* 속도 향상 위해 각 FPN의 pyramid level에서 가장 점수가 높은 1000개의 prediction 사용  
* subnetwork의 출력 결과에서 모든 level의 prediction 병합 후 Non maximum suppression(threshod = 0.5)를 통해 최종 prediction 산출  
![image](https://github.com/sj990710/Thesis_Review/assets/127752372/df3e83f3-11de-4f99-88f9-ddb5b40f8d3c)  

