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
<img width="691" alt="image" src="https://user-images.githubusercontent.com/93971443/202368411-fa37a134-6933-4cdf-8306-fec308a53019.png">

#### Multi-scale feature maps
- 각 feature map에 대해 여러 scale의 anchor box를 생성
- anchor box의 개수 구하기는 다음 과정을 통해 진행

<img width="832" alt="image" src="https://user-images.githubusercontent.com/93971443/202455914-27a3bb61-a4b6-430e-81e1-d26a08b26d4e.png">

- 6개의 box 좌표와 각 class score로 이루어짐
<img width="741" alt="image" src="https://user-images.githubusercontent.com/93971443/202458656-77486e82-97a9-4d91-b677-baed795f2881.png">

- 총 8732개의 anchor box 가짐
<img width="622" alt="image" src="https://user-images.githubusercontent.com/93971443/202459099-19df5c04-9059-4f72-bd68-2bbbc7f7ee8d.png">

#### Training
- Hard Negative mining
- Non maximum suppression 진행
##### Loss
<img width="723" alt="image" src="https://user-images.githubusercontent.com/93971443/202459393-10754087-b187-4efe-a1be-6380af6dc0b8.png">


* * *

## YOLO V2

- Better(정확도), Faster(속도), Stronger(더 많은 class 예측) 3가지 파트에서 model 향상을 꾀함

#### Better
- Batch Norm 적용
- High resolution classifier 사용
  - v1에선 224$\times$224 이미지로 사전 학습된 VGG를 448$\times$448 detection task 진행
  - v2에선 448$\times$448 이미지로 새로 fine-tuning 진행
- Conv with anchor boxes
  - fc layer 제거
  - anchor box 도입
  - K-means clusters on COCO datasets
  - 좌표 값 대신 offset 예측하는 문제가 단순하고 학습하기 쉬움
- Fine-grained features
  - FPN 구조 사용 (low level/high level 섞어줌)
- MUlti scale training

#### Faster
- backbone 모델 GoogLeNet->Darknet-19로 변경
  - darknet for detection
    - 마지막 fc layer 제거 -> 3$\times$3 conv layer로 대체
    - 1$\times$1 conv layer 추가

#### Stronger
- Classification 데이터셋(image net), detection 데이터셋(COCO dataset) 함께 사용
- Image net data : COCO data = 4 : 1로 구성
- 계층적인 트리구조(Word Tree 구성)

* * * 
## YOLO V3
- Darknet-53사용
  - skip connection 적용
  - conv stride 2 사용
  - max pooling 사용X

- Multi-scale Feature maps(=FPN 사용)
  - 서로 다른 3개의 scale 사용
  - FPN 사용   




### ※ 위 내용 및 이미지는 네이버 부스트캠프 교육 자료를 참고하였습니다.

<img width="337" alt="image" src="https://user-images.githubusercontent.com/93971443/202462647-bd98f4a6-c9e2-4d0c-91c9-6b00ba080df6.png">

