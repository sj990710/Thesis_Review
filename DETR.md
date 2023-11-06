# **DETR (2020)**  
---
End-to-End Object Detection with Transformers  
Review.LSJ, 2023.11.05  
## **Abstract**  
* 최초로 Transformer를 object detection에 적용. transformer encoder-decoder architecture
* object detection을 direct set prediction problem으로 간주-> bipartite matching을 통해 유니크한 예측
* hand-designed component인 NMS, anchor generation 등을 제거해 파이프 라인 간소화  

## **Background Knowledge : Transformer**  
* **기존의 NLP 분야는 RNN을 자주 이용**  
  > 단점 1 : long-term dependecy problems, 앞에 있는 객체의 영향력 손실  
  > 단점 2 : RNN은 각 단어에 대한 전파력이 앞 방향으로만 있으므로 모든 단어들 간의 관계성을 파악하기 힘듦
  > -> 기존 RNN 모델들은 Attention 기법을 같이 엮어서 활용했음
* **Transformer는 RNN을 이용하지 않고 오직 Attention만을 이용해 NLP task 수행**
  * Attention이란 단어를 개별적으로 input 하는 것이 아닌 sentence 단위로 input하는 word embedding하는 것과 같음  
    > embedding이란?  
    > 단어를 벡터로 표현하는 방법. 밀집 표현으로 변환.
    > Q(Query) : Word matrix(단어 행렬), K(Key) : Similarity matrix(유사도 행렬), V(Value) : Weight matrix(가중치 행렬)
    > 


## **Introduction**  
* ### **기존 object detection은 pre-defined anchor 사용**  
  > - 이미지 내 고정된 지점마다 다양한 scale, aspect ratio 가진 anchor 생성  
  > - 해당 anchor를 기반으로 생성한 예측된 bounding boux와 ground truth match  
  > - IoU > threshold : positive, IoU <threshold : negeative. positive sample에 한해 bounding box regression 수행  
  > - threshold를 기준으로 독립적인 prediction을 수행하므로 하나의 groud truth에 다수의 bounding box가 매칭되는 many-to-one 관계 성립  
  
  > - 다수의 anchor 생성하므로 다양한 크기와 형태의 객체 효과적으로 포착 가능
  > - 하나의 ground truth를 예측하는 다수의 bounding box -> near-duplicate/redundant한 예측 존재
  > - 이를 제거하기 위해 NMS(Non Maximum Suppression)과 같은
* ### **DETR은 hand-crafted anchor 사용하지 않음**
  > - 하나의 ground truth에 오직 하나의 예측된 bounding box만 matching
  > - Ground truth를 예측하는 bounding box가 오직 하나인 one-to-one 관계이므로 post-processing 과정이 필요하지 않음
## **The DETR Model**
### **Object Detection Set Prediction Loss**

