---
title: const_cast用法
date: 2022-07-11 17:55:38
tags:
---

## const_cast
const_cast<>  <>必须是指针或引用

## const std::shared_ptr<const class_nam>去 const
```cpp
#include <iostream>
#include <memory>

struct A{
        int x = 0;
};

int main(){
        const std::shared_ptr<const A> const_ptr(new A());
        std::cout << typeid(const_ptr).name() << std::endl;

        //ptr->x = 100; //error
        //std::shared_ptr<A> ptr = const_cast<std::shared_ptr<A>>(const_ptr);  //error
        std::shared_ptr<const A> ptr = const_cast<std::shared_ptr<const A> &>(const_ptr); //必须是const_cast<&> 必须加引用

        A &a = const_cast<A &>(*const_ptr); // const_cast<&> 必须加引用
        a.x = 100;

        std::cout << "a.x = " << a.x << std::endl;
        std::cout << "const_ptr->x = " << const_ptr->x << std::endl;
        return 0;
}

```