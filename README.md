데이터 소개서

1. 데이터셋 개요
데이터셋 이름: 6 Human Emotions for image classification
데이터 출처:
[Kaggle
dataset](https://www.kaggle.com/datasets/yousefmohamed20/sentiment-images-classifier)
데이터셋 설명: 이 데이터셋은 인간의 감정을 나타내는 6개의 카테고리(anger, disgust,
fear, happy, pain, sad)에 대한 이미지로 구성되어 있다. 각 감정 카테고리별로 약
200개의 이미지를 포함하고 있다.

2. 데이터셋 구조
총 데이터 수: 1200개
파일 형식: JPEG or PNG 이미지 파일
3. 데이터 수집 및 처리 방법
수집 방법: Kaggle에서 제공된 데이터셋을 다운로드하여 사용
처리 방법:
● 압축 해제: 제공된 ZIP 파일을 Colab 환경에서 압축 해제
● 데이터 분할: ‘splitfolders’ 라이브러리를 사용하여 데이터셋을 학습(train),
검증(val), 테스트(test) 세트로 분할
● 얼굴 검출 및 전처리: ‘dlib’ 라이브러리를 사용하여 얼굴을 검출하고
크롭(crop)하여 모델에 적합한 형태로 변환
● 데이터 증강: 랜덤 회전, 좌우 반전, 색상 변화 등을 통해 학습 데이터 증강
4. (추가) 테스트 - 외부 이미지 데이터 출처
1. 아기(Sad) : https://www.segye.com/newsView/20160521000377
2. 유재석(Anger) : [무한도전 332회(13.06.01)]
https://youtu.be/DdXAZFgkBMQ?si=DLASGWIUFDkgOXhA 해당 영상 내 캡쳐
3. 여자(Disgust) :
https://www.news18.com/news/buzz/beyond-gross-woman-disgusted-after-finding-ou
t-her-partner-eats-earwax-and-snot-5642689.html
4. 여자(happy) :
https://www.kitchenerdentistsherwood.com/gum-boils-what-you-need-to-know-how-t
o-prevent-them/
