---
title: yuv2rgb
date: 2022-06-29 11:32:43
tags:
    - C++
---

# YUV详解

### YUV分类
##### 1、按采样方式分类
   （1） YUV444: 全量UV，类似于RGB，每个像素点都有一套自己的YUV
![在这里插入图片描述](https://img-blog.csdnimg.cn/000b2132c524457583a034fcb2bd787c.png#pic_center)
   （2） YUV422: 50%的UV，与全量相比，UV数量减少一半，每行中由 2 个 Y 分量共用一套 UV 分量。
![在这里插入图片描述](https://img-blog.csdnimg.cn/eea18cfb6599473aab6374a6cc7b1fc2.png#pic_center)
    
（3）YUV420: 25%的UV， UV的数量减少到1/4，每行两个Y、每列两个Y，共由 4 个 Y 分量共用一套 UV 分量。![在这里插入图片描述](https://img-blog.csdnimg.cn/e01002399f09464e8916f4421fa5e8c0.png#pic_center)
##### 2、按内存中排列方式分类
（1）Planar YUV 三个分量分开存放
（2）Semi-Planar Y 分量单独存放，UV 分量交错存放
（3）Packed YUV 三个分量全部交错存放

以YUV420为例：

##### 3、ffmpeg中h264解码后的YUV格式

### YUV转RGB
色彩空间：
公式：

##### ffmpeg接口
##### cuda实现

参考：
[https://blog.csdn.net/yu540135101/article/details/107121769](https://blog.csdn.net/yu540135101/article/details/107121769)
[https://cloud.tencent.com/developer/article/1612357](https://cloud.tencent.com/developer/article/1612357)
[https://juejin.cn/post/6920848468797816846](https://juejin.cn/post/6920848468797816846)