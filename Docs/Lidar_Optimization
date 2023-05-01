- **Carla传统Lidar方式：**
  - 在每一帧根据两帧之间的时间差、无人车位置和激光雷达的转速、线数 (Channel) 等参数计算出每一束激光的方位和角度，并调用 UE4 射线碰撞检测函数
  LineTraceSingleByChannel 与仿真环境的物体求交点，之后将交点从世界坐标系变换到无人车的坐标系从而得到点云。

- **采样方式：深度图采样**
    - 在深度图上进行采样，将深度图转化为点云。
      - 论文第9页讲述了激光和深度图坐标的对应关系，给出了具体获得深度的方式


- **优化方式：**
    - 插值优化：双线性插值优化
    - 将Map2D的坐标X_map,Y_map转化为行列坐标line，col，找到其左下，右下，左上，右上距离该店最近的整数进行索引。
    - 公式非常简单，就是上取下界，下去上界。左取下界，右取上界
    - 左下就是line取左，col取下。依次同理

    - 这里获得了line，col后需要获得dep，dep的值有一个具体的公式，DepBuffer2D(line,col)*(line_down-line_raw)(col_right-col_raw).....
      - 具体公式见论文15页。



- **主要实现:**
    - 路径：
      - Unreal/CarlaUE4/Plugins/Carla/Source/Carla/Sensor
        - LidarCaptureSensor.cpp
        -  LidarCaptureSensor.h
        -  LidarShaderSensor.cpp
        -  LidarShaderSensor.h
      - **`LidarCaptureSensor.cpp`**
        - CaptureRenderTarget 是一个 UTextureRenderTarget2D 数组，保存激光雷达传感器采集的图像。
        - 有几个重要的函数
        - 定义了一个类ALidarCaptureSensor，它是USceneCaptureComponent2D的子类，用于实现激光雷达传感器的功能。
        - 其他都是一些基础的函数
          - 例如：SetImageSize，用于设置捕获图像的大小。
            - SetFOVAngle，用于设置相机的视场角。
            - SetExposureMethod，用于设置自动曝光的方法。
            - SetShutterSpeed，用于设置相机快门速度。
          - 这部分主要是为了采集深度图像所用 
        -   BeginPlay
            -   设置了渲染目标
            -   使用 CaptureComponent2D 渲染场景
        - Tick()
          - 根据 LidarSpeedMode 的状态激活下一个捕捉组件并更新其内容
        - EndPlay()
          - 部分值置为零
        - **`LidarCaptureSensor.h`**
            - 类：ALidarCaptureSensor，基于USceneCaptureComponent2D
              - 主要包含设置和获取传感器的属性，例如设置图像的大小、调整感光度、设置模糊度等功能
        - **`LidarShaderSensor.h`**
          - 包含了ALidarShaderSensor类和FLidarDepShader结构体的定义
            - 着色器相关，包括加载UMaterial，添加着色器列表，处理等等。
        -  **`LidarShaderSensor.cpp`**
           -  函数 ALidarShaderSensor::LoadPostProcessingMaterial 是一个公共成员函数，它加载材质，并将其添加到传感器的着色器列表中。
              -  函数通过调用 ConstructorHelpers::FObjectFinder 来查找材质，并将其加载到传感器的着色器列表中，以及相应的权重。如果找到材质，则返回 true，否则返回 false。
           -  函数 ALidarShaderSensor::SetUpSceneCaptureComponent 是一个保护成员函数，它配置传感器的场景捕捉组件。
              -  函数遍历传感器的着色器列表，并将每个着色器添加到场景捕捉组件的后处理设置中，以及相应的权重。


    - 在PythonAPI/examples下有一个新的manual_control_2.py.类比manual_control做了修改
    - test下有测试文件
      - `test_lidar.py`
      - `test_lidaer_2.py`
        - 这两个仿照manual_control.py,只改了CameraManager(object):
          - 更改self.sensors.lidar.ray_cast
          - 主要是:
        ```py
                if item_index == 6:
                bp.set_attribute('cal_ratio', 'false')
                bp.set_attribute('gpu_mode', 'true')
                bp.set_attribute('cal_error', 'true')
                bp.set_attribute('pc_cycle', 'true')
                bp.set_attribute('pc_all', 'true')
                elif item_index == 7:
                bp.set_attribute('cal_ratio', 'false')
                bp.set_attribute('gpu_mode', 'true')
                bp.set_attribute('cal_error', 'true')
                bp.set_attribute('pc_cycle', 'true')
                #bp.set_attribute('pc_all', 'false')
        ```                  
      - `test_memset。cpp`
        - 没用 
