# Deformable DETR
****   
Review.LSJ, 2023.12.01   
## **Abstract**   
* Deformable Convolution(이하 DCN)   
  > convolution filter의 kernel 위치 학습시켜 광범위한 receptive field 획득

  > 기준이 되는 픽셀에 대하 offest을 FC layer로 학습시켜 해당 offset에 대해서 conv 연산 수행   

  > 기준 픽셀과 연관되어 있는 offset 픽셀을 학습시켜 큰 object에는 큰 offset, 작은 object에는 작은 offset이 학습   

## **Introduction**  
* 기존 object detector들의 단점들 중 많은 수작업 구성 요소 사용을 보완하고자 DETR 제안   
* end-to-end object detector를 구축할 수 있었지만 기존의 detector에 비해 훨씬 느린 수렴과 작은 object에 대해 상대적으로 낮은 성능을 발휘한다는 단점 발생   
* 이를 보완하고자 Deformable Convolution 아이디어를 가져와 Attention 구조에 적용   
* key를 sampling point offset으로 사용해 k개로 sampling하여 attention module로도 많은 양의 image feature르 처리해 학습시간 감소   
* multi-scale feature map 사용하므로 작은 object에 대한 성능 상승   
## **Deformable DETR**  
![image](https://github.com/sj990710/Thesis_Review/assets/127752372/7d9f13b4-c92c-4c42-a94b-e0734e9dd51a)

* DCN에서 conv filter의 offset 학습 시킨 것을 deformable DETR에서는 encder 내의 attention의 입력이 되는 key를 offset으로 사용
  + 논문에서는 4개의 offset point으로 사용, key의 요소가 감소하므로 모델의 연산량 감소  
* Deformable Attention Module   
![image](https://github.com/sj990710/Thesis_Review/assets/127752372/8469a439-f553-43d2-b1b5-810bc4854ced)
   
입력 featuremap $x\in\Re^{C\times H\times W}$가 주어지면 q가 content fequre $z_q$와 기준점 $p_q$를 포함하는 query element를 인덱싱한다고 하면 Deformable attention feature는 다음과 같이 계산된다.   
   
![image](https://github.com/sj990710/Thesis_Review/assets/127752372/d995fc6a-8cbd-4b60-84b3-0f954877d87f)
![image](https://github.com/sj990710/Thesis_Review/assets/127752372/80d3d160-d3c3-4f2d-b973-bc0f2cd510f7)

  + Multiscale Deformable Attention Module
    각 attention head에서 1개의 sampling point에 focus를 뒀던 기존과 달리 multi-scale의 sampling point에 focus
    ![image](https://github.com/sj990710/Thesis_Review/assets/127752372/5f4fe385-3534-4e43-a79e-7af6a921d946)
    > m : attention head, l : feature level, k : sampling point
* Encoder
  ![image](https://github.com/sj990710/Thesis_Review/assets/127752372/2dc4dbd2-e991-41a0-9912-b6947581fbab)   
  기존 DETR과 달리 한 위치에서 sampling points들에 대해서만 attention 연산을 수행하며 multi-scale의 pixel에 대해서도 연산을 수행하므로 속도와 여러 크기의 object detect에 대해 발전
* Decoder
  ![image](https://github.com/sj990710/Thesis_Review/assets/127752372/9cd57459-c79e-464f-8ce0-db9171eb5c47)
  + self attention(기존과 동일)
   decoder의 input, object queries들을 multi-head attention
  + cross attention
   object queries들을 linear layer에 통과시켜 reference points 추출 후 detection head에서 relative offest 예측
* Experiments
  ![image](https://github.com/sj990710/Thesis_Review/assets/127752372/393e1b4a-d576-402c-80f0-fea1e857ef8d)   
  ![image](https://github.com/sj990710/Thesis_Review/assets/127752372/bdf215a1-4c10-4499-8381-bfc4ccfa14b2)
  ![image](https://github.com/sj990710/Thesis_Review/assets/127752372/ebcccad4-984b-449b-963b-ce85c7e994b2)


 
