---
title: Point Cloud Clustering
category: sensorfusion
layout: post
use_math: true
---

 Point Cloud Clustering
Point Cloud에서 Clustering은 보행자, 자동차 등 obstacle들을 찾아내는데 사용합니다. Udacity 강의에서는 KD-TREE를 사용한 Euclidean Clustering을 사용합니다.

## Clustering
클러스터링(Clustering : 군집화)는 비슷한 개체끼리 한 그룹으로, 다른 개체는 다른 그룹으로 묶는 것을 의미합니다. 클러스터링은 다음과 같이 얘기할 수 있습니다.
* 군집 간 분산(outer-cluster variance) 최대화
* 군집 내 분산(inner-cluster variance) 최소화

클러스터링은 정답 라벨이 없는 Unsupervised Learning(비지도 학습) 입니다. 이는 그룹 정보 없이 비슷한 개체끼리 군집화 하는 것을 의미합니다.

<br>
### 군집 타당성 평가
클러스터링 알고리즘은 정답 라벨이 없기 때문에 Accuracy 지표로 표현할 수 없습니다. 또한 최적의 cluster 개수를 알아내는 것은 쉽지 않습니다.
<p align="center"><img src="/assets/img/sensorfusion/clustering.jpg" width="60%" height="40%"></p>
이러한 문제 때문에 clustering은 평가가 어렵습니다. 그래서 군집의 유용성을 다시 말해 군집 간 분산과 군집 내 분산을 기반으로 평가하는 Clustering Validity Index(Dunn Index, Silhouette)가 있습니다.

<br>
### KD-Tree
<p><img src="/assets/img/sensorfusion/KDTree.jpg"></p>
K-D Tree는 공간을 기반으로 partitioning하는 B-Tree(<a href="/2023/01/Tree.html">참조</a>)로, 점들을 k-차원 공간에서 정리합니다. K-D Tree는 다음과 같이 동작합니다.
1. k차원의 점들이 N개 존재
2. X좌표를 기준으로 X의 중간값을 찾고 이진 트리화
3. 자식 노드들은 각각 노드마다 Y의 중간값을 찾고 이진 트리화
4. 손자 노드들은 각각 노드마다 Z의 중간값을 찾고 이진 트리화
5. 증손자 노드들은 각각 노드마다 다시 X의 중간값을 찾고 이진 트리화.. 
6. 반복
<p><img src="/assets/img/sensorfusion/BinaryTree.jpg"></p>


<br>
### Searching Points in a KD-Tree
KD-Tree를 이용하여 거리가 distanceTol(distanceTolerance) 이내인 점들을 찾아내는 방법입니다. \
루트를 시작으로 반복하며 search 함수를 진행하면서 거리 이내의 포인트들을 찾아낼 수 있습니다. \
**< Pseudo Code >**
1. if node가 nullptr이면 종료
2. 만일 현재 노드의 점과 target 점의 X,Y,Z의 거리가 distanceTol보다 크다면 종료(최적화를 위해)
3. 만일 distance D $\sqrt{x^2 + y^2 + z^2}$의 값이 distanceTol보다 크다면 종료(거리 이내에 없기 때문에)
4. KD-Tree의 Depth 기준에 맞게 노드의 점과 target 점의 X(0)Y(1)Z(2) 거리가 distanceTol이내에 있으며
   * 0보다 작으면 search(node$\rightarrow$left)
   * 0보다 크면 search(node$\rightarrow$right)

이렇게 동작하면 KD-Tree를 이용하여 범위 안에 속하는 점들을 빠르게 찾을 수 있습니다.


<br>
### Euclidean Clustering
Euclidean Clustering은 거리를 기준으로 Clustering하는 기법 중 하나입니다. 지금까지 우리는 Euclidean Clustering을 하기 위해 KD-Tree와 KD-Tree를 활용한 근접 점들을 찾는 방법에 대해서 배웠습니다.\
Euclidean Clustering은 다음과 같이 동작합니다. \
1. 각각 점들을 순회하면서
2. 만일 그 점이 처리되지 않은 점이라면
   * Searching Point를 통해서 그 점과 근접한 점들을 획득하고
   * 그 점들을 순회하면서 Searching Point를 하며 다시 근접 점들을 획득
   * 만약 이미 처리되어 있는 점이라면 스킵
3. 이렇게 하나의 Cluster를 만들고
4. 계속해서 순회하면 집단의 Cluster를 획득

이렇게 동작하면 Euclidean Clutering이 완성됩니다!
<p><img src="/assets/img/sensorfusion/Euclidean Clustering.jpg"></p>

<br>
### Bounding Box
이렇게 획득한 Cluster를 기준으로 Bounding Box를 생성하면 더욱 쉽게 확인할 수 있습니다. \
Bounding Box는 각각 점들의 최소 최대 값을 기준으로 생성할 수 있습니다. 다만, 이렇게 생성할 경우 아래 왼쪽 경우처럼 박스가 Fit하게 생성되지 않을 수 있습니다. \
이러한 문제를 PCA를 이용하여 PCA Boxes를 생성하면 오른쪽과 같이 생성할 수 있습니다. PCA의 개념은 MATH 항목을 참조하면 됩니다. 
<p><img src="/assets/img/sensorfusion/Box_Rotate.jpg"></p>


<br><br><br><br><br><br>

---
### 단어
Alternating : 교대로 \
Consecutive : 연이은 \
Dereference : 역참조 \
Proximity : 근접성 \
Negligible : 무시할 수 있는 \
Reap : 베다, 보상을 받다, 받다 \
Innermost : 가장 깊숙히 \
Coincide : 일치하다 \
Chronological : 연대순
