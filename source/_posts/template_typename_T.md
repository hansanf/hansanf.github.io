---
title: 缺少模板参数列表
date: 2021-10-28 07:02:03
tag: C++
---


![在这里插入图片描述](https://img-blog.csdnimg.cn/da1fd3f3cf3244ecac3814f3e1ed0f0f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAcXFx55av5ZWm55av5ZWm,size_20,color_FFFFFF,t_70,g_se,x_16)

```cpp
#include <iostream>

template <typename T>
class  vector
{
private:
    /* data */
public:
     vector(/* args */);
    ~ vector();
    void push_back( T const& );
    void clear();
};

template <typename T>  // 不加这一行 会报错：“缺少模板参数列表”
void vector<T>::clear(){

}

int main(int argc, char *argv[])
{
    
    return 0;
}

```

- 参考 [https://blog.csdn.net/u013891092/article/details/51583666/](https://blog.csdn.net/u013891092/article/details/51583666/)
