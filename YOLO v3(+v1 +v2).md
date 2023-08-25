# YOLO v3(+v1 +v2)
**YOLO v3: an incremental improvement**   
Review.LSJ, 2023.08.25   
## Abstract 
YOLO (You Only Look Once)
> object detection을 classification이 아닌 regression으로 접근
> confidene(신뢰도 점수) : bbox가 객체를 포함하는지에 대해 신뢰성이 있는지, bbox가 얼마나 정확한지 반영
> Class probability  Pr(Class¬_i  |Object)  : 1. grid cell이 객체를 포함할 때 해당 개체가 i번째 클래스일 확률
> IoU : Intersection over Union
![image](https://github.com/sj990710/Thesis_Review/assets/127752372/4a0e4ffd-9d13-48f5-b4cd-d493294b0ac2)
