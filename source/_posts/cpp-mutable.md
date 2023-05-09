---
title: cpp_mutable
date: 2022-07-06 20:11:59
tags:
    - CPP
---

### mutable 关键字
[mutabel 介绍](https://www.jianshu.com/p/b2883dbf3854)

#### 为什么std::mutex 前通常用mutable 修饰

在const成员函数中，对mutex的加锁和释放锁操作会违背const的不可变语义，所以，只能将mutex定义为mutable，从而可以在const修饰的函数中加锁，实现线程安全。

```cpp
#include <mutex>
#include <iostream>
class Cal {
public: 
    Cal(int n) {num = n;}

    void inc_num() {
        std::lock_guard<std::mutex> lg(m);
        ++num;
    }

    int get_num() const {  //const m 状态的改变会修改const 语义，但不报错？
        std::lock_guard<std::mutex> lg(m);
        return num;
    }
private:
    int num;
    mutable std::mutex m;
};

int main() {
    Cal c{0};
    std::cout << c.get_num() << std::endl;
}
```