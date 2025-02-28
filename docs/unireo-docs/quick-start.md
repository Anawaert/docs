---
date:
    created: 2025-01-13
comments: true
draft: false
---

# 快速开始

## 配置开发所需环境
&emsp;&emsp;确保您的 `Python` 版本高于 `Python` 3.6，且已安装最新版本的 pip 包管理器，并完成 OpenCV 和 NumPy 库的安装。若仍未安装上述模块，请在终端中执行以下命令：

``` bash
# 检查 Python 版本
python --version
# 或
python3 --version

# 安装 OpenCV 和 NumPy
pip install numpy==1.23.0
pip install opencv-python==4.9.0
```

!!! warning "易错配置"
    &emsp;&emsp;若您曾经通过编译 OpenCV 的 `C++` 库生成 OpenCV 的 `Python` 绑定，请**不要**在此处使用 `pip install opencv-python==4.9.0.80` ，因为这样会使得通过 pip 安装的 OpenCV 覆盖原本使用编译生成的 OpenCV 的 `Python` 绑定。

## 通过 pip 包管理器安装 unireo
&emsp;&emsp;在终端中，使用以下命令安装 unireo 模块：

``` bash
pip install unireo
```

&emsp;&emsp;完成安装后，在终端中运行 `python` 或 `python3` 命令，在 `Python` 中尝试导入 unireo 来是检查否已正确安装：

``` Python
import unireo
```

## 使用 unireo 来尝试打开您的双目相机并拍摄图像
&emsp;&emsp;使用以下代码示例即可使用 unireo 对双目相机的画面进行拍摄：

``` Python
# 导入 unireo 模块中有关相机信息的子模块
import unireo.cam_info as cam_info
# 导入 unireo 模块中与错误码与异常处理相关的子模块
import unireo.err_proc as err_proc
# 导入 OpenCV 模块
import cv2

# 设定相机启动参数 params，并将其传给 Camera 类的构造函数，最后打开相机
params = cam_info.InitParameters()
camera = cam_info.Camera(params)
camera.open()

while True:
    # 使用 Camera 类的 grab_frame() 方法来抓取一帧图像，该函数会返回一个错误码
    # 当错误码并非 GRAB_IMAGE_SUCCESS 时，即未成功抓取图像时，说明相机打开错误，即退出循环
    if camera.grab_frame() != err_proc.ErrCode.GRAB_IMAGE_SUCCESS:
        print("Failed to Read Image from Camera")
        break
    
    # 使用 Camera 类的 get_img() 方法来读出抓取到的图像
    # LEFT_IMAGE 和 RIGHT_IMAGE 分别代表左、右图像，这两个变量 Flag 都在 cam_info 的 GrabbedImageType 类中定义
    left = camera.get_img(cam_info.GrabbedImageType.LEFT_IMAGE)
    right = camera.get_img(cam_info.GrabbedImageType.RIGHT_IMAGE)

    # 使用 OpenCV 显示读出画面
    cv2.imshow('Left', left)
    cv2.imshow('Right', right)

    # 使用 Camera 类的 get_stereo_img_saved() 方法来读出抓取到的图像
    # 该函数需要传入四个参数，分别是：
    # 1. 图像要保存的格式，如 JPG 或 PNG（推荐）格式
    # 2. 双目相机左目画面的保存路径，如'/home/left'，前提是该文件夹已经存在，否则无法保存
    # 3. 双目相机右目画面的保存路径，如'/home/right'，前提是该文件夹已经存在，否则无法保存
    # 4. 需要保存画面时按下的键盘按键，如s键
    camera.get_stereo_img_saved(cam_info.SavedImageFormat.IMG_FMT_PNG, '/home/left', '/home/right', 's')
    
# 若退出主循环，需要关闭所有窗口并且释放相机占用资源
cv2.destroyAllWindows()
camera.release()
```

## 深入使用
&emsp;&emsp;若想探索 unireo 的更多 API 与功能，请继续阅读，查阅更多的模块与 API 的使用方法。
