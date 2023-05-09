---
title: 模板模式 
date: 2021-11-19 20:48:53
tags: 设计模式
categories: 设计模式
---


### 模板模式
**意图:** 定义一个操作中的算法的骨架，而将一些步骤延迟到子类中。模板方法使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。

**主要解决:** 一些方法通用，却在每一个子类都重新写了这一方法。

> **例子**
> 建造房子的流程都是一样的，比如：打地基->砌砖头->盖屋顶
> 但是不同种类的房子，比如茅草房和别墅，在这三个步骤中所要做的具体事情不一样
> 此时就可以应用模板模式，在接口类（基类）中抽象出统一的流程，在子类中再重写具体步骤的方法。

```cpp

```cpp
#include <iostream>
using namespace std ; 

class IGame{  // 接口类
public:
    virtual void initialize(){};
    virtual void startPlay(){};
    virtual void endPlay(){};

    //模板：固定不变的部分，定义统一的游戏流程
    void play(){
        initialize();
        startPlay();
        endPlay(); 
    }

    virtual ~IGame(){}
};

class Basketball:public IGame{  
public:  // 重写接口类中游戏流程中的具体方法
    virtual void initialize(){
        cout<<"Basketball initialize"<<endl; 
    };
    virtual void startPlay(){
        cout<<"Basketball startPlay"<<endl;
    };
    virtual void endPlay(){
        cout<<"Basketball endPlay"<<endl ; 
    };

    virtual ~Basketball(){}
};

class Football:public IGame{
public:  // 重写接口类中游戏流程中的具体方法
    virtual void initialize(){
        cout<<"Football initialize"<<endl; 
    };
    virtual void startPlay(){
        cout<<"Football startPlay"<<endl;
    };
    virtual void endPlay(){
        cout<<"Football endPlay"<<endl ; 
    };

    virtual ~Football(){}
};

int main(int argc, char *argv[])
{
    IGame *pb ;  
    Basketball basketball ; 
    Football Football ; 
    pb = &basketball ; 
    pb->play() ; 
    pb = &Football ; 
    pb->play() ; 
    return 0;
}
```
```
打印输出：

Basketball initialize
Basketball startPlay
Basketball endPlay
Football initialize
Football startPlay
Football endPlay
```


****
**总结：**
模板模式在类库的设计中很常见，在模板模式中，库设计者会给使用者提供固定的流程和需要重写的接口，使用者通常只需要：
1、继承父类；
2、重写某一个或某几个接口；
3、调用包含了所有流程的方法，比如`play()`
