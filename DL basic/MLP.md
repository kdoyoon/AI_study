# Multi Layer Perceptron

* * *

### Neural Network

- 동물 뇌의 생물학적 뉴런 네트워크에서 착안된 컴퓨팅 시스템
- affine transformation(행렬 곱연산)과 비선형 transformation의 반복을 통한 function approximators


### Linear Neural Networks

<img width="600" alt="image" src="https://user-images.githubusercontent.com/93971443/193743241-f05d5732-39e7-4a7b-ae74-3954b8820e17.png">
- loss 값을 최소로 하기 위해서 최적의 w ,b를 구해야 함
- w, b에 대해 편미분 진행, gradient descent를 반복


### Multi Layer Perceptron

- neural network를 여러 층 쌓는 것을 말함
<img width="600" alt="image" src="https://user-images.githubusercontent.com/93971443/193743914-616632f9-0ecc-498f-9f16-4966e3892f84.png">

- 비선형 함수를 추가함으로써 표현할 수 있는 범위가 늘어난다.
- activation function(non-linear)
<img width="762" alt="image" src="https://user-images.githubusercontent.com/93971443/193744198-1a0fe650-40cf-429b-b4f0-d514a440304f.png">

#### Loss function
- 어떤 데이터를 가지고 어떤 결과를 얻고 싶은지에 따라 사용되는게 다름
<img width="600" alt="image" src="https://user-images.githubusercontent.com/93971443/193744905-f72af12d-1cce-439d-a630-e684dd545aaf.png">





### ※ 위 내용 및 이미지는 네이버 부스트캠프 교육 자료를 참고하였습니다.
