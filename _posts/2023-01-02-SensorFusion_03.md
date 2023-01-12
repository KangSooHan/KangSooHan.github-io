---
title: Highway Simulation - Environment.CPP 부수기
category: sensorfusion
layout: post
---

## Highway Simulation

### Environment.cpp 부수기

 main 함수
```cpp
int main()
{
    // viewer 객체 생성
    pcl::visualization::PCLVisualizer::Ptr viewer (new pcl::visualization::PCLVisualizer ("3D Viewer"));

    // getRenderWindow() : VTK Render Window가 사용하는 pointer를 가져옴
    // GlobalWarningDisplayOff() : VTK 에러창 안보이게
    viewer->getRenderWindow()->GlobalWarningDisplayOff();

    // ENUM CameraAngle
    CameraAngle setAngle = XY;

    // initCamera 함수
    initCamera(setAngle, viewer);

    // simpleHighway 함수
    simpleHighway(viewer);

    // interactor를 부르고 내부적인 loop를 실행시킴(screen을 업데이트)
    viewer->spin();
}
```

 initCamera 함수
```cpp
//CameraAngle(Enum), PCLVisualizer pointer
void initCamera(CameraAngle setAngle, pcl::visualization::PCLVisualizer::Ptr& viewer)
{
    // setBackgroundColor : viewer의 backgroundColor 설정(0, 0, 0) -> 검정색
    viewer->setBackgroundColor (0, 0, 0);

    // set camera position and angle
    // camera parameter를 default value로 init
    viewer->initCameraParameters();

    // distance away in meters
    int distance = 16;

    // setCameraPosition : 카메라의 위치 조정(x, y, z, up_x, up_y, up_z) up은 시점 방향
    switch(setAngle)
    {
        case XY : viewer->setCameraPosition(-distance, -distance, distance, 1, 1, 0); break;
        case TopDown : viewer->setCameraPosition(0, 0, distance, 1, 0, 1); break;
        case Side : viewer->setCameraPosition(0, -distance, 0, 0, 0, 1); break;
        case FPS : viewer->setCameraPosition(-10, 0, 0, 0, 0, 1);
    }

    if(setAngle!=FPS)
        // addCoordinateSystem(scale, id, viewport) : 0,0,0에 3차원 좌표계 생성
        // Eigen:Affine3f 혹은 x, y, z,를 줌으로써 변경 가능(optional)
        viewer->addCoordinateSystem (1.0);
}
```

 simpleHighway 함수
```cpp
void renderHighway(pcl::visualization::PCLVisualizer::Ptr& viewer)
{

    // units in meters
    double roadLength = 50.0;
    double roadWidth = 12.0;
    double roadHeight = 0.2;

    // addCube(x_min, x_max, y_min, y_max, z_min, z_max, r, g, b, id) : Cube 모델 추가    
    viewer->addCube(-roadLength/2, roadLength/2, -roadWidth/2, roadWidth/2, -roadHeight, 0, .2, .2, .2, "highwayPavement");

    // setShapeRenderingProperties(property, double, {double, double}(optional), id) : 모양에 대한 rendering properties를 설정
    // double 1개 : shape, double 2개 : 2x values - e.g, minmax values, double 3개 : 3x values - e.g., RGB
    viewer->setShapeRenderingProperties(pcl::visualization::PCL_VISUALIZER_REPRESENTATION, pcl::visualization::PCL_VISUALIZER_REPRESENTATION_SURFACE, "highwayPavement");
    viewer->setShapeRenderingProperties(pcl::visualization::PCL_VISUALIZER_COLOR, .2, .2, .2, "highwayPavement");
    viewer->setShapeRenderingProperties(pcl::visualization::PCL_VISUALIZER_OPACITY, 1.0, "highwayPavement");

    // addLine(point1, point2) : point 1에서 point 2까지 선 추가
    viewer->addLine(pcl::PointXYZ(-roadLength/2,-roadWidth/6, 0.01),pcl::PointXYZ(roadLength/2, -roadWidth/6, 0.01),1,1,0,"line1");
    viewer->addLine(pcl::PointXYZ(-roadLength/2, roadWidth/6, 0.01),pcl::PointXYZ(roadLength/2, roadWidth/6, 0.01),1,1,0,"line2");                                                                                 }

void simpleHighway(pcl::visualization::PCLVisualizer::Ptr& viewer)
{
    // RENDER OPTIONS
    bool renderScene = true;

    // initHighway 함수
    std::vector<Car> cars = initHighway(renderScene, viewer);
}

std::vector<Car> initHighway(bool renderScene, pcl::visualization::PCLVisualizer::Ptr& viewer)
{
    Car egoCar( Vect3(0,0,0), Vect3(4,2,2), Color(0,1,0), "egoCar");
    Car car1( Vect3(15,0,0), Vect3(4,2,2), Color(0,0,1), "car1");
    Car car2( Vect3(8,-4,0), Vect3(4,2,2), Color(0,0,1), "car2");
    Car car3( Vect3(-12,4,0), Vect3(4,2,2), Color(0,0,1), "car3");

    std::vector<Car> cars;
    cars.push_back(egoCar);
    cars.push_back(car1);
    cars.push_back(car2);
    cars.push_back(car3);

    if(renderScene)
    {
        renderHighway(viewer);
        egoCar.render(viewer);
        car1.render(viewer);
        car2.render(viewer);
        car3.render(viewer);
    }
    return cars;
}
```





<br><br><br><br><br><br>

---
### 단어
Underlying : 근본적인


