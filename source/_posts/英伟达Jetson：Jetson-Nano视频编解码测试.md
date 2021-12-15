---
title: 英伟达Jetson：Jetson Nano视频编解码测试
date: 2021-07-20 19:05:37
img: /images/2.png
author: Yuri
top: false
summary: 本文主要测试Jetson Nano的编解码的能力是否符合官方文档所展示，本文主要基于1080P和4K的两类分辨率视频进行H.264和H.265的编解码测试，测试内容主要有：1080P，4K视频的直接解码测试；从1080P的USB摄像头和4K的CSI摄像头获取图像进行编码再进行解码显示。
categories: 深度学习
tags: 
   - 深度学习
   - 边缘AI
   - Jetson
---
## 前言
本文主要测试Jetson Nano的编解码的能力是否符合官方文档所展示，本文主要基于1080P和4K的两类分辨率视频进行H.264和H.265的编解码测试，测试内容主要有：1080P，4K视频的直接解码测试；从1080P的USB摄像头和4K的CSI摄像头获取图像进行编码再进行解码显示。

## 一、安装 GStreamer
- 首先使用以下命令来安装Gstreamer1.0
```
sudo add-apt-repository universe
sudo add-apt-repository multiverse
sudo apt-get update
sudo apt-get install gstreamer1.0-tools gstreamer1.0-alsa \
  gstreamer1.0-plugins-base gstreamer1.0-plugins-good \
  gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly \
  gstreamer1.0-libav
sudo apt-get install libgstreamer1.0-dev \
  libgstreamer-plugins-base1.0-dev \
  libgstreamer-plugins-good1.0-dev \
  libgstreamer-plugins-bad1.0-dev
```
- 查看Gstreamer版本
```
gst-inspect-1.0 --version
```

