# unireo 文档

## 项目概述
&emsp;&emsp;unireo 是一款使用 `Python` 开发，基于 OpenCV、NumPy 和若干 `Python` 内置 API 开发的微型计算机视觉 SDK，主要面向支持 UVC 协议的同步画面的 RGB 双目相机，未来也将逐步加入对使用 UVC 协议的异步画面的 RGB 双目相机或单目相机的支持。

&emsp;&emsp;unireo 的名字取自 **uni**form 和 ste**reo**，意在为所有支持的双、单目相机提供过去需要几十甚至上百行代码才能完成的任务的简化 API，目前项目仍处于非常初期的阶段，但已经能满足部分使用需求。下面将列出目前 unireo 已支持的任务，同时也将列出 unireo 计划开发的功能以及 unireo 所需的开发环境与依赖。

## unireo 已实现的功能
> * 以正确的后端与参数打开 UVC 相机
> * 显示、保存与读取相机画面
> * 单目相机的内参数标定
> * 双目相机的内、外参数标定
> * 双目相机深度图的生成（试验性）

## unireo 近期计划开发的功能
> * 标定参数的保存与加载
> * 双目相机深度图的生成与显示
> * “眼在手上”与“眼在手外”标定
> * 对单目相机的支持
> * ……

## unireo 所需的环境以及依赖
> * `Python` 基础环境（大于等于 `Python` 3.6 但小于 `Python` 3.12）
> * NumPy（大于等于 1.23.0 但小于 2.0.0）
> * OpenCV-Python（大于等于 4.6.0.66，无需 GPU 加速）

## unireo 项目地址
&emsp;&emsp;访问 [**unireo**](https://github.com/Anawaert/unireo) 项目以获取更多信息。

## 版权所有
&emsp;&emsp;Copyright &copy; 2017-2025 Anawaert Studio.