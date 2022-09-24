后续代码整理后，陆续上传。

# NanoDet-PyTorch

* 说明：NanoDet作者开源代码地址：https://github.com/RangiLyu/nanodet  （致敬）
* **该代码基于NanoDet模型进行小裁剪，下载直接能使用，支持图片、视频文件、摄像头实时目标检测。**


## 原模型性能

Model     |Resolution|COCO mAP|Latency(ARM 4xCore)|FLOPS|Params   | Model Size(ncnn bin)
:--------:|:--------:|:------:|:-----------------:|:---:|:-------:|:-------:
NanoDet-m | 320*320 |  20.6 | 10.23ms | 0.72B   | 0.95M | 1.8mb
NanoDet-m | 416*416 |  21.7 | 16.44ms | 1.2B    | 0.95M | 1.8mb
YoloV3-Tiny| 416*416 | 16.6 | 37.6ms  | 5.62B   | 8.86M | 33.7mb
YoloV4-Tiny| 416*416 | 21.7 | 32.81ms | 6.96B   | 6.06M | 23.0mb
Our SAT  | 320*320 | 21.1 | 8.52ms | 0.65B   | 0.84M | 4.21mb

说明：
* 以上性能基于 ncnn 和麒麟 980 (4xA76+4xA55) ARM CPU 获得的
* 使用 COCO mAP (0.5:0.95) 作为评估指标，兼顾检测和定位的精度，在 COCO val 5000 张图片上测试，并且没有使用 Testing-Time-Augmentation。

## NanoDet损失函数
* NanoDet 使用了李翔等人提出的 Generalized Focal Loss 损失函数。该函数能够去掉 FCOS 的 Centerness 分支，省去这一分支上的大量卷积，从而减少检测头的计算开销，非常适合移动端的轻量化部署。
* 详细请参考：Generalized Focal Loss: Learning Qualified and Distributed Bounding Boxes for Dense Object Detection

##本人的 开发环境
operation system (OS) ： Win 10; 
Development platform: Python3.8+OpenCV+PyCharm+torch1.12.0+cuda116
CPU: Intel(R) Core (TM) i7
GPU: GeForce RTX 3080 SUPER
Memory: 16 G; Disk:1 T.

Windows开发环境安装可以参考：
```text
安装cudatoolkit 10.1、cudnn7.6请参考 https://blog.csdn.net/qq_41204464/article/details/108807165
安装PyTorch请参考 https://blog.csdn.net/u014723479/article/details/103001861
安装pycocotools请参考 https://blog.csdn.net/weixin_41166529/article/details/109997105
```

## 运行程序
```text
'''目标检测-图片'''
# python detect_main.py image --config ./config/nanodet-m.yml --model model/nanodet_m.pth --path  street.png

'''目标检测-视频文件'''
# python detect_main.py video --config ./config/nanodet-m.yml --model model/nanodet_m.pth --path  test.mp4

'''目标检测-摄像头'''
# python detect_main.py webcam --config ./config/nanodet-m.yml --model model/nanodet_m.pth --path  0
```

## 总结
* 经过改进后的NanoDet，通过测试发现在速度方面，与原NanoDet差不多，在处理低像素图片时更快。
* 适用于对检测精度要求不高的，对实时要求高的移动端或嵌入式设备。
