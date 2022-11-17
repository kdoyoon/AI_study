# 1 Stage Detectors

***

## YOLO
- **Y**ou **O**nly **L**ook **O**nce
- 하나의 이미지의 Bbox와 classification을 동시에 진행 -> 이미지 전체를 관찰하여 맥락적 이해가 높아짐
- region proposal 단계가 없음
<img width="680" alt="image" src="https://user-images.githubusercontent.com/93971443/202362792-bf2ed400-b346-4feb-9ee6-ae5256fc5fb8.png">

#### Network

<img width="693" alt="image" src="https://user-images.githubusercontent.com/93971443/202364602-54a473e8-df64-48bc-b960-c6f57997fef1.png">

- GoogLeNet을 변형하여 사용
- 24개의 conv layer에서 feature map을 추출
- 2개의 fc layer로 box의 좌표값 및 확률 계산

#### Pipeline

1) 입력 이미지를 S * S 그리드로 나눔(S = 7)
2) 각 그리드 영역마다 B개의 Bounding box와 Confidence Score 계산
  - 총 B box의 개수 : 7 * 7 * 2 = 98개
  - 신뢰도 : $Pr(obj) * IoU_{pred}^{truth}$

3) 각 그리드 영역마다 C개의 class에 대한 해당 클래스일 확률 계산
  - conditional class probability = $Pr(class_i | Object)$
   
* 그리드 별로 다음과 같은 과정을 진행
* 총 98개의 class score 도출

<img width="689" alt="image" src="https://user-images.githubusercontent.com/93971443/202364787-01ad110a-baae-4388-b35a-50bc88a9c286.png">

<img width="683" alt="image" src="https://user-images.githubusercontent.com/93971443/202364986-3a13b6a4-31fc-436e-ab07-4fc0062d49fc.png">

- class score들을 내림차순으로 정렬한 후 NMS 알고리즘을 적용하여 RoI들을 추려냄

#### Loss
- Loss는 크게 Localiztion, Confidence, Classification 세가지로 나뉨

<img width="670" alt="image" src="https://user-images.githubusercontent.com/93971443/202365542-57715927-23a9-4199-8b69-07574f1d3eb4.png">

* Localiztion loss
  * obj가 있을 때 중심점 위치, H, W에 대한 regression loss를 구함

* Confidence loss
  * obj가 있을 때, 없을 때의 경우로 나눠 loss 계산
  * obj가 없을 경우에는 람다를 이용하여 밸런스 조절을 함(-> 일반적으로 obj가 없는 cell이 많기 때문에 조절)

* classification loss
  * 확률에 대한 MSE 진행

#### 장점
* 빠른 속도
* 다른 real time detector보다 정확도가 좋음
* 이미지 전체를 보기때문에 클래스와 사진에 대한 맥락적 정보를 갖고있음
* 물체의 일반화된 표현을 학습

#### 단점
* 7 * 7 그리드 영역으로 나눔 -> 그리드보다 작은 물체 검출 불가
* 마지막 feature map만 사용 -> 정확도 하락 

* * *

## SSD

* YOLO와의 차이점
  * 300 $\times$ 300 이미지 사용(YOLO : 448)
  * 시간이 오래 걸리는 fc layer대신 1$\times$1 conv layer 사용
  * extra feature map에 대해서 모두 detection 진행
    - 큰 feature map : 작은 물체 탐지
    - 작은 feature map : 큰 물체 탐지
    - FPN과 유사
* 추가적인 특징
  * anchor box 사용

#### Network
- backbone : VGG16 + extra conv layer
<img width="611" alt="image" src="https://user-images.githubusercontent.com/93971443/202368411-fa37a134-6933-4cdf-8306-fec308a53019.png">

#### Multi-scale feature maps


