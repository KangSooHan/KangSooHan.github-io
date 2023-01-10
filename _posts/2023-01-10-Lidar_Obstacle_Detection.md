---
title: Lidar Obstacle Detection Code Review
category: sensorfusion
layout: post
---

### Code Review
지금까지 우리는 Lidar Obstacle Detection을 하기 위해 달려 왔고 크게 4가지를 배웠습니다. 이번 포스트에서는 이 함수들에 관한 code Review 및 Refactoring을 하도록 하겠습니다.
1. Point Cloud Segmentation(RANSAC)
2. Point Cloud Clustering(KD-Tree)
3. Point Cloud Clustering(Euclidean Clustering)
4. Point Cloud Downsampling & Filtering(Voxel Grid)
5. Point Cloud Downsampling & Filtering(Region of Interest)

<br>
### 1. Point Cloud Segmentation(RANSAC)
```cpp
std::unordered_set<int> Ransac(pcl::PointCloud<pcl::PointXYZ>::Ptr cloud, int maxIterations, float distanceTol)
{
    // cloudSize to get random index
    int cloudSize = cloud->size();

    while(maxIterations--)
    {
        std::unordered_set<int> inliers;

        // randomly sample subset
        // select two points.(Line Ransac)
        for(int i=0; i<2; ++i)
            inliers.emplace(rand() % cloudSize);

        // set cant contain same key
        // if inliers size is not two select same key in random sample
        if(inliers.size()!=2) continue;

        // take out two point that are in the form of pcl::PointXYZ format
        auto it = inliers.begin();
        pcl::PointXYZ point1 = cloud->points[*it++], point2 = cloud->points[*it];


        // make line AX+BY+C = 0
        float A = point1.y - point2.y;
        float B = point2.x - point1.x;
        float C = point1.x*point2.y - point2.x*point1.y;

        // iterate all points
        for(int nIndex=0; nIndex < cloudSize; ++nIndex)
        {
            // skip select subset points
            if (inliers.count(nIndex)>0) continue;

            // calculate distance
            float d = fabs(A*cloud->points[nIndex].x + B*cloud->points[nIndex].y + C) / sqrt(A*A+B*B);

            // if distance < distanceTol
            if(d <= distanceTol)
            {
                // it is inliers
                inliers.emplace(nIndex);
            }
        }

        // want to extract plain(biggest one)
        if(inliersResult.size() < inliers.size())
        {
            inliersResult = inliers;
        }
    }

    return inliersResult;
}
```
<a href="/2023/01/SensorFusion_05.html">참조</a>\
<a href="https://github.com/KangSooHan/SensorFusion/blob/main/SFND_Lidar_Obstacle_Detection/src/quiz/ransac/ransac2d.cpp"> github </a>


<br>
### 2. Point Cloud Cluster(KD-Tree)


 


