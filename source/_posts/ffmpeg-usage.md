---
title: ffmpeg-usage
date: 2022-09-02 11:11:25
tags:
    - ffmpeg
---

### ffmpeg 命令行
``` ffplay -f rawvideo -pixel_format bgr24 -video_size 1280x1280 -framerate 10 video_cuda_1.rgb ```
播放rgb格式的视频

```ffplay -f h264 -width 1920 -height 1080 record_424_sensor_ipcamera_h264_10_128_156_101.h264```
播放 h264格式的视频

```ffmpeg -i Free_Test_Data_15MB_MP4.h264 -framerate 30 -vcodec copy -f mp4 output.mp4```
转码 h264 => mp4
```ffmpeg -i Free_Test_Data_15MB_MP4.mp4 -vcodec libx264 -acodec aac Free_Test_Data_15MB_MP4.264 ```
转码 mp4 => h264

