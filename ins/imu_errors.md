# IMU Errors and Rectification

* [IMU误差模型与校准](https://www.cnblogs.com/buxiaoyi/p/7541974.html)

-----

[TOC]

* 确定性误差（六面法 标定）
  - 开机后恒定的零偏误差（bias）
  - 比例因子误差（scale factor）
  - 轴偏及非正交误差（misalignment errors and non-orthogonality）
  - 非线性误差（non-linearity）
  - 温度误差（thermal noise）
  - 陀螺仪还包含加速度的变化引起的误差（g-dependent noise）
* 随机性误差（Allan方差 标定）
  - 高斯白噪声（Noise Density）
  - 零偏不稳定性（Bias Instability or Random Walk）

<div align=center>
  <img src="../images/error_acc.png">
</div>
<br>
<div align=center>
  <img src="../images/error_gyro.png">
</div>

## IMU Noise Model

* a high frequency additive **White Noise**
* a slower varying sensor **Bias**

**continuous-time** model:

Parameter | YAML element | Symbol | Units
--- | --- | --- | ---
Gyroscope "white noise" | `gyr_n` | <img src="https://latex.codecogs.com/svg.latex?{%5Csigma_g}"> | <img src="https://latex.codecogs.com/svg.latex?{%5Cfrac%7Brad%7D%7Bs%7D%5Cfrac%7B1%7D%7B%5Csqrt%7BHz%7D%7D}">
Accelerometer "white noise" | `acc_n` | <img src="https://latex.codecogs.com/svg.latex?{%5Csigma_a}"> | <img src="https://latex.codecogs.com/svg.latex?{%5Cfrac%7Bm%7D%7Bs^2%7D%5Cfrac%7B1%7D%7B%5Csqrt%7BHz%7D%7D}">
Gyroscope "bias Instability" or "random walk" | `gyr_w` | <img src="https://latex.codecogs.com/svg.latex?{%5Csigma_b_g}"> | <img src="https://latex.codecogs.com/svg.latex?{%5Cfrac%7Brad%7D%7Bs^2%7D%5Cfrac%7B1%7D%7B%5Csqrt%7BHz%7D%7D}" />
Accelerometer "bias Instability" or "random walk" | `acc_w` | <img src="https://latex.codecogs.com/svg.latex?{%5Csigma_b_a}"> | <img src="https://latex.codecogs.com/svg.latex?{%5Cfrac%7Bm%7D%7Bs^3%7D%5Cfrac%7B1%7D%7B%5Csqrt%7BHz%7D%7D}"/>
IMU sampling rate | `update_rate` | <img src="https://latex.codecogs.com/svg.latex?{%5Cfrac%7B1%7D%7B%5CDelta%20t%7D}"> | <img src="https://latex.codecogs.com/svg.latex?{Hz}">

**Ref**:   

* [IMU Noise Model (kalibr)](https://github.com/ethz-asl/kalibr/wiki/IMU-Noise-Model)

* [IMU Noise Model（游）](https://www.cnblogs.com/youzx/p/6291327.html)

### How To Get

#### the Datasheet of the IMU

* White Noise Terms
  - **Rate Noise Density (Angular Random Walk - ARW)**
  - **Acceleration Noise Density (Velocity Random Walk - VRW)**

* Bias Terms
  - **In-Run Bias (Bias Stability)**

#### the Allan standard deviation (AD)

* "white noise" is at tau=1 (slope -1/2 in a log-log AD plot)
* "random walk" is at tau=3 (slope +1/2 in a log-log AD plot)

<div align=center>
  <img src="https://cloud.githubusercontent.com/assets/1916839/3589506/8f57d0ee-0c4e-11e4-9ab4-33821c040490.png"/>
</div>

### Noise Samples (Continuous-time)

* MPU6000 / MPU6050

  ```yaml
  core_noise_acc: 0.003924    # [m/s^2/sqrt(Hz)] mpu6000 datasheet
  core_noise_gyr: 0.00008726  # [rad/s/sqrt(Hz)] mpu6000 datasheet
  ```

* ADIS 16448

  ```yaml
  # avg-axis
  gyr_n: 1.8582082627718251e-04
  gyr_w: 7.2451532648461174e-05
  acc_n: 1.9862287242243099e-03
  acc_w: 1.2148497781522122e-03
  ```

* MYNT-EYE-S1030 IMU

  ```yaml
  gyr_n: 0.00888232829671
  gyr_w: 0.000379565782927
  acc_n: 0.0268014618074
  acc_w: 0.00262960861593
  ```

## Performance Analysis Software
  - [IMU-TK](https://bitbucket.org/alberto_pretto/imu_tk): Inertial Measurement Unit ToolKit
  - [gaowenliang/imu_utils](https://github.com/gaowenliang/imu_utils): A ROS package tool to analyze the IMU performance
  - [rpng/kalibr_allan](https://github.com/rpng/kalibr_allan): IMU Allan standard deviation charts for use with Kalibr and inertial kalman filters
  - [XinLiGH/GyroAllan](https://github.com/XinLiGH/GyroAllan): 陀螺仪随机误差的 Allan 方差分析
  - [AllanTools](https://pypi.org/project/AllanTools/): A python library for calculating Allan deviation and related time & frequency statistics.
