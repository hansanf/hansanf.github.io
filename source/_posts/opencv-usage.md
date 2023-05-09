---
title: opencv_usage
date: 2023-02-14 15:02:59
tags:
    - opencv
---


### 保存图片

```
cv::Mat m(1080, 1920, CV_8UC3);
unsigned char* m_ptr = m.ptr<uchar>(0);
static int fgq_n = 0;
memcpy((void*)m_ptr, image_blob->cpu_data(), 1080 * 1920 * 3);
cv::imwrite("/home/caros/work/tmp/" + std::to_string(fgq_n++) + ".png", m);
```

