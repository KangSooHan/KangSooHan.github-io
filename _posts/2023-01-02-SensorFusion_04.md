---
title: Highway Simulation - Lider 부수기
category: sensorfusion
layout: post
---

### Highway Simulation

### Lidar 부수기

#### main 함수
```cpp
void simpleHighway(pcl::visualization::PCLVisualizer::Ptr& viewer)
{
    // RENDER OPTIONS
    bool renderScene = true;
    std::vector<Car> cars = initHighway(renderScene, viewer);

    // Lidar 함수 파해치기!!
    Lidar* lidar = new Lidar(cars, 0);

    pcl::PointCloud<pcl::PointXYZ>::Ptr cloud = lidar->scan();
    renderRays(viewer, lidar->position, cloud);

    renderPointCloud(viewer, cloud, "highwayScene", Color(1, 1, 1));
}

struct Lidar
{
    std::vector<Ray> rays;
    pcl::PointCloud<pcl::PointXYZ>::Ptr cloud;
    std::vector<Car> cars;

    Lidar()
    {
        // Vertical Angle 가장 가파른 steepestAngle부터 angleRange까지 angleIncrement(angleRange/numLayers) 단위로
        for(double angleVertical = steepestAngle; angleVertical < steepestAngle+angleRange; angleVertical+=angleIncrement)
        {
            // 360도 각도를 horizontalAngleInc 단위로
            for(double angle = 0; angle <= 2*pi; angle+=horizontalAngleInc)
            {

                //Ray 생성 후 vector에 넣기
                Ray ray(position,angle,angleVertical,resolution);
                rays.push_back(ray);
            }
        }
    }

    pcl::PointCloud<pcl::PointXYZ>::Ptr scan()
    {
        // point cloud 초기화
        cloud->points.clear();

        // chrono 시간 측정 library, steady_clock(역행 불가 시계 system_clock, high_resolution_clock도 있음), now 현재
        auto startTime = std::chrono::steady_clock::now();

        // Ray를 돌면서 rayCast 실행
        for(Ray ray : rays)
            ray.rayCast(cars, minDistance, maxDistance, cloud, groundSlope, sderr);

        auto endTime = std::chrono::steady_clock::now();
        auto elapsedTime = std::chrono::duration_cast<std::chrono::milliseconds>(endTime - startTime);
        cout << "ray casting took " << elapsedTime.count() << " milliseconds" << endl;
        
        //PointCloud에서 width와 height(2d map에서) point 개수, height=1이면 unordered point cloud
        cloud->width = cloud->points.size();
        cloud->height = 1; // one dimensional unorganized point cloud dataset
        return cloud;
    }

}


struct Ray
{
    void rayCast(const std::vector<Car>& cars, double minDistance, double maxDistance, pcl::PointCloud<pcl::PointXYZ>::Ptr& cloud, double slopeAngle, double sderr)
    {

        while(!collision && castDistance < maxDistance)
        {
            castPosition = castPosition + direction;
            castDistance += resolution;

            // check if there is any collisions with ground slope
            collision = (castPosition.z <= castPosition.x * tan(slopeAngle));

            // check if there is any collisions with cars
            if(!collision && castDistance < maxDistance)
            {
                for(Car car : cars)
                {
                    collision |= car.checkCollision(castPosition);
                    if(collision)
                        break;
                }
            }
        }

        if((castDistance >= minDistance)&&(castDistance<=maxDistance))
        {
            // add noise based on standard deviation error
            double rx = ((double) rand() / (RAND_MAX));
            double ry = ((double) rand() / (RAND_MAX));
            double rz = ((double) rand() / (RAND_MAX));
            cloud->points.push_back(pcl::PointXYZ(castPosition.x+rx*sderr, castPosition.y+ry*sderr, castPosition.z+rz*sderr));
        }

    }
}

void renderRays(pcl::visualization::PCLVisualizer::Ptr& viewer, const Vect3& origin, const pcl::PointCloud<pcl::PointXYZ>::Ptr& cloud)
{
    //PointCloud를 돌면서 
    for(pcl::PointXYZ point : cloud->points)
    {
        //Line을 그리기
        viewer->addLine(pcl::PointXYZ(origin.x, origin.y, origin.z), point,1,0,0,"ray"+std::to_string(countRays));
        countRays++;
    }
}


void renderPointCloud(pcl::visualization::PCLVisualizer::Ptr& viewer, const pcl::PointCloud<pcl::PointXYZ>::Ptr& cloud, std::string name, Color color)
{
    //viewer에 pointcloud 추가
    viewer->addPointCloud<pcl::PointXYZ> (cloud, name);
    viewer->setPointCloudRenderingProperties (pcl::visualization::PCL_VISUALIZER_POINT_SIZE, 4, name);
    viewer->setPointCloudRenderingProperties (pcl::visualization::PCL_VISUALIZER_COLOR, color.r, color.g, color.b, name);
}
```


<br><br><br><br><br><br>

---
### 단어
instantiate : 인스턴스화(Object를 \
Compliation : \
Orbit : \
Discern : 분별하다, \
Steepest : 가장 \
