# CV 응용

* * *
### Semantic Segmentation

- 픽셀마다 분류를 통해 이미지 구분
- 자율 주행에서 쓰임
- 예시)
<img width="650" alt="image" src="https://user-images.githubusercontent.com/93971443/193793478-7e78c673-46fa-40b7-9e6e-c05808783f79.png">

##### Fully Convolutional Network
- dense layer을 없애고 conv layer로만 이루어진 네트워크

<img width="750" alt="image" src="https://user-images.githubusercontent.com/93971443/193793993-42ff23d6-7f32-4a22-bda8-7e4426894b31.png">

- dense layer가 있던 없던 input/output의 파라미터 개수는 변함이 없음
- why?
  - input의 크기가 얼마나 크건 작건 동일한 conv 필터를 거침
  - 따라서, output의 size만 달라지고 바뀌는게 없음
  - classification net을 heat map으로 나타낼 수 있게 해줌

<img width="700" alt="image" src="https://user-images.githubusercontent.com/93971443/193794667-ccd2fb23-b8c0-4218-a3bd-fb4b55c0cd69.png">


##### Deconvolution

- conv의 역연산이라 하지만, 엄밀히 말하면 역연산은 아님
- 원상태로 복원을 못하기 때문
- 원래 데이터에 padding을 많이 줘서 원하는 size를 만듦

<img width="250" alt="image" src="https://user-images.githubusercontent.com/93971443/193795330-74b529f4-6861-4a78-aeca-dd502b2b3fd1.png">


* * *

### Detection

- R-CNN
- SPP-Net
  - network를 한번 돌려 얻어지는 conv-feature map에서 바운딩 박스에 해당되는 텐서를 가져옴
- Fast R-CNN
  - SPPNet과 유사, NN을 이용하여 결과 도출
- Faster R-CNN
  - Region Proposal Network + Fast R-CNN
  - Region Proposal Network란
    - 미리 size를 정해놓은 바운딩 박스(anchor box)에 물체가 있을지 없을지 확인
    - 9 * (4 + 2) 개의 채널이 나오는 conv feature map 
    - 54개를 하나씩 보고 어떤 바운딩 박스를 사용할지말지 결정
- YOLO
  - 여러 개의 바운딩 박스 예측과 classification을 동시에 진행 
  - 가장 빠른 방법
  - $S * S * (B * 5 + C)$ 의 크기를 갖는 텐서가 됨
  - 각 채널에 맞는 정보가 들어갈 수 있게 NN이 학습을 시켜줌


### ※ 위 내용 및 이미지는 네이버 부스트캠프 교육 자료를 참고하였습니다.
  