## 二、Gstreamer介绍
- Gstreamer1.0包含 gst-omx解码插件如下所示：
![](https://img-blog.csdnimg.cn/img_convert/8cd7aa4b688340d096f6172437a3e649.png)
- GStreamer 1.0 包括以下 gst-v4l2 视频解码器：
![](https://img-blog.csdnimg.cn/img_convert/ea1cd3e38dc9e653b920e7b26f850903.png)
- GStreamer 1.0 包括以下 gst-omx 视频编码器：
![](https://img-blog.csdnimg.cn/img_convert/d66e34505e39277d0450f2145abeed81.png)
- GStreamer 1.0 包括以下 gst-v4l2 视频编码器：
![](https://img-blog.csdnimg.cn/img_convert/2d5fad6e22f577384b69c654f661e798.png)
- Gstreamer1.0包括以下几种视频接收器  
![](https://img-blog.csdnimg.cn/img_convert/81325a5aaca8189a85a79c2c49093c07.png)

## 三、安装v4l-utils
- 终端输入以下命令来安装
```
sudo apt-get install v4l-utils
```
- 利用V4-utils查询当前的USB设备
```
v4l2-ctl --list-devices
```
- 打开USB摄像头并显示出来
```
gst-launch-1.0 v4l2src ! xvimagesink
```

## 四、USB摄像头的编解码测试
> 备注：本人的摄像头最高只支持1920x1080，30FPS，这里根据自己个人摄像头情况来修改下面的命令，如果不知道可以通过`v4l2-ctl -d /dev/video0 --list-format-ext`来查询自己的USB设备支持格式
- 使用gst-omx 进行H264硬件编码（打开摄像头进行视频录制，按下Ctrl+C即可退出保存视频文件）
```
gst-launch-1.0 v4l2src device="/dev/video0" ! "video/x-raw, width=1920, height=1080, format=(string)YUY2" ! nvvidconv ! 'video/x-raw(memory:NVMM), format=(string)I420' ! omxh264enc ! 'video/x-h264, stream-format=(string)byte-stream' ! h264parse ! qtmux ! filesink location=1080P.mp4 -e
```
- 使用gst-omx 进行H265硬件编码（打开摄像头进行视频录制，按下Ctrl+C即可退出保存视频文件）
```
gst-launch-1.0 v4l2src device="/dev/video0" ! "video/x-raw, width=1920, height=1080, format=(string)YUY2" ! nvvidconv ! 'video/x-raw(memory:NVMM), format=(string)I420' ! omxh265enc ! 'video/x-h265, stream-format=(string)byte-stream' ! h264parse ! qtmux ! filesink location=1080P.mp4 -e
```
> 备注：如果下面解码后显示的界面太大可以将下面`nvoverlaysink`改成`nv3dsink`这样显示就变成窗口化
- 使用gst-omx进行H264硬件解码（对上面保存的视频文件进行解码）
```
gst-launch-1.0 filesrc location=1080p.mp4 ! \ qtdemux name=demux demux.video_0 ! queue ! h264parse ! omxh264dec ! \ nvoverlaysink -e
```
- 使用gst-omx进行H265硬件解码（对上面保存的视频文件进行解码）
```
gst-launch-1.0 filesrc location=1080p.mp4 ! \ qtdemux name=demux demux.video_0 ! queue ! h265parse ! omxh265dec ! \ nvoverlaysink -e
```
> 备注：下面只展示了gst-v4l2的H264编解码，这里不再重复H265，因为与上面的gst-omx编解码方式相同只不过把对应的编解码参数改变而已
- 使用gst-v4l2进行H264硬件编码
```
gst-launch-1.0 v4l2src device="/dev/video0" ! "video/x-raw, width=1920, height=1080, format=(string)YUY2" ! nvvidconv ! 'video/x-raw(memory:NVMM), format=(string)I420' ! nvv4l2h264enc ! 'video/x-h264, stream-format=(string)byte-stream' ! h264parse ! qtmux ! filesink location=1080P.mp4 -e
```
- 使用gst-v4l2进行H264硬件解码
```
gst-launch-1.0 filesrc location=1080p.mp4 ! \ qtdemux name=demux demux.video_0 ! queue ! h264parse ! nvv4l2decoder enable-max-performance=1 ! \ nvoverlaysink -e
```
> 备注：以下展示H264和omx，H265以及v4l2自行类比
- 网络流式传输RTSP，这里展示在同个设备下进行网络传输
```
gst-launch-1.0 v4l2src ! decodebin ! videoconvert ! omxh264enc ! video/x-h264, stream-format=byte-stream ! rtph264pay ! udpsink host=127.0.0.1 port=5000
```
- 再打开一个新的终端
```
gst-launch-1.0 udpsrc port=5000 ! application/x-rtp, encoding-name=H264, payload=96 ! rtph264depay ! h264parse ! omxh264dec ! nvoverlaysink
```

## 五、本地视频文件解码测试
> 备注：这里视频文件请自行查找获得，本文共测试了1080P，4K，6K，8K的视频素材文件，并且以下的解码方式为gst-omx进行，gst-v4l2不再重复;其中filename文件根据自己的文件名来填入；另外由于网络上大部分视频文件都是以H264编码因此这里也只展示了H264的解码
- 对本地视频文件进行gst-omx的H264解码
```
gst-launch-1.0 filesrc location=<filename.mp4> ! \ qtdemux name=demux demux.video_0 ! queue ! h264parse ! omxh264dec ! \ nveglglessink -e
```
## 六、CSI摄像头的编解码测试
> 备注：这里使用的是Jetson-IMX477-RPIV3摄像头
- 进行4K的视频编码保存
```
SENSOR_ID=0 # 0 for CAM0 and 1 for CAM1 ports
FRAMERATE=30 # Framerate can go from 2 to 30 for 4032x3040 mode
gst-launch-1.0 -e nvarguscamerasrc sensor-id=$SENSOR_ID ! "video/x-raw(memory:NVMM),width=4096,height=2160,framerate=$FRAMERATE/1" ! nvv4l2h264enc ! h264parse ! mp4mux ! filesink location=4K.mp4
```
- 进行4K视频的解码显示
```
gst-launch-1.0 filesrc location=4K.mp4 ! \ qtdemux name=demux demux.video_0 ! queue ! h264parse ! nvv4l2decoder enable-max-performance=1 ! \ nv3dsink -e
```
## 七、总结
> 总的来说，Jetson Nano对于4K视频可以不管在H.264还是H.265下做到很好地编解码，对于4K以上视频进行测试后发现将会超出Nano能力范围无法运行，对于1080P的图像最多可以4路进行编解码，总结来说就是Jetson nano 的编解码能力在4K视频流范围内

## 八、参考
[1] [Linux之gstreamer视频编解码测试指令](https://blog.csdn.net/zong596568821xp/article/details/114585504)  
[2] [Jetson DeepStream GStreamer使用记录](https://blog.csdn.net/chongzi865458/article/details/102865374)  
[3] [英伟达开发指南](https://docs.nvidia.com/jetson/l4t/index.html#page/Tegra%20Linux%20Driver%20Package%20Development%20Guide/accelerated_gstreamer.html#wwpID0E0N20HA)  
[4] [嵌入式笔记](https://www.hackster.io/SaadTiwana/embedded-diaries-jetson-gstreamer-video-encoding-decoding-585ba7)


