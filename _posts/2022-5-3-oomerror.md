---
layout: single
title: 에러해결 - OOM
categories: 딥러닝, 딥러닝에러
tag: numpy, weight, gpu
typora-root-url: ../
---
ResourceExhaustedError: OOM when allocating tensor with shape[11772117,100] and type float on /job:localhost/replica:0/task:0/device:GPU:0 by allocator GPU_0_bfc [Op:RandomUniform]


![OOM](/images/2022-5-3-oomerror/OOM.JPG)


모델 훈련을 할 때 GPU하드웨어 부족으로 인해 발생하는 OOM Error는 여러 문제 원인이 있다.

그 원인으로 

첫번째, 말 그대로 GPU reosurce가 부족한 것인데 이를 해결하기 위한 근본적이고 물리적인 방법은 GPU memory를 늘리는 것이다.

하지만 이러한 답은 우리에게 바로 해결가능한 문제는 아니다.



두번째, 이미지 크기를 줄인다.

하지만 저해상도의 이미지를 통해 학습을 하기때문에이러한 경우 성능 하락을 야기할 수 있다. 


세번째, batch size를 줄인다.

가장 현실적인 문제해결방법으로 해당방법을 추천한다. 