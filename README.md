# Detect-the-Birds

# Overview
<table>
  <tr>
    <td><img src = 'https://user-images.githubusercontent.com/77375223/125194573-df9f7400-e28c-11eb-9c67-37880ee88029.jpg' width = 100 height = 70></td>
    <td><img src = 'https://user-images.githubusercontent.com/77375223/125194574-e0d0a100-e28c-11eb-9bc5-485ee619bebe.jpg' width = 100 height = 70></td>
    <td><img src = 'https://user-images.githubusercontent.com/77375223/125194579-e4642800-e28c-11eb-95d5-eda349b57af6.jpg' width = 100 height = 70></td>
    <td><img src = 'https://user-images.githubusercontent.com/77375223/125194580-e4fcbe80-e28c-11eb-9a69-9c4f3a4231d4.jpg' width = 100 height = 70></td>
    <td><img src = 'https://user-images.githubusercontent.com/77375223/125194581-e4fcbe80-e28c-11eb-997c-5c360861097a.jpg' width = 100 height = 70></td>
  </tr>
  <tr>
    <td><img src = 'https://user-images.githubusercontent.com/77375223/125194582-e5955500-e28c-11eb-8746-cb06a983b37d.jpg' width = 100 height = 70></td>
    <td><img src = 'https://user-images.githubusercontent.com/77375223/125194583-e62deb80-e28c-11eb-8c8a-5978c2bb7a84.jpg' width = 100 height = 70></td>
    <td><img src = 'https://user-images.githubusercontent.com/77375223/125194584-e62deb80-e28c-11eb-8176-8bead7837636.jpg' width = 100 height = 70></td>
    <td><img src = 'https://user-images.githubusercontent.com/77375223/125194585-e6c68200-e28c-11eb-8d15-2f9e84485ddd.jpg' width = 100 height = 70></td>
    <td><img src = 'https://user-images.githubusercontent.com/77375223/125194586-e6c68200-e28c-11eb-9ea1-0da3791d9418.jpg' width = 100 height = 70></td>
  </tr>
  
  <tr>
    <td><img src = 'https://user-images.githubusercontent.com/77375223/125194587-e75f1880-e28c-11eb-92e7-1bb45079f99f.jpg' width = 100 height = 70></td>
    <td><img src = 'https://user-images.githubusercontent.com/77375223/125194588-e75f1880-e28c-11eb-9a41-4615301b5127.jpg' width = 100 height = 70></td>
    <td><img src = 'https://user-images.githubusercontent.com/77375223/125194589-e7f7af00-e28c-11eb-84e9-49b82eaceef9.jpg' width = 100 height = 70></td>
    <td><img src = 'https://user-images.githubusercontent.com/77375223/125194590-e7f7af00-e28c-11eb-8fd8-5280bcad8c4d.jpg' width = 100 height = 70></td>
    <td><img src = 'https://user-images.githubusercontent.com/77375223/125194591-e8904580-e28c-11eb-9f4e-3cf13dbdcefc.jpg' width = 100 height = 70></td>
  </tr>
</table>
<br>
총 15가지의 새를 구분하는 CNN모델을 직접 구성하여 kaggle의 리더보드 상위에 위치하는 것을 목표로 한다.

# Data Preprocessing
<img src = 'https://user-images.githubusercontent.com/77375223/125220971-778b7500-e302-11eb-97e6-3e4cc02ef529.png' width = 400 height = 350>

데이터에 맞는 정규화를 진행해주기위해 각각 채널에 대해 평균, 분산값을 구하여 Data normalization을 진행해주었습니다.

<img src = 'https://user-images.githubusercontent.com/77375223/125220970-765a4800-e302-11eb-8385-d4eaaab505be.png'>
<br>
transforms.Resize : input 이미지의 size를 통일해주기 위해서 적용하였습니다. (256x256) <br>
transforms.CenterCrop : 중앙에 학습에 필요한 정보(Bird image)가 더 많을 것으로 생각하여 적용해 주었습니다. <br>
transforms.RandomHorizontalFlip(0.5) : 50%확률로 Y축 방향으로 flip을 적용시켜주는 함수입니다. <br>
transforms.RandomRotation(10) : 랜덤으로 Image의 각도를 10도 만큼 변환해주는 함수입니다. <br>
transforms.Grayscale(0.1) : 10%확률로 흑백 이미지로 변환해주는 함수입니다. <br>
transforms.ToTensor : 데이터의 타입을 Tensor로 변경해주는 함수입니다. <br>
transforms.Normalize : 위에서 구한 각 채널의 평균, 분산을 통하여 normalizataion을 적용해주는 함수입니다. <br>

# model

### 1st model
<img src = 'https://user-images.githubusercontent.com/77375223/125222191-925ee900-e304-11eb-8a25-3b8d4140dca1.png'>
처음엔 간단한 모델로 성능을 측정해 보았습니다. 해당 모델로 파라미터를 튜닝했을 때 valiation의 경우 80%, leader board에서 70%의 성능을 이루어 냈습니다. <br>
해당 모델의 경우 파라미터를 다양하게 바꾸어 보았는데 계속 제자리 걸음이어서 좋은 feature map을 잘 추출해내지 못하는 것 같아 avg pooling을 사용하여 2번째 모델을 구성해 보았습니다.
하지만 그럼에도 성능의 개선이 없어 모델을 조금 더 깊게 구성해야 한다고 생각했습니다. 그래서 조금 더 딥한 모델을 구성했는데, cuda memory가 터지는 오류가 발생하였습니다.
그래서 1x1 convolution을 사용하여 채널수를 조정해 조금 더 깊은 모델을 아래와 같이 구성해 보았습니다.

### 2nd model
<img src = 'https://user-images.githubusercontent.com/77375223/125225075-96413a00-e309-11eb-8967-69bf878c69dd.JPG'>
해당 모델은 1x1 convolution을 사용하여 구성한 모델입니다. 채널수를 조정하여 파라미터 수를 줄이면서, 모델의 깊이는 더 깊어지도록 모델을 설정해보았습니다. 
해당 모델로 좋은 성능을 거둘 수 있었고, 최종적으로 84%의 성능을 거둘 수 있었습니다.

# conclusion
<img src = 'https://user-images.githubusercontent.com/77375223/125225817-db19a080-e30a-11eb-8ca1-f43e76f20631.JPG'>
