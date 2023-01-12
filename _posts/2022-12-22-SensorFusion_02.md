---
title: Lidar?
category: sensorfusion
layout: post
---

<br>
## Lidar란?
라이다는 빛을 감지하고 거리를 측정하는 센서입니다. beam 을 쏴서 돌아오는데 까지 걸린 시간을 측정하여 물체까지의 거리를 결정합니다. 레이저는 매우 짧은 파장을 쏴 돌아오는데 까지 걸리는 시간을 계산합니다. Laser 빔은 적외선 스펙트럼으로 다른 방향으로 쏩니다(일반적으로 360). 가장 잘 알려지고 사용되는 선구 제품은 Velodyne입니다.

<p align="center"><img src="/assets/img/sensorfusion/velodyne-hdl-64e.jpg" width="30%" height="30%"></p>

* 64 lasers/detectors
* 360 degree field of view
* 0.08 degree angular resolution(360 * 0.08 개의 포인트를 획득)
* 26.8 degree vertical field of view +2 up to -24.8 down\
  with 64 equally spaced angular subdivisions(approximately 0.4, 0.4*64 = 25.8)
* <2 cm distance accuracy
* 5-15 Hz field of view update
* \>1.3 M points per second

<br>
## Point Cloud Data?
(x1, y1, z1, I1)\
(x2, y2, z2, I2)\
(x3, y3, z3, I3)\
포인트 클라우드는 위와 같이 X좌표 Y좌표 Z좌표 그리고 Intensity I로 이루어진 점들의 구성으로 벨로다인은 1초에 256000(64\*(360/0.8)\*10)개의 PCD로 구성되어 있습니다. I는 신호의 세기로 reflective properties를 의미합니다.
<p><img src="/assets/img/sensorfusion/pcd-coordinates.jpg" width="30%" height="30%"></p>
이러한 좌표 시스템은 자동차의 local 좌표와 동일합니다. x축에 평행한 -24.8도 각도로 도착시간이 66.7ns 인 레이져가 있을 때 ( 빛의 속도 300000000m/s ) (66.7/2 * 300000000 / 10^-9 * cos(-24.8) , 0 , 66.7/2 * 300000000 / 10^-9 * sin(-24.8)) 좌표를 얻을 수 있습니다.
처리 툴로는 PCL(point cloud library), OpenCV를 자주 사용합니다.

<br>
### PCL Library?
Point Cloud Library\
장점
* Open Source tool
* 로보틱스 및 자율주행에서 널리 사용되어 tutorial이 많음
* Filtering, Segmentation, Clustering과 같은 내장함수
* Rendering 기능

<br>
### PCD의 문제점
Lidar는 Environmental Condition(환경의 영향을)을 많이 탄다. Lidar는 비가 많이 오거나, 눈이 많이 오는 WhiteOut, 모래 먼지가 많이 낀 BrownOut에서 잘 동작하지 않습니다. 또한, 빛을 반사하는 재질을 가진 물체들을 오탐지(Ghost Objects)하는 경우도 발생합니다.

<br>
### DownSampling
PCD는 많은 용량의 데이터를 포함하고 있으며 자동차 네트워크에서 데이터를 전송하는 것은 한정적입니다. 이를 해결하기 위해 Stixel(Object를 직사각형 막대기로 나타내는 방법)을 사용합니다.
<p><img src="/assets/img/sensorfusion/Stixel.jpg"></p>

<br>
### Filtering with PCL
PCL 라이브러리를 활용하여 PCD를 Downsampling 혹은 Filtering하는 알고리즘을 사용할 수 있습니다. 저희는 VoxelGrid Filter를 사용할 예정입니다. Voxel(3D Pixel) Grid Filter는 Cubic(3차원 공간)에 하나의 Point만 오도록 filtering하는 기법입니다. 






<br><br><br><br><br><br>

---
### 단어
Coherent : 일관된 \
Ranging : 거리 \
Contraption : 장치, 기계\
Rod : 막대, 매, 뭉터기 \
Bunch of : 무더기의 \
Steer : 조종하다, 움직이다 \
Exotic : 외적인 \
Emit : 뱉다, 방출하다\
Infrared Specturm : 적외선 \
Angular Resolution : 분해능 -> 서로 떨어져 있는 두 물체를 서로 구별할 수 있는 능력, 분해능이 작으면 가까워 보이는 두 물체도 서로 다른 물체로 인식하고, 분해능이 크다면 멀리 있는 물체도 하나의 물체로 인식할 수 \
1Hz : 헤르쯔, 1초에 \
Proprietary : 소유주의, 등록 \
Granularity : 세분성, 입도, 입자
