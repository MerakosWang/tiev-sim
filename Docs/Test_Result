
<h4>所有更改的目录树:</h4>

**Python API /examples**
**Unreal/CarlaUE4**

- CarlaUE4.uproject
- **Config**
- **Plugins/Carla/Source**




!!!Error
     Traceback(most recent call last):File "test_lidar.py",line 77,in &lt;module&gt; import carla ImportError:No module named carla

- 原因：程序找不到 CARLA Python的路径
- 解决：使用.egg文件(included)的完整路径执行以下命令
- 找到carla-0.9.11-py3.7-linux-x86_64.egg
  - 执行`unzip carla-0.9.11-py3.7-linux-x86_64.egg -d carla-0.9.11-py3.7-linux-x86_64`


!!!Error
    RuntimeError:time-out of 2000ms while waiting for the simulator,make sure the simular is ready and connected to 127.0.0.1:2000

- 原因：没有启动客户端
- 解决：在downloads\Carla_0.9.13\下运行CarlaUE4.sh,再运行



!!!ERROR
    Traceback (most recent call last):File "test_lidar.py", line 930, in <module>main()File "test_lidar.py", line 922, in maingame loop(args)File "test lidar.py", line 833, in game_loopworld = world(client.get world(), hud, args)File "test lidar.py", line 168, in . initself.restart()File "test_lidar.py", line 207, in restartself.camera manager = CameraManager(self.player, self.hud, self. gamma)File "test lidar.py",line 721. ininitbp.setattribute('camera width' 1000')IndexError: attribute'camera width' not foundWARNING: sensor object went out of the scope but the sensor is still alive in the simulation: Actor 25(sensor.other.collision)WARNING: sensor object went out of the scope but the sensor is still alive in the simulation: Actor 27 (sensor.other .gnss)terminate called without an active exception(core dumpedAborted
- **bxd** 的测试文件,test_lidar.py
- 原因：由提示信息可知，删除了camera但是再camera width仍然在sensor object中，通过对源码分析，发现在camera manager下，多了两行对camera的定义语句

```python
            elif item[0].startswith('sensor.lidar'):
                bp.set_attribute('range', '5000')
                bp.set_attribute('points_per_second', '160000')
                bp.set_attribute('channels', '32')
                #bp.set_attribute('camera_width', '1000')
                #bp.set_attribute('camera_height', '1000')
                bp.set_attribute('lidar_visible', '1')
                bp.set_attribute('lidar_shift', '1.0')
                bp.set_attribute('speed_mode', 'true')
                bp.set_attribute('old_mode', 'false')
                #bp.set_attribute('pc_cycle', 'true')

```
- 解决：将camera_width和camera_height进行注释






#### 实验问题

- **bxd**所上传的test和manual_control均无法正常打开
  - 具体情形为，运行官方样例文件，均可成功打开pygame界面，而bxd的代码文件，运行均处于pygame黑屏界面，无任何报错提醒，点击触发未响应弹窗，退出也无任何提示信息。
