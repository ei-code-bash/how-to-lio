<!--
 * @Author: ei_code_bash && 3080152159@qq.com
 * @Date: 2026-03-23 18:10:48
 * @LastEditors: ei_code_bash && 3080152159@qq.com
<<<<<<< HEAD
 * @LastEditTime: 2026-04-27 20:56:44
=======
 * @LastEditTime: 2026-03-23 20:03:59
>>>>>>> 8087bdc03bef816e8235dab7aea3c5850d44767c
 * @FilePath: /how-to-lio/README.md
 * @Description: 我永远喜欢雪之下雪乃
 * 
 * Copyright (c) 2026 by ei_code_bash, All Rights Reserved. 
-->
# how-to-lio  
自己对所有里程计代码的阅读总结
## 对于preprocess.h/cpp  
1.对于相关参数 🌟   
先是对于不同雷达类型进行分类，并都将其注册到PCL库中去。  
>PointCloudXYZI  pl_full, pl_corn, pl_surf   全部点，角点，平面点  
>blind = 0.01  对于1cm范围内的点直接丢掉(单位米)  
> point_fliter_num 点云下采样的参数，为1时不进行采样  
> det_range = 100 最大检测的距离，超过该距离的点可以忽略  
> N_SCANS = 6 扫描线的数量  
> SCAN_RATE = 10 雷达的扫描频率  
>disA 最小的距离阈值  
>inf_bound = 10 两个点的之间的距离差  
>p2l_ratio = 225 用于判断一组点是否贴近一条直线/平面  
>limit_maxmid = 6.25  
>limit_midmin = 6.25  
>limit_maxmin = 3.24  
max最大距离，mid表示中间距离，min表示最小的距离。这些参数控制是否构成平面，是否形状合理  
>边缘检测参数  
jump_up_limit = 170.0
jump_down_limit = 8.0
<<<<<<< HEAD
## IMU_Process.h/cpp
### 主要的功能  
1.IMU静止的初始化  
2.对于平均加速度与角速度之间的估计  
3.对于初始的姿态的对其  
ImuProcess主要处理的是一组同步的激光雷达和IMU数据，即观测的数据有雷达帧的开始和结束的时间，imu的数据，（x,y,z的线性加速度，x,y,z的角加速度,四元数表示的物体的旋转姿态）即为观测的数据  
4.对于平均加速度与平均角速度平均估计（累积100次）  
5.重置Reset imu的状态器（异常处理）  
6.根据平均加速度方向与配置中的重力方向计算一个Rot,来用于对其的处理  
7.这个部分似乎只完成了IMU的初始化阶段的数据累积，平均加速度，平均角速度，根据重力方向计算初始的姿态，基于时间戳对点云进行排序。
## laserMapping 
1.订阅的部分  
sub_pcl_livox / sub_pcl_pc   
sub_imu  
- Livox 用livox_ros_driver2::msg::CustomMsg  
其他雷达用的PointCloud2  
IMU用标准的sensor_msgs::msg::Imu  

2.发布的部分  
1.cloud_registered  
2.cloud_registered_body  
3.cloud_effected  
4.Laser_map  
5.aft_mapped_to_init  
6.path  
注意的是拿到的某一帧的点云必须要与这个时刻的imu的数据是对应的。才能打包成同一MeasureGroup.  
点云似乎是没有被去畸变的，而是直接拷贝过来的  
3.对于地图的初始化  
1.前几帧的点云不做scan_to_map的优化  
2.直接根据当前的为姿把点从雷达系转到世界系  
3.累积够init_map_size的点后，加入到ivox部分的地图  
4.后续帧才真正的开始匹配地图  
5.对于状态的估计  
IMU作为观测/输出的状态  
## Estimator.h/cpp    
1.到底干了什么  
1.定义滤波器状态，过程模型，噪声模型  
2.把点到地图平面的机和约束写成ESKF的观测方程  
state_input 24维 state_output，30维  
