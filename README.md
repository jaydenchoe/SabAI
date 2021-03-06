# Team SabAI
Seoul Hackathon SabAI Team Repository

## 팀원 구성 및 역할

- 최재훈: 팀장, STM32 보드 기본 디바이스 enabling / 인공지능 추론+학습 구현 / 개발 내용 발표 정리

- 박세문: STM32 보드 AWS IoT 환경 구현, AWS IOT 웹서버/서버사이드렌더링/웹앱 서비스 구현 / 인공지능 학습데이터 제공

- 김병헌: User Interface/ User Experience / 시스템 시나리오 기획 / 인공지능 학습데이터 제공


## 프로젝트 제목
사회적 약자를 위한 초저가 인공지능 운동 도우미


## 프로젝트 배경 혹은 목적
뇌졸중, 당뇨, 고혈압은 우리가 쉽게 피할 수 없는 지병입니다. 병원에서는 초기 치료 이후 건강 관리로써 유산소 및 근력 운동을 권유합니다.
건강을 관리한다는 것은 자신의 신체 능력을 유지하고 향상시키는 것인데, 이미 우리는 운동 시간과 횟수를 기록하는게 쉽지 않다는 걸 알고있습니다.

SabAI팀은 모두가 함께 건강 기록을 위해 사용가능한 IoT 제품서비스를 제안합니다.
제품에서는 가속도센서 기반 패턴인식 머신러닝을 통해 운동 종류와 횟수를 분석합니다.
나아가, 분석 결과를 기록하고 관리할 수 있도록 서비스를 제공합니다.
SabAI는 특별한 관리를 받을 수 없어도 편리한 운동 기록을 통한 자기 관리가 가능한 미래를 꿈 꿉니다.

## 구성
본 프로젝트는 `aws_iot` 그리고 `stm32_infer`로 크게 나뉩니다. 각각 stm32보드에서의 AWS IoT 및 인공지능 추론을 포함합니다.

### `aws_iot`
> 정해진 프로토콜대로 UART 인풋을 읽어드립니다. 이후, 데이터를 MQTT 프로토콜을 이용, AWS IoT 서비스 환경에 전달합니다.

### `stm32_infer`
> 학습 데이터 수집 및 인공지능 모델 추론 코드를 모두 포함합니다.
> 학습 데이터 수집에는 UART, GPIO, SPI 기능을 사용해 내장 가속도 센서 데이터를 획득 및 데이터를 수집하는 경로(UART)로 전달합니다.
> 
> 인공지능 모델 추론에는 UART, GPIO, SPI 기능을 사용해 내장 가속도 센서 데이터를 획득 및 데이터를 이용해 인공지능 모델을 통한 추론을 진행합니다.
> 추론 결과는 UART를 통해 `aws_iot` 역할을 수행하는 다른 보드로 전달합니다.

## 구현사항

GPIO / UART / SPI 를 이용하여 프로젝트를 진행했습니다.

### 사용 클라우드
* AWS IoT (데이터 provisioning)
* vercel (웹서비스 deploy)

### 기타 사항
* 카메라 기능 사용하지 않음.

* 이미지 분석 기능 사용하지 않음.
(인공지능 모델 상 Convolution 네트워크는 이용함)

## 코드 기여자

* aws_iot/STM32CubeExpansion_Cloud_AWS_V2.0.0/Projects/B-L4S5I-IOT01A/Applications/Cloud/aws_demos/Src/st_sensordata_collector.c 박세문
* aws_iot/STM32CubeExpansion_Cloud_AWS_V2.0.0/Projects/B-L4S5I-IOT01A/Applications/Cloud/aws_demos/Inc/aws_customdemo_globals.h 박세문
* aws_iot/STM32CubeExpansion_Cloud_AWS_V2.0.0/Projects/B-L4S5I-IOT01A/Applications/Cloud/aws_demos/Src/st_sensordata_collector.c 박세문
* aws_iot/STM32CubeExpansion_Cloud_AWS_V2.0.0/Middlewares/Third_Party/amazon-freertos/demos/include/aws_clientcredential.h 박세문
* aws_iot/STM32CubeExpansion_Cloud_AWS_V2.0.0/Projects/B-L4S5I-IOT01A/Applications/Cloud/aws_demos/Inc/main.h 박세문
* stm32_infer/Src/main.c 최재훈
* stm32_infer/Src/gesture_predictor.c 최재훈
* stm32_infer/Src/axis_sensor.c 최재훈
* stm32_infer/Src/peripherals.c 최재훈
* stm32_infer/Src/stm32l4xx_hal_msp.c 최재훈
* stm32_infer/Src/stm32l4xx_it.c 최재훈

## 구현 영상  및 참가영상 
 
- 개발 내용 요약 자료 (PPT)

· Team SabAI 개발솔루션 설명 영상 / 발표 영상 / 시연 영상 : https://www.youtube.com/channel/UCpIxpUlECa4bGSRHpx8D43w

· 발표자료: https://drive.google.com/drive/folders/11SJwUSGQbpouVL-liDn9XfmvkwitSEFn?usp=sharing


#### STM32 Inference(모션 데이터 센싱과 인공지능 추론) Board README
* [stm32_infer/README.md](https://github.com/jaydenchoe/SabAI/blob/main/stm32_infer/README.md)

#### STM32 AWS IOT Board README
* [aws_iot/README.md](https://github.com/jaydenchoe/SabAI/blob/main/aws_iot/README.md)


## 부록 (인공지능 학습 데이터와 학습 관련 프레임워크 설명)

## Application Architecture
- x, y, z 3 axis accelerometer sensor 값을 사용합니다. 전처리 없이 그냥 x, y, z 의 값을 바로 때려 넣습니다. 나중에는 calibration이나 normalization windowing 작업 필요할 수 있어요.

## Model Overview
- x, y, z 3개 1 set일 때 128 set 데이터를 입력 받습니다 이는 즉 5.12초 정도의 데이터 양입니다. ==> 향후에 데이터 주실 때 동일하게 128 set로 끊어서 만들어 주셔야 합니다!!
- output node가 4개입니다: 즉 4가지 모션을 처리하게 됩니다.

## (중요) SabAI에서 취할 모션 제스처 종류 (참고자료: 뇌졸중 재활 환자를 위한 앉아서 하는 자가 재활운동 by 재활의료기관 서울재활병원)
0) 반드시 영상 먼저 대충 보셔야 합니다: https://www.youtube.com/watch?v=fgA63sgDft4
1) 어깨 으쓱하며 숨쉬기
2) 양손 위로 들기
3) 가슴 펴기
4) No gesture


# 데이터 학습 방법 (정리중)
1) SabAI 제스처 데이터 학습 프레임워크(Colab) : https://colab.research.google.com/drive/1DqGlu8Nrcyt-fy0vpPPRo6iL97KjmdLT#scrollTo=LG6ErX5FRIaV
2) 학습에 필요한 각 모션 데이터들은 GDrive 폴더에 모으고, 이것을 Mount해서 Colab에서 당겨 오는 식으로 구현했습니다.


