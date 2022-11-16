# NECK

* * *

<img width="800" alt="image" src="https://user-images.githubusercontent.com/93971443/202096000-10a2411c-6b84-423b-af1c-adaeacbf14e5.png">

- backbone 마지막 layer의 feature map만 쓸 경우 이미지의 모든 정보를 파악하는데에 어려움이 있음
- low level : 작은 B box / location 정보가 많음
- high level : 큰 B box / segmentic 정보가 많음
- 이 둘의 정보를 잘 섞어보자! -> NECK이 그 역할을 함
   
* 즉, 다양한 크기의 객체를 더 잘 탐지하기위해, low/high level의 정보를 교환하기 위해 사용

* * * 

## FPN(feature pyramid network)

- high -> low로 semantic 정보 전달하기 위해 top-down path way 추가
- 피라미드 구조를 통해 다음과 같이 high 정보를 low로 전달
<img width="594" alt="image" src="https://user-images.githubusercontent.com/93971443/202097512-018629ab-a561-458a-928d-c5e212f77215.png">

- 이때 lateral connections로 연결되어있음
  - lateral connection
    - target과 top-down/bottom-up에서 오는 input들의 shape이 다름
    - __bottom-up : 1X1 conv layer를 통과하여 channel 증가__
    
    <img width="427" alt="image" src="https://user-images.githubusercontent.com/93971443/202098066-e43ec28f-294b-4a71-8e03-4df958e2c82e.png">
    
    - __top-down : upsampling__
    
    <img width="434" alt="image" src="https://user-images.githubusercontent.com/93971443/202098358-299ed3b6-463c-4b37-9881-ab748b8812d3.png">

- 전체 pipe line
<img width="513" alt="image" src="https://user-images.githubusercontent.com/93971443/202098613-b60e0858-bca7-48b6-8072-6bc5eb383221.png">

- RoI projection시 대상이 되는 feature map이 필요함
- 그러나 FPN에선 굉장히 많은 RoI가 존재하여 mapping이 필요
- 다음 식을 통해 어느 input에서 나왔는지 파악할 수 있음

<img width="468" alt="image" src="https://user-images.githubusercontent.com/93971443/202099922-567b4530-d93a-4e52-ac9c-595631c844fc.png">

### Summary
- Bottom-up에서 다양한 크기의 feature map 추출
- 다양한 크기의 feature map의 semantic을 교환하기 위해 top-down 방식 사용

### 문제점
- backbone 모델의 굉장히 많은 layer을 거치다 보니 information loss 발생


* * *

## PANet 

- FPN의 문제 해결을 위함
- Bottom-up path 추가
- 경계에 있는 RoI에 대한 대응이 힘듦 -> 모든 stage의 feature map에 RoI projection을 진행

<img width="693" alt="image" src="https://user-images.githubusercontent.com/93971443/202101175-62c0a796-b26a-4b0b-be14-6bee0c595f1e.png">

* * *

## DetectoRS

- 주요구성
  - Recursive Feature Pyramid(RFP)
  - Switchable Atrous Convolution(SAC)
 
### RFP
- 하나의 FPN을 하나의 step으로 진행, 이를 N번 반복

<img width="421" alt="image" src="https://user-images.githubusercontent.com/93971443/202101746-dc3f6840-c1b7-496e-a4fe-338899d78d0a.png">

![image](https://user-images.githubusercontent.com/93971443/202102127-5328c303-3216-42f4-8024-294ab7a09b70.png)

- 1st iter : 

![image](https://user-images.githubusercontent.com/93971443/202102250-4179dd1c-22fd-46c5-981e-d7cfe53ab199.png)

- N iter :

![image](https://user-images.githubusercontent.com/93971443/202102417-90a8519e-061f-4b1c-b846-3a604319e9e0.png)

### SAC

<img width="617" alt="image" src="https://user-images.githubusercontent.com/93971443/202102606-65d0d261-773c-413c-8869-8674f16f9278.png">

- 하나의 feature map에서 pooling을 진행하는데 rate를 키워 receptive field의 크기를 늘림
- concat 후 사용
- receptive field의 영역을 늘려줌

<img width="647" alt="image" src="https://user-images.githubusercontent.com/93971443/202103266-ef8a0e3c-d85a-47df-b691-45a18c3d86e6.png">

* * *
## BiFPN


