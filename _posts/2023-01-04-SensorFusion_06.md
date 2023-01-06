---
title: Point Cloud Clustering
category: sensorfusion
layout: post
---

## Point Cloud Clustering
Point Cloud에서 Clustering은 보행자, 자동차 등 obstacle들을 찾아내는데 사용합니다. Udacity 강의에서는 KD-TREE를 사용한 Euclidean Clustering을 사용합니다.

### Clustering
클러스터링(Clustering : 군집화)는 비슷한 개체끼리 한 그룹으로, 다른 개체는 다른 그룹으로 묶는 것을 의미합니다. 클러스터링은 다음과 같이 얘기할 수 있습니다.
* 군집 간 분산(outer-cluster variance) 최대화
* 군집 내 분산(inner-cluster variance) 최소화

클러스터링은 정답 라벨이 없는 Unsupervised Learning(비지도 학습) 입니다. 이는 그룹 정보 없이 비슷한 개체끼리 군집화 하는 것을 의미합니다.

#### 군집 타당성 평가
클러스터링 알고리즘은 정답 라벨이 없기 때문에 Accuracy 지표로 표현할 수 없습니다. 또한 최적의 cluster 개수를 알아내는 것은 쉽지 않습니다.
<p align="center"><img src="/assets/img/sensorfusion/clustering.jpg" width="60%" height="40%"></p>
이러한 문제 때문에 clustering은 평가가 어렵습니다. 그래서 군집의 유용성을 다시 말해 군집 간 분산과 군집 내 분산을 기반으로 평가하는 Clustering Validity Index(Dunn Index, Silhouette)가 있습니다.

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
Chronological : 연대순 \
