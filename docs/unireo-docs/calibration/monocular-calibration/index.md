---
date:
    created: 2025-01-13
draft: false
---


# 单目标定

## 单目标定概述
&emsp;&emsp;单目相机标定是一种通过捕获不同视角下的标定板图像，利用这些图像计算相机内部参数（如焦距、光学中心等）和畸变系数的过程。通过将相机固定在不同的位置和角度，从多个方向拍摄标定板的照片，分析这些照片中图案的几何变形，并使用特定的算法进行计算，即可得到相机的内部参数和畸变系数。这些参数对于校正相机视图中的畸变非常重要，也是进行精确图像测量和 3D 重建的基础。

&emsp;&emsp;unireo 是借助 OpenCV，基于张友正标定法实现的单目标定，标定过程简单且精确，标定所需步骤如下：
> * 1. 使用相机从不同角度拍摄（棋盘格）标定板约20-30张图像
> * 2. 将保存的所有图像路径转为 `Python` 字符串列表，并利用 unireo 获取单目标定数据
> * 3. 使用 `.` 成员运算符访问单目标定数据对象的成员数据

## ` calib.mono_calib ` 子模块的使用
&emsp;&emsp;在您的项目中，可以这样导入 `calib.mono_calib` 子模块：

``` Python
import unireo.calib.mono_calib as mono_calib
```

&emsp;&emsp;`calib.mono_calib` 子模块具有三个公开静态函数，分别是：
> * ` get_calib_data(chessboard_size: tuple, img_size: tuple, img_series: list) -> calib_data.MonoCalibData `

> * ` undistort_img(img: np.ndarray, mono_calib_data: calib_data.MonoCalibData) -> numpy.ndarray `

> * ` reprojection_error(mono_calib_data: calib_data.MonoCalibData) -> float `

### ` get_calib_data ` 函数的使用
&emsp;&emsp;该函数用于获取单目标定数据，返回值为 ` calib_data.MonoCalibData ` 类型。该函数接受三个参数，分别是：
> * `chessboard_size` ：一个 `tuple` 元组，表示棋盘格的内角点数，如 `(11, 8)`

> * `img_size` ：一个 `tuple` 元组，表示用于标定的图像尺寸，如 `(1280, 720)`

> * `img_series` ：一个 `list` 列表，表示所有标定图像的绝对路径所构成的列表，如` ['/home/1.png', '/home/2.png', '/home/3.png'] `

### ` undistort_img ` 函数的使用
&emsp;&emsp;该函数用于校正**该相机**的画面畸变，并返回一个类型为 ` numpy.ndarray ` 的图像变量。该函数接受两个参数，分别是：
> * `img` ：一个 ` numpy.ndarray ` 图像矩阵，表示原图像

> * ` mono_calib_data ` ：一个 ` calib_data.MonoCalibData ` 类型的变量，表示已经获取到的单目标定数据

### ` reprojection_error ` 函数的使用
&emsp;&emsp;该函数用于计算本次标定所获取的单目标定数据的重投影误差，并返回一个 `float` 类型的误差值（以像素为单位）。一般来说，重投影误差小于等于0.5像素时认为结果良好，而大于2像素值时认为结果应当舍弃。该函数接受一个参数：
> * `mono_calib_data` ：一个 ` calib_data.MonoCalibData ` 类型的变量，表示已经获取到的单目标定数据

&emsp;&emsp;示例：

``` Python
import glob
import cv2
import unireo.calib.mono_calib as mono_calib

# 假定所有拍摄的标定图像位于 "/home/img/" 目录下
# 使用 glob 将 "/home/img/" 目录下所有 PNG 格式的图像整理成字符串数组
images = glob.glob('/home/img/*.png')

# 获取标定数据
mono_calib_data = mono_calib.get_calib_data((11, 8), (1280, 720), images)

# 对 images 中的第一幅图像进行校正并展示
img = cv2.imread(images[0])
img_new = mono_calib.undistort_img(img, mono_calib_data)
cv2.imshow('Original', img)
cv2.imshow('Calibrate', img_new)

# 计算并输出重投影误差
print(f'重投影误差为：{ mono_calib.reprojection_error(mono_calib_data) }')

# 展示5秒后关闭所有窗口并退出程序
cv2.waitKey(5000)
cv2.destroyAllWindows()
```
