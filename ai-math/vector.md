# VECTOR

* * *
## 정의 

* 숫자를 원소로 가지는 리스트 또는 배열
* n차원 공간 상의 한 점이자 원점으로 부터 상대적 위치 표현

* * *

## Vector 연산
* 같은 크기의 벡터끼리 +, - 성분곱(Hadamard Product) 연산 가능
* 벡터의 덧셈, 뺄셈 : 상대적 위치 이동을 나타냄
> <img width="200" alt="image" src="https://user-images.githubusercontent.com/93971443/191453891-23b54887-30dd-49a2-8895-9f5ad7d60e25.png">

* * *

## 노름(norm)
* 정의 : 원점에서 부터의 거리
* L1-norm, L2-norm이 있다.
  - L1-norm : 성분 변화량 절댓값의 합
  > ![image](https://user-images.githubusercontent.com/93971443/191454400-b2e0ac4d-a209-47b2-adc3-f13864b3e143.png)
  > <img width="200" alt="image" src="https://user-images.githubusercontent.com/93971443/191454768-f2fb0298-8a4e-4260-8a36-870984d34f3b.png">

  - L2-norm : 유클리드 거리 계산
  > <img width="200" alt="image" src="https://user-images.githubusercontent.com/93971443/191454500-d0d3603f-c172-4055-a53b-271a047f7b0e.png">
  > <img width="200" alt="image" src="https://user-images.githubusercontent.com/93971443/191454787-d3e8ef93-4422-4f80-9382-2a3f030b961e.png">
* norm에 따라 기하하적 성질에 차이가 발생하여 머신 러닝 목적에 맞게 사용
 (ex. L1 : 로버스트 회귀, 라쏘 회귀에서 사용 / ㅣL2 : 라플라스 근사, 릿지 회귀)

* * *

## 두 벡터 사이의 각도
* 제 2 코사인 법칙 사용(L2-norm 활용)
<img width="365" alt="image" src="https://user-images.githubusercontent.com/93971443/191457605-f15db710-6b97-4a53-9dc8-c74d2bdfda32.png">

* 더 쉬운 계산을 위한 내적 사용
  - 내적 : 정사영된 벡터의 길이
    * 정사영의 길이는 ||X||cosΘ이고, 이를 ||y||만큼 조정하면 내적을 구할 수 있다.

