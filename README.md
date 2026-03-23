<!--
 * @Author: ei_code_bash && 3080152159@qq.com
 * @Date: 2026-03-23 18:10:48
 * @LastEditors: ei_code_bash && 3080152159@qq.com
 * @LastEditTime: 2026-03-23 20:03:59
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
