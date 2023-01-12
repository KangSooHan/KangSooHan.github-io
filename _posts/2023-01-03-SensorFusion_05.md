---
title: Point Cloud Segmentation
category: sensorfusion
layout: post
---

## Point Cloud Segmentation

<br>
### Segmentation
이미지 분야에서 segmentation은 이미지 내에 있는 객체들을 객체 단위로 분할하는 작업을 의미합니다.
<p align="center"><img src="/assets/img/sensorfusion/Segmentation.jpg"></p>
Point Cloud Segmentation은 마찬가지로 3D Point cloud 내에 있는 점들에서 객체들을 객체 단위로 분할하는 작업을 의미합니다.
<p align="center"><img src="/assets/img/sensorfusion/pointcloud_segmentation.jpg"></p>
자율주행 자동차에서는 도로에 있는 obstacles을 탐지하는데 사용되고 있습니다. Segmentation을 진행하기 위해서 Plane 과 Obstacle을 분리해야 합니다. 우리는 Plane을 RANSAC알고리즘을 통해 추출할 수 있습니다.

<br>
### RANSAC
RAndom Sample Consensus 알고리즘은 여러 point중에서 inlier를 가장 많이 가지는 모델을 찾는 알고리즘입니다.
* Iterative 방법
* 각 반복에서 points의 subset(집합)을 랜덤하게 고름
* 모델에 point를 적용
* 가장 많은 inliers를 가지는 모델을 선택
<p align="center"><img src="/assets/img/sensorfusion/RANSAC_LINE.gif"></p>

<br>
### RANSAC for Line
RANSAC을 Lines에서 적용하는 방법에 대해서 소개합니다.
* 각각 반복에서 2개의 점을 선택
* 두 개의 점을 지나는 선을 그림
* 그린 선과 모든 점들의 거리를 계산하고 distance tolerance 이내에 있는 점들은 inlier

1. 두 점을 선택
2. 선의 방정식을 구함
3. 나머지 점들과 선의 거리들을 계산
4. 거리들을 모아서 크기가 가장 작은 선을 선택
<p><img src="/assets/img/sensorfusion/RANSAC_LINE.jpg" width="50%" height="50%"></p>

<br>
### RANSAC for Plane
<p><img src="/assets/img/sensorfusion/RANSAC_PLANE.jpg" width="50%" height="50%"></p>

<br>
### RANSAC Simulation Results
 in Line
<p><img src="/assets/img/sensorfusion/RANSAC_RESULT.JPG" width="50%" height="50%"></p>
 in Plane
<p><img src="/assets/img/sensorfusion/Ransac3D.jpg" width="50%" height="50%"></p>






<br><br><br><br><br><br>

---
### 단어
Fairly : 꽤 \
Straightforward : 직설적인, 간단한, 복잡하지 않은 \
Segmentation : 분할 \
Associate : 연관짓다 \
Parentheses : 괄호 \
Elicit : 끌어내다 \
Indices : index의 복수, 색인 지수 \
Stub : 몽땅, 꽁초, 남는 부분 \
Declaration : 선언 \
Handy : 편리하게, 편리한 \
Encourage : 격려하다 \
Film Up : 촬영하다 \
Consensus : 의견일치, 합의 \
Prep : 준비하다 \
flavor : 맛, 맛이 나다, 풍미를 더하다 \
Prependicular : 전치(전치 몇주 할 때) \
Perpendicular : 수직의, 수직적인, 수직 \
Trigonometry : 삼각법 \
Numerator : 분자 \
Denominator : 분모 \
Magnitude : 크기, 규모 \
Hill : 언덕 \
in terms of : ~측면에서, ~면에서 \

