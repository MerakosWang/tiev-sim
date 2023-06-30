#### Manual_control.py 解读

!!! Example 63-69行修改内容
    ```python
    try:
        sys.path.append(glob.glob('PythonAPI/carla/dist/carla-*%d.%d-%s.egg' % (
            sys.version_info.major,
            sys.version_info.minor,
            'win-amd64' if os.name == 'nt' else 'linux-x86_64'))[0])
    except IndexError:
        pass
    ```

- 用try except处理异常，这里是引入python的外部库文件

!!! Example 167行修改内容
    ```python
    self._spawn = args.spawn
    ```
- 返回地图创建者提出的建议列表，这些建议将用作车辆的生成点。该列表包括具有特定位置和方向的carla.Transform对象。为了避免Z型碰撞，上述位置会略微高于地面，所以车辆在开始行驶前会衰落晃动一下。

!!! Example 167行修改内容
    ```python
            if spawn_points:
                spawn_point = random.choice(spawn_points) if self._spawn<0 or self._spawn>len(spawn_points)-1 else spawn_points[self._spawn]
            else:
                spawn_point = carla.Transform()
    ```
- 出生点设置判断


!!! Example 198-201行修改内容
    ```python
            (carla.Transform(carla.Location(x=-5.5, z=2.5), carla.Rotation(pitch=10.0)), Attachment.SpringArm),
            (carla.Transform(carla.Location(x=1.6, z=1.7)), Attachment.Rigid),
            (carla.Transform(carla.Location(x=5.5, y=1.5, z=1.5)), Attachment.Rigid),
            (carla.Transform(carla.Location(x=-8.0, z=6.0), carla.Rotation(pitch=6.0)), Attachment.Rigid),
    ```
- camera设置了一下，俯仰角调整。


!!! Example 716-718行修改内容
    ```python
                bp.set_attribute('range', '4000')
                bp.set_attribute('camera_quality', '0.5')
                bp.set_attribute('points_per_second', '300000')
    ```
- 设定了lidar每一步的计算点数，公式：`points_per_second / (FPS * channels)`.这里的points_per_second是每秒钟获得的点数，这个数量除以通道数是激光雷达每秒钟旋转的次数。


!!! Example 877-881行修改内容
    ```python
    argparser.add_argument(
        '-s', '--spawn',
        default=0,
        type=int,
        help='Spawn point index (default: 0)')
    ```
- argparser是python用于解析命令行参数和选项的模块。type表示参数类型， 此处是出生点设置。
  




运行：
    - manual_control.py ternimal输入python3 manual_control.py 启动