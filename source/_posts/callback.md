---
title: callback
date: 2022-06-15 15:48:05
tags: 回调函数
categories: C++
---


## C++回调函数

### 函数指针
```cpp
void(* func)(int)
返回值类型(* 函数指针变量名)(参数)
```

### std::function
```cpp
std::function<void(int)>
             <返回值类型 (参数) >
```
### 常见用法

1、普通函数
```cpp
#include <iostream>
#include <functional>
void callback(int x){
    std::cout << "x = " << x << std::endl; 
}
// 函数指针
void register_func1(void(* cb)(int)){
    cb(100);
}

// std::function
using Callback = std::function<void(int)>;
void register_func2(Callback cb){
    cb(200);
}

int main(){
    register_func1(callback);
    register_func2(callback);
    return 0;
}
```

2、成员函数
```cpp
#include <iostream>
#include <functional>
class A{
public:
    void callback(int x){
        std::cout << "x = " << x << std::endl;
    }
};
// 函数指针
void register_func1(A a, void(A::*cb)(int)){
    (a.*cb)(100); // Note: 要用括号; 要带*
}

// std::function 
using Callback = std::function<void(int)>;
void register_func2(Callback cb){
    cb(200);
}

int main(){
    A a;
    register_func1(a, &A::callback); // 要传入两个参数

    auto cb = std::bind(&A::callback, &a, std::placeholders::_1); // 使用std::bind将对象地址绑定到this位置。placehoders::_1 待传入参数的占位符。
    register_func2(cb);
    return 0;
}
```

### Summary

1、函数指针是C中定义的实际函数的地址. std :: function是一个包装器,可以容纳任何类型的可调用对象(可以像函数一样使用的对象)

2、函数指针也可以和std::bind配合使用，但是std::function功能更强大，和std::bind配合更方便