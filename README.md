**<ASD 아동의 사회적 의사소통 강화를 위한 표정 인식 학습 코드 개발>**

1. 프로젝트 개요 및 목적

프로젝트 배경: 
얼굴 표정은 다양한 문화 속에서도 일관성을 가지기에 표정으로 감정을 해석하는 능력은 대인관계를 형성하는데 있어 핵심이라고 볼 수 있다. 그러나 자폐스펙트럼장애(autism spectrum disorder, ASD)를 가지고 있는 아동은 타인의 표정을 보고 감정과 마음을 이해하는 능력에서 결함을 보인다. ASD가 있는 아동은 얼굴 표정 인식에 장애가 있다는 것이 증명되었으며, 특히 분노와 슬픔, 행복을 구별하는데 있어 어려움이 있다고 학회에 보고되었다. ASD를 가지고 있는 아동은 정상적 아동에 비해 학습능력이 떨어지지만, 선행되었연 연구들에 의하면 훈련과 반복을 통해 표정 인식 학습이 가능하다. ASD아동이 사회의 일원이 되기 위한 첫 발걸음은 상대의 표정을 인식하는데에서 시작한다. 이러한 사실을 바탕으로, 우리는 ASD 아동을 위한 표정 인식 학습 모델을 개발하고자 한다.

목적: 
ASD 아동들이 사회적으로 더 잘 적응하고 소통하는데 필수적인 기술을 향상시킨다. 이는 ASD 아동들이 표정을 식별하고 그에 따른 감정을 이해하는 과정을 지원하여, 그들이 친구나 가족과 더 원활하게 상호작용할 수 있도록 도울 수 있게 한다.


2. 데이터 전처리 및 변환

데이터 불러오기:
“6 Emotions for image classification’디렉터리에서 가져온 6가지 클래스의 1200개의 이미지를 불러온다.
데이터 크롭:
‘dlib’ 라이브러리를 사용하여 이미지에서 얼굴을 탐지하고, 해당 영역을 크롭하여 얼굴 영역만 추출한다.

데이터 증강(Transformations):
모델 호환성을 위해 이미지 크기를 224x224로 조정하고, 데이터의 다양성을 증대시키기 위해 랜덤으로 좌우 반전, 얼굴 각도에 따른 다양한 상황을 구현하기 위해 30도 회전시킨다. 왜곡된 이미지 또한 학습하여 다양한 변화에 적용하기 위해 랜덤 어파인 변환을 시켜준다. 밝기, 대비, 채도 등의 변화를 잘 적용되도록 하기 위한 색상 변화와 실제 환경의 조명 조건에 따른 10% 그레이스케일 변환과 같은 데이터 확장 기법을 적용하여 모델의 일반화 능력을 향상시킨다. 이미지 텐서를 numpy 배열로 변환하고, 정규화를 수행하여 모델 학습에 적합한 형태로 데이터를 가공한다.

데이터로더 구축:
‘torch.utils.dat.DataLoader’를 사용하여 데이터를 미니배치 단위로 로드한다. 이를 통해 학습 과정에서 효율적으로 데이터를 처리할 수 있다. 전체 데이터셋은 train, val, test 용도로 6:2:2의 비율로 분할된다.		

전처리 전:

