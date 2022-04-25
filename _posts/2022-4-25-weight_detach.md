---
layout: single
title: weight 확인
categories: 딥러닝, 딥러닝에러
tag: numpy, weight, gpu
typora-root-url: ../
---

학습중 각각의 layer의 현재 weight를 확인을 해 보아야 하는 상황이 생긴다.

상황을 가정해 보자
```python
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
layer = nn.Conv2d(in_channels = 1, out_channels = 3, kernel_size = 3, stride = 1).to(device)
print(layer)
```

#Conv2d(1, 3, kernel_size=(3, 3), stride=(1, 1))

이러한 경우 layer의 weight를 보려고 
`print(layer.weight)`
`print(layer.weight.shape)`
을 할 수 있다. 하지만 이것을 numpy형태로 변형하고자 
`layer.weight.numpy()` 를 하면 다음과 같은 에러가 발생한다.

TypeError: can't convert cuda:0 device type tensor to numpy. Use Tensor.cpu() to copy the tensor to host memory first.

왜냐하면 현재 layer에서의 weight 는 학습이 가능한 상태이기 때문에 업데이트될 수 있어서 바로 뽑아낼 수가 없도록 되어있다. 이때 .detach()를 통해 gradinet의 영향을 받지 않도록 한다.
`weight = layer.weight.detach().numpy()`

하지만 여기서 새로운 에러가 발생하는데
TypeError: can't convert cuda:0 device type tensor to numpy. Use Tensor.cpu() to copy the tensor to host memory first.

이는 GPU에 할당되어 있는 텐서를 numpy배열로 변환하면서 발생한 에러이다. GPU에서 CPU로 저장한 후 numpy로 변환 해주면 해결 가능하다.
`weight = layer.weight.detach().cpu().numpy()`

# 결론
1. pytorch에서 weight를 바로 numpy로 변형하는것은 막혀있다.
2. 이를 위해선 detach()를 해주어야한다.
3. gpu에 할당된 배열을 numpy로 바로 변환이 안된다.
4. 이를 위해선 .cpu()를 통해 cpu에 저장후 .numpy()를 해 주어야한다.
`layer.weight.detach().cpu().numpy()` 