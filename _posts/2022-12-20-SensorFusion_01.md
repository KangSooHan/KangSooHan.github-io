---
title: 센서 퓨전이란?
category: ss
layout: post
---

### 센서 퓨전은 왜 필요한가?
로봇은 환경을 이해하고 만들기 위해서 다양한 종류의 센서를 씁니다. 센서는 각각 자신의 장단점을 가지고 있습니다. 센서 퓨전은 이런 센서 정보들을 종합하여 더 나은 환경을 제공하게 해줍니다.


### 뭘 배울까요??
우리는 Lidar, Camera, Rader, kalman Filters에 대해 학습합니다.


### Lidar란?
Light + Rader의 합성어로 레이저 빔을 쏴서 물체에 반사되어 돌아오는 빔을 측정하는 센서입니다.
* 장점
    * 거리를 측정하는데 용이
    * Point Cloud Data(XYZ)형태를 제공
* 단점 
    * 비싼 가격


### Camera란?
Camera는 빛에서 이미지를 생성하는 Passive Sensor입니다.
* 장점
    * 높은 Resolution.(신호등 같은 정보를 파악하기 용이함)
    * 가격이 적당
* 단점
    * 거리 측정 불가.

### Rader란?
Rader는 빛 파장을 쏴 물체에 반사되어 돌아오는 신호를 측정하는 Active Sensor입니다. Signal Processing을 Rader 파장 형태에 사용해서 Position과 Velocity를 획득할 수 있습니다.
* 장점
    * 다양한 유형의 환경에서 높은 신뢰성과 기능을 제공
    * 가격이 쌈

* 단점
    * 낮은 해상도

### Fusion?
다른 Sensor 데이터를 하나로 합치는 방법을 말합니다. 수학적인 필터를 사용해서 서로 다른 타입의 데이터를 하나로 합쳐 불확신성을 줄이고 정확도를 늘립니다. Prediction -> Update과정을 반복하며 정확한 환경을 만듭니다.

<br><br><br><br><br><br>

---
### 단어
Passive Sensor(수동센서) : 센서가 만든 신호가 아닌 다른(태양) 신호로 측정하는 센서(Camera etc..)  
Active Sensor(능동센서) : 센서가 만든 신호로 측정하는 센서(Lidar, Rader etc..)  
Modality : 양식  
Cohesive : 결합력있는  
Portmanteau : 합성어  
Unscented 무향 