![image](https://github.com/user-attachments/assets/77955e1d-5334-4c5b-b612-7140c4123214)

전처리 후:

![image](https://github.com/user-attachments/assets/71f71b84-1b6b-4288-bf8f-0d4e1be351f0)

3. 모델 학습

CNN 모델 선정:
각 모델은 이미지 분류에서의 성능을 극대화하기 위해 사전 학습된 가중치를 사용하였으며, 모델의 최종 출력 계층을 수정하여 6개의 표정 클래스를 분류하도록 설정하였다. 모든 모델의 손실 함수는 Cross Entropy Loss, Optimizer는 Adam으로 고정하여 일관된 비교를 가능하게 하였다.
CNN 모델로는 resnet50, LeNet5, AlexNet, Vggnet, SEnet, EfficientNet, DenseNet, resnet152을 사용하였고, epoch를 30으로 설정하였을 때, resnet152, resnet50, Vggnet 순으로 높은 정확도를 보였다.

![image](https://github.com/user-attachments/assets/8e690fb8-7e3f-4231-aacd-d1fa5269e565)

Loss Function 선정:
Optimizer는 Adam으로 고정하였으며, 앞서 가장 높은 정확도를 보인 ResNet152 모델을 적용하였다.
Loss function으로는 CroosEntropyLoss, MSELoss, MultiLabelSoftMarginLoss, MultiMarginLoss을 사용하였고, epoch를 30으로 설정하였을 때, 각 모델의 Best val Acc는 0.7722, 0.6962, 0.7511, 0.7553으로, CroosEntropyLoss에서 가장 높은 정확도를 보였다.

![image](https://github.com/user-attachments/assets/5bd9aa84-4bfd-40ac-8ac8-d9f98a8bcd85)

Optimizer 선정:
각 Optimizer의 성능을 비교하기 위해 Adam, Adagrad, RMSprop, SGD, 그리고 Ranger를 사용하여 모델을 학습시켰다. 각 Optimizer는 같은 모델 아키텍처 및 학습 파라미터를 사용하였으며, 학습률은 모두 1e-5로 설정하였다.
epoch를 30으로 설정하였을 때, 각 Adam, Adagrad, RMSprop, SGD, Ranger 모델의 Best val Acc는 0.7511, 0.3333, 0.7553, 0.3671, 0.6624으로, RMSprop에서 가장 큰 정확도를 보였다.

![image](https://github.com/user-attachments/assets/99b6aef9-5703-4b57-af03-5f17e129957b)

최종 모델:
정확도(Accuracy)는 전체 예측 중에서 올바르게 예측된 비율을 나타내는 간단하고 직관적인 지표이다. 이에 표정 인식 학습 모델에서 전체 표정 데이터셋에 대한 정확한 예측이 가장 중요한 지표이기 때문에  정확도를 지표로 삼았다.
따라서 최종 모델은 가장 높은 정확도를 보여준 ResNet-152 아키텍처를 기반으로 하며, Loss Function으로는 Cross Entropy Loss, Optimizer로는 RMSprop를 사용하여 학습되었다. 



4. 성능 평가

Test 이미지 랜덤 10개 출력:
각 disgust, anger, fear, happy 클래스의 test 이미지 예측 결과에서 "happy"와 "fear" 다른 표정에 비해 확률의 차이가 뚜렷하여 쉽게 예측이 가능하다는 것을 알 수 있다. 특히, "happy"와 "fear"에 대한 예측 정확성은 모델이 이러한 표정 인식에서 다른 감정과의 혼동이 적음을 시사하며, 이를 섬세하게 구분하고 예측할 수 있는 능력을 갖추고 있음을 보여준다.

외부 이미지 출력:

![image](https://github.com/user-attachments/assets/fffee732-2eb5-475c-9a4e-3f8d4ca1f2a0)

"happy"와 "disgust"에 대한 정확한 예측이 이루어졌다. 그러나“anger”와 “sad” 예측에서는 외부 이미지와 모델의 예측 간에 불일치가 발생하였다. 모델이 "anger"를 예측했으나 이미지에서는 해당 감정이 뚜렷하게 드러나지 않았다. 또한, "sad"에 대한 잘못된 예측에서는 모델이 높은 확률로 해당 감정을 예측했으나 이미지에서는 다른 감정이 더 두드러지는 것으로 보인다.

5. 한계점 및 추후 보완사항

모델은 일부 외부 이미지에서 감정을 정확하게 예측하지 못했다. 특히, 특정 이미지에서는 감정을 잘못 예측하는 경향이 있다. “happy”에 대한 예측에서 정확한 예측을 하였지만, “anger”와 “sad”와 같은 다른 감정에 대한 예측은 정확하지 않았다.
이에 더 많은 데이터셋을 사용하여 모델을 학습시키고, 각 감정 클래스에 대해 충분한 샘플을 확보한다. 또한 모델에 사용된 데이터셋은 성인의 표정을 학습하는 데 사용되어 아동의 표정을 학습한 모델에 비해 성능이 떨어질 가능성이 있다. 이에 아동의 표정을 포함하는 추가적인 데이터셋을 수집하여 모델 학습에 활용한다. 
특히 "disgust"와 "anger"과 같이 미세한 차이로 인해 혼동될 수 있는 감정을 다루는 데 중점을 둔다. 인간이 감정을 인식할 때 시각, 청각, 맥락 등을 종합적으로 판단한다는 연구를 바탕으로, 사진의 표정과 목소리를 동시에 인식할 수 있는 모델을 개발한다. "Multimodal Emotion Recognition Using Deep Learning: A Survey" 연구에 따르면, 감정 인식을 개선하기 위한 다양한 음성, 텍스트, 얼굴 표정의 결합이 중요하다는 것을 강조한다. 이러한 모달리티를 통합하는 딥러닝 아키텍처를 사용하면 감정을 더 정확하게 학습시킬 수 있을 것으로 보인다.

6. 출처

· "주의력결핍과잉행동장애 아동과 자폐스펙트럼장애 아동에서 얼굴 표정 정서 인식과 구별의 차이." 소아청소년정신의학 27.3 (2016): 207-215.
· "이모티콘의 표현 유형과 시각적 표현 방식이 자폐스펙트럼 장애 (ASD) 아동· 청소년의 정서와 행동 변화 유도에 미치는 영향." Archives of Design Research-Vol 36.3 (2023): 149-165.
· "A review of affective computing: From unimodal analysis to multimodal fusion." Poria, S., Cambria, E., Bajpai, R., & Hussain, A. (2017): Information Fusion, 37, 98-125.
